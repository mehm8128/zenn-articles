---
title: "NumberFieldについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は NumberField について書いていきます。

https://react-spectrum.adobe.com/react-aria/useNumberField.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx

```

## 主な a11y 考慮事項

https://www.w3.org/WAI/ARIA/apg/patterns/spinbutton/

- role
- 様々な format

## いくつかピックアップ

### role

`input`の`type`を`number`ではなくて`text`にしているので、`spinbutton`role ではなくて`textbox`role になっています。これは後述する色々なフォーマットに対応するためです。
その代わりに、要素の役割についてスクリーンリーダーの読み上げ用に補足説明を入れる[`aria-roledescription`](https://developer.mozilla.org/ja/docs/Web/Accessibility/ARIA/Roles/application_role#aria-roledescription)が利用されています。今回の場合、日本語では「数値フィールド」と読み上げられるようになっています。

https://github.com/adobe/react-spectrum/blob/b3a4d6c1134aae882aa1dcfce64efba1d8f4308d/packages/%40react-aria/numberfield/src/useNumberField.ts#L212

https://github.com/adobe/react-spectrum/blob/b3a4d6c1134aae882aa1dcfce64efba1d8f4308d/packages/%40react-aria/numberfield/src/useNumberField.ts#L234

また、`useSpinButton`という hooks から返される`spinButtonProps`によって`spinbutton`role に上書きすることも可能なのですが、React Aria の実装ではさらにそれを`role: null`で上書きしてデフォルトの`textbox`role にしています。これは、Voice Over 利用時に`spinbutton`role にフォーカスできなくなってしまっていることが理由らしいです。
ちなみに、+/-ボタンはキーボードの矢印キーでインクリメント・デクリメントの操作が可能なことから Tab フォーカスされないようになっています。

https://github.com/adobe/react-spectrum/blob/b3a4d6c1134aae882aa1dcfce64efba1d8f4308d/packages/%40react-aria/numberfield/src/useNumberField.ts#L231-L238

さらに、`spinbutton`role ではないので、`spinButtonProps`から返される`aria-valuemax`などの`aria-`属性も`null`に上書きしています。

https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-valuemax

### 様々な format

ドキュメントにもあるように、小数点やパーセント表記、通貨、その他の単位のフォーマットがサポートされています。この変換を行ったり、+/-ボタンによるインクリメント・デクリメントをサポートしたりするために、`useNumberFieldState`という hook が提供されています。

https://github.com/adobe/react-spectrum/blob/main/packages/%40react-stately/numberfield/src/useNumberFieldState.ts

数値フィールド内の値は`numberValue`と`inputValue`という 2 つの state で管理されています。`numberValue`は内部で持つ用の`number`型の値、`inputValue`は表示用の`string`型の値で、後者は単位がついたりしているものです。
どちらも`useNumberFieldState`内で`useState`を用いて管理されています。`numberValue`は`useSpinButton`に渡されて`spinButtonProps`の`aria-valuenow`に用いられ、`inputValue`は`inputProps`として`useNumberField`から返されて`input`要素に渡されます。

#### label

Determine the label for the increment and decrement buttons.を頑張って読む

name プロパティは使えないですよ。number 型を送信したいので、そのままじゃなくて hidden の input を別で用意する
https://github.com/adobe/react-spectrum/issues/2745

まだ読んでない
https://github.com/adobe/react-spectrum/issues/5474

## まとめ

明日は の話です。お楽しみにー
