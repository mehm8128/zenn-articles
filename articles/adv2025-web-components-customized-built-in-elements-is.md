---
title: "カスタム組み込み要素 `is`の課題 - Web Components a11y 1人 Advent Calendar"
emoji: "🧑‍🔧"
type: "tech"
topics: ["frontend", "webcomponents", "a11y", "html"]
published: false
---

:::message
この記事は [Web Components a11y 1 人 Advent Calendar - Qiita Advent Calendar 2025](https://qiita.com/advent-calendar/2025/web-components-a11y) の n 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。

## is

https://developer.mozilla.org/ja/docs/Web/HTML/Reference/Global_attributes/is

#### モチベーション

https://github.com/MicrosoftEdge/MSEdgeExplainers/blob/main/ElementInternalsType/explainer.md

`<p is="word-count"></p>`みたいに書くと、カスタム要素の定義側で`HTMLParagraphElement`を extends して定義でき、動作やセマンティクスを継承しながらカスタム要素を作成することができる
"customized built-in elements"

明示的に属性を受け取る設定をしなくても、そのまま渡せるようになるのがメリット
→ これあったら Reference Target のやつも解決するはず（は Reference Target の方に書く）

例

- button 要素の popover invoker
- button 要素の type="submit"や"reset"
- label 要素のラベル付け

progressive enhancement もある

#### 問題点

- shadow tree が attach できなかった
- `HTMLHeadingElement`が全部の level をまとめていたり、`<input>`の type を指定できない
- 3 回同じようなことを書く必要がある
- https://github.com/WICG/webcomponents/issues/509#issuecomment-222860736
  - local name はカスタム要素の名前？
- https://github.com/WICG/webcomponents/issues/509#issuecomment-230700060
  - 親クラスに機能が追加されたときに、サブクラスでもサポートしないといけなくなる
- `<custom-element>`の場合と`<a is="custom-element">`の場合が混在して読みづらい

https://github.com/WebKit/standards-positions/issues/97
https://github.com/w3c/tpac2023-breakouts/issues/44
https://bugs.webkit.org/show_bug.cgi

### custom attributes

https://github.com/w3c/tpac2023-breakouts/issues/44
で代替案として提案された

1000 番を基にして ↑ で話し合われ、1029 が誕生

https://eisenbergeffect.medium.com/2023-state-of-web-components-c8feb21d4f16#a31c
https://github.com/WICG/webcomponents/issues/1000
https://github.com/WICG/webcomponents/issues/1029

https://github.com/lume/custom-attributes

#### custom enhancement

`customEnhancement.define()`で定義して、HTML 要素につけると enhance した機能が付与される
`allowedCSSMatches`などでつけられる要素を制限可能

https://github.com/WICG/webcomponents/issues/1000

#### custom attributes

カスタム要素みたいな感じでカスタム属性を作って、それを HTML 要素やカスタム要素に適用することで好きな動作を当てることができる
現状まだ要素に対して属性を当てる方法がないので、popovertarget とか commandfor とかもその属性を当てられるようにする（MyInput.attributeRegistry.define()みたいに？）

`HTMLElement.attributeRegistry.define("sc-list", ListAttribute)`みたいに custom attributes を追加するため、`HTMLElement`自体に追加してどの要素でも使えるようにすることもできるし、特定の HTML 要素や、カスタム要素に限定して使えるようにすることもできる

[element-behaviors](https://github.com/lume/element-behaviors) では has に behavior の id を渡せるようにしていたけど、カスタム属性ではそれぞれ属性として渡せるようになっている

ユースケースとして

- Popover API や Invokers 機能をつける
  - 上で書いてるように、用意してある custom attributes を登録する感じ？
- `<time>`に`format`属性をつけてそのフォーマットで表示されるように
- `<p loading-placeholder="3 sentences">`のスケルトン表示
- `<audio start-at="0:05">`の開始時刻指定

## まとめ

明日はについて紹介します。
