---
title: "カスタム組み込み要素 まとめ - Web Components a11y 1人 Advent Calendar"
emoji: "🧑‍🔧"
type: "tech"
topics: ["frontend", "webcomponents", "a11y", "html"]
published: false
---

:::message
この記事は [Web Components a11y 1 人 Advent Calendar - Qiita Advent Calendar 2025](https://qiita.com/advent-calendar/2025/web-components-a11y) の n 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。

## wrapup

### feature decomposition

https://github.com/MicrosoftEdge/MSEdgeExplainers/pull/1153

`static buttonActivationBehaviors = true`みたいにして細かく分解された機能を使えるようにするやつ

`buttonActivationBehaviors`や`labelBehaviors`のような粒度だが、前回書いたように複雑になって矛盾や曖昧さが発生したり、`canUsePopoverTarget`のような粒度だとアクセシビリティやセマンティクス、フォーカス可能性、イベント処理などのようなものも結局入れないといけなくて、メリットが少ない

実用的というより理論的と書かれているように、こういう方法もありそう、くらいのものっぽい

### まだ新しく提案されてるやつ

https://github.com/WICG/webcomponents/issues/1108

基本的には、カスタム要素を定義するときに HTMLButtonElement などを extends すればそのまま使えるようにしたいという話
使うときは define したカスタム要素の名前で利用`<custom-button></custom-button>`
`type="number"`とかは「How to subclass specific types of elements?」に書いてある感じにする

DOM を追加したい場合
（多分カスタム要素の template 的なやつで）親クラスの（拡張元の）要素が入るところに`<slot type="supershadow">`を追加すると、attachShadow したときに initialize される

`type="supershadow` は shadowroot が配置され、`type="super"` は shadowhost が配置される

```
<slot name="prefix"></slot>
<slot type="supershadow"></slot>
<slot name="suffix"></slot>
```

↓

```
<slot name="prefix"></slot>
{buttonのshadowroot？ってなんだ}
<slot name="suffix"></slot>
```

```
<label><slot name="label">{{ attrs.label }}</slot>
  <slot type="super"></slot>
</label>
<slot name="hint"></slot>
```

↓

```
<label><slot name="label">{{ attrs.label }}</slot>
  <input value="aaa" type="text">
</label>
<slot name="hint"></slot>
```

### いい感じにまとめる

## まとめ

明日はについて紹介します。
