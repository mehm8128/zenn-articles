---
title: "Buttonについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
これから 25 日間、1 人アドベントカレンダーとして React Aria を読んでいきます。
主に React Aria の提供している hooks について解説していくのですが、最初に軽く hooks 自体の説明をした後、内部実装を読んで気になったところの解説や疑問に思って調査した点の説明をしていこうと思います。内部実装は全てちゃんと読んでいるわけではなく、主に a11y 的な観点に絞って読んでいます。
また、解説とは書きましたが、解説というよりは自分の勉強メモのような感じになっていくと思います。

それでは今日は Button について書いていきます。

https://react-spectrum.adobe.com/react-aria/useButton.html

## `useButton` が提供しているもの

- マウスやタッチ、キーボードによるインタラクション
- ARIA を用いて、`button` タグ以外の HTML 要素もボタンにできる

## 使用例

ドキュメントからそのまま取ってきています。

```tsx
function Button(props) {
  let ref = useRef<HTMLButtonElement | null>(null);
  let { buttonProps } = useButton(props, ref);
  let { children } = props;

  return (
    <button {...buttonProps} ref={ref}>
      {children}
    </button>
  );
}
```

## 主な a11y 考慮事項

https://www.w3.org/WAI/ARIA/apg/patterns/button/

- `usePress`
- `aria-pressed`
- `isPending`

## いくつかピックアップ

### `usePress`

usePress では様々な a11y 対応がされているのですが、ブログ記事にまとめられています。
https://react-spectrum.adobe.com/blog/building-a-button-part-1.html

簡単に概要をまとめます。

主にタッチデバイスへの対応の話が書かれていて、マウスでできる操作がタッチイベントだと難しかったり、`:active`や`:hover`の動作がユーザーの期待と一致しない場合があって UX が悪くなりがちだったりといった問題が挙げられています。

そこで React Aria では[Pointer events API](https://developer.mozilla.org/ja/docs/Web/API/Pointer_events) を利用されています。この API はマウス、タッチ、ペンによる操作に対応していて、[`pointerType`](https://developer.mozilla.org/ja/docs/Web/API/PointerEvent/pointerType)プロパティによってどの機器によってイベントが発火されたのかも知ることができます。
その他`usePress`ではタッチキャンセルやテキスト選択、キーボード操作時にキーを押しっぱなしにすることによるイベントの複数発火防止などにも対応していて、`useButton`以外にもいくつかの hooks で用いられています。

実装を読みたい人はこちらから。実装を読むとかいうタイトルのアドベントカレンダーですが、僕は読んでいません。

https://github.com/adobe/react-spectrum/blob/main/packages/%40react-aria/interactions/src/usePress.ts

### `isPending`

こちらは hooks にはないのですが、React Aria Components にのみ含まれる props です。
データの送信中などにボタンを一時的に pending にしておくことができます。
`isPending=true`のときはボタンに`aria-disabled="true"`がつき、また、即座に ProgressBar（`role="progressbar`のもの）をアクセシビリティツリーに含める必要があります。

[https://react-spectrum.adobe.com/react-aria/Button.html#accessibility](https://react-spectrum.adobe.com/react-aria/Button.html#accessibility)

`isPending`中のラベルはここで設定されており、`progressbar`role はデフォルトでライブリージョンのように振る舞うので、`isPending`になった瞬間にこれらが読み上げられます。

https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Live_Regions#roles_with_implicit_live_region_attributes

### バグ修正

ソースコード読んでたら軽微なバグを見つけたので、修正 PR を出して無事マージされました。

https://github.com/adobe/react-spectrum/pull/7239

## まとめ

明日は Text Field の話です。お楽しみにー
