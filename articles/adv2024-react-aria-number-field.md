---
title: "NumberFieldについて - React Ariaの実装読むぞ"
emoji: "🔢"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: true
---

:::message
この記事は [React Aria の実装読むぞ - Qiita Advent Calendar 2024](https://qiita.com/advent-calendar/2024/react-aria) の 5 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は NumberField について書いていきます。

https://react-spectrum.adobe.com/react-aria/useNumberField.html

## `useNumberField` とは

数値の入力欄を作るための hook で、様々な format や i18n をサポートしています。

## 使用例

ドキュメントからそのまま取ってきています。

```tsx
import { useNumberFieldState } from "react-stately";
import { useLocale, useNumberField } from "react-aria";

// Reuse the Button from your component library. See below for details.
import { Button } from "your-component-library";

function NumberField(props) {
  let { locale } = useLocale();
  let state = useNumberFieldState({ ...props, locale });
  let inputRef = React.useRef(null);
  let {
    labelProps,
    groupProps,
    inputProps,
    incrementButtonProps,
    decrementButtonProps,
  } = useNumberField(props, state, inputRef);

  return (
    <div>
      <label {...labelProps}>{props.label}</label>
      <div {...groupProps}>
        <Button {...decrementButtonProps}>-</Button>
        <input {...inputProps} ref={inputRef} />
        <Button {...incrementButtonProps}>+</Button>
      </div>
    </div>
  );
}
```

## 本題

APG はこちらです。

https://www.w3.org/WAI/ARIA/apg/patterns/spinbutton/

### role

`input`の`type`を`number`ではなくて`text`にしているので、`spinbutton`role ではなくて`textbox`role になっています。これは後述する色々なフォーマットに対応するためです。
その代わりに、要素の役割についてスクリーンリーダーの読み上げ用に補足説明を入れる[`aria-roledescription`](https://developer.mozilla.org/ja/docs/Web/Accessibility/ARIA/Roles/application_role#aria-roledescription)が利用されています。今回の場合、日本語では「数値フィールド」と読み上げられるようになっています。

https://github.com/adobe/react-spectrum/blob/b3a4d6c1134aae882aa1dcfce64efba1d8f4308d/packages/%40react-aria/numberfield/src/useNumberField.ts#L212

https://github.com/adobe/react-spectrum/blob/b3a4d6c1134aae882aa1dcfce64efba1d8f4308d/packages/%40react-aria/numberfield/src/useNumberField.ts#L234

また、`useSpinButton`という hooks から返される`spinButtonProps`によって`spinbutton`role に上書きすることも可能なのですが、React Aria の実装ではさらにそれを`role: null`で上書きしてデフォルトの`textbox`role にしています。これは、Voice Over 利用時に`spinbutton`role にフォーカスできなくなってしまっていることが理由らしいです。
ちなみに、+/-ボタンはキーボードの矢印キーでインクリメント・デクリメントの操作が可能なことから Tab フォーカスされないようになっています。

https://github.com/adobe/react-spectrum/blob/b3a4d6c1134aae882aa1dcfce64efba1d8f4308d/packages/%40react-aria/numberfield/src/useNumberField.ts#L231-L238

さらに、`spinbutton`role ではないので、`spinButtonProps`から返される[`aria-valuemax`](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-valuemax)などの`aria-`属性も`null`に上書きしています。

### 様々な format

ドキュメントにもあるように、小数点やパーセント表記、通貨、その他の単位のフォーマットがサポートされています。この変換を行ったり、+/-ボタンによるインクリメント・デクリメントをサポートしたりするために、`useNumberFieldState`という hook が用意されています。

https://github.com/adobe/react-spectrum/blob/main/packages/%40react-stately/numberfield/src/useNumberFieldState.ts

数値フィールド内の値は`numberValue`と`inputValue`という 2 つの state で管理されています。`numberValue`は内部で持つ用の`number`型の値、`inputValue`は表示用の`string`型の値で、後者は単位がついたりしているものです。
どちらも`useNumberFieldState`内で`useState`を用いて管理されています。`numberValue`は`useSpinButton`に渡されて`spinButtonProps`の`aria-valuenow`に用いられ、`inputValue`は`inputProps`として`useNumberField`から返されて`input`要素に渡されます。

### タッチパッド の `onWheel`

マウスホイールを上下に動かすことで increment/decrement ができますが、タッチパッドのスクロール操作（一般的に指 2 本でやるやつ）でももちろんできます。
しかし、タッチパッドだと上下以外にも横方向へのスクロールや、斜め方向へのスクロールもできるので、誤って操作してしまうのを防ぐために、横方向のスクロール（`e.deltaX`）が縦方向のスクロール（`e.deltaY`）より大きい場合は increment/decrement されないようになっています。

https://github.com/adobe/react-spectrum/blob/b0f15697245de74ebc99ab3d687f5eb3733d3a34/packages/%40react-aria/numberfield/src/useNumberField.ts#L134-L147

### `inputMode`

昨日紹介した `inputMode` です。
今回は数値に特化しているので hook 内部で指定しています。
しかし、ブラウザや OS によって同じ `inputMode` でも表示されるキーボードが異なってしまうので、負の値を許容する NumberField のときは`-`ボタンが表示されるキーボードを表示する、とかをちゃんと条件分岐して設定しています。

具体的に説明します。
iPhone では`inputMode`が`numeric`でも`decimal`でも`-`ボタンが表示されないので、負の値を許容する場合は`inputMode`を`text`にします。
Android では`inputMode`が`numeric`の場合に`-`ボタンがあり、`decimal`のときにはないので、負の値を許容する場合は`inputMode`を`numeric`にします。

細かいところですがしっかり各端末の挙動が調査されて適切な設定がされていることが分かります。

https://github.com/adobe/react-spectrum/blob/b0f15697245de74ebc99ab3d687f5eb3733d3a34/packages/%40react-aria/numberfield/src/useNumberField.ts#L152-L176

公式のブログ記事にもまとめられていました（`Mobile`セクション以外は i18n の回で解説予定です）。

https://react-spectrum.adobe.com/blog/how-we-internationalized-our-numberfield.html#mobile

## まとめ

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、 Radio と Checkbox についての記事です。お楽しみにー
