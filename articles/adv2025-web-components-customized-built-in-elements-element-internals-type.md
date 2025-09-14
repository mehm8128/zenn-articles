---
title: "カスタム組み込み要素 ElementInternals.type - Web Components a11y 1人 Advent Calendar"
emoji: "🧑‍🔧"
type: "tech"
topics: ["frontend", "webcomponents", "a11y", "html"]
published: false
---

:::message
この記事は [Web Components a11y 1 人 Advent Calendar - Qiita Advent Calendar 2025](https://qiita.com/advent-calendar/2025/web-components-a11y) の n 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。

## ElementInternals.type

### explainer

https://github.com/whatwg/html/issues/11061
https://github.com/MicrosoftEdge/MSEdgeExplainers/blob/cc33ef36e04c2e6c6c4c37ce54a2e01a9c1696e2/ElementInternalsType/explainer.md

`ElementInternals.type`に以下のような値を指定すると、カスタム要素にそれらの動作を追加できるようになる

- `button`: `<button type="button">`のような動作。例えば `disabled` や `popoptarget`, `commandfor` など
- `submit`: `<button type="submit">`のような動作
- `reset`: `<button type="reset">`のような動作
- `label`: `<label>`のような動作。例えば`<input>`との紐づけなど

button と label の例を載せる。label は accessible name つけられることも

動作だけで、概観は変化しない
ElementInternals なので、`formAssociated = true`である必要がある

全体的に賛成が得られている
https://github.com/openui/open-ui/issues/1088

機能を分解して開発者が組み合わせられるようにするという案もあったけど、組み合わせによっては矛盾が発生したり、popoverTarget はサポートするけど disabled はサポートしないみたいなボタンを作ってしまうおそれがあるので、`elementInternals.type`としてひとまとまりで提供する方がいいという流れに
https://github.com/whatwg/html/issues/11061#issuecomment-2675779659

### BehavesLike などその他の案

https://github.com/MicrosoftEdge/MSEdgeExplainers/blob/main/ElementInternalsType/explainer.md
https://github.com/MicrosoftEdge/MSEdgeExplainers/pull/1130

ElementInternals.type では以下の問題が会った

- 指定できるものに、button や label などの HTML タグ名と、submit や reset などの button の type 属性の値が混ざっていた
  - 一旦前者の HTML タグ名のみサポートするように
- attachInternals()した後で設定され、いつどこで設定されるのかが明確でなかった
  - static な property として設定するように
- 1 回しか設定できないのに、命令的なのが分かりづらい
  - static な property として宣言的に設定するように

`static behavesLike = 'button'`プロパティを追加することで追加する機能を指定する
今回は`button`と`label`だけ

`elementInternals.buttonMixin`で`.disabled`や`.popoverTargetElement`、`.commandForElement`などにアクセスできる
label も同様で、`elementInternals.labelMixin.htmlFor`などができる

## まとめ

明日はについて紹介します。
