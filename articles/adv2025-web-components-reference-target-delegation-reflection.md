---
title: "Reference Target Cross-root ARIA Reflection/Delegation - Web Components a11y 1人 Advent Calendar"
emoji: "🎯"
type: "tech"
topics: ["frontend", "webcomponents", "a11y", "waiaria"]
published: false
---

:::message
この記事は [Web Components a11y 1 人 Advent Calendar - Qiita Advent Calendar 2025](https://qiita.com/advent-calendar/2025/web-components-a11y) の n 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。

https://github.com/alice/aom/blob/gh-pages/semantic-delegate.md
https://github.com/Westbrook/cross-root-aria-reflection/blob/main/cross-root-aria-reflection.md
https://github.com/leobalter/cross-root-aria-delegation/blob/main/explainer.md

shadowrootdelegatesariaattributes や shadowrootreflectsariacontrols、reflectariacontrols でつなげる
問題点としては、同じ aria-属性を複数の要素に別の値で紐づけることができないこと
ただし、カスタム要素は atomic であることが多いので、問題にならない説もある（つまり、問題になる場合は大体カスタム要素自体を複数に分けられる可能性がある）

## delegation

shadowDOM 内の input に、shadowDOM 外の label の値を aria-labelledby で渡したい

`delegatedariaattributes="aria-label aria-describedby"`
カスタム要素に渡された aria-属性を delegate して、`delegatedariaattributes="aria-label"`を持つ要素に渡す

スクリーンリーダーが 2 回読む可能性があるので、いい感じに shadow host の方が無視されるようにしたい
→ デフォルトロールが none になった話に繋がる

複数の要素に同じ名前で別の値を持つ aria-属性をつけたい場合に対応できない
ユースケースがあるかは不明（カスタム要素は atomic な場合が多いので～みたいな話）

## reflection

shadowDOM 外の input から、shadowDOM 内の label の値を aria-labelledby で参照したい

方法 1
カスタム要素を id で参照されたときに、参照された属性によって内部で参照する要素を変える
`shadowrootreflectsariaattributes="aria-controls aria-activedescendant"`
`aria-controls`から参照されたら、`reflectedariaattributes="aria-controls"`を持つ要素が参照される、とか

方法 2
CSS の`::part()`で同様に参照されるようにする
`part="aria-controls"`
ただ、これは CSS 用のものなので新しく`exportfor`を生やすとか、exportparts みたいに exportids を生やすとか（exportparts はネストされた ShadowDOM のときに動かせるようにするためのもの）（多分ここで exportids が初登場）
中身の id を知っておかないといけない・カスタム要素を使うたびに毎回マッピングを書かないといけない・ネストされたらさらに複雑になるなど問題は多い

よく分からんかった ↓ reflect と auto の違いとか
https://github.com/Westbrook/cross-root-aria-reflection/blob/main/cross-root-aria-reflection.md

## 上記の 2 つが複合したケース

上 2 つの方法で対処できるはず？

## 問題

label の for や popover api、command for などの ref にも対応するかどうか

## semantic-delegate

https://github.com/alice/aom/blob/gh-pages/semantic-delegate.md

組み込みの input などの要素を wrap して、独自の機能を追加したコンポーネントを作るときのことを考える（semantic delegate pattern）

shadow host の「代役」としてセマンティクスな役割を子孫要素に delegate するような API
`this.shadowRoot.semanticDelegateElement = input`
これによって、delegate した要素につけることができる属性は全て shadow host につけることができるようになる

宣言的な書き方にも対応する: `shadowrootsemanticdelegate="actualinput"`

1 つの要素だけを wrap する場合においては、cross-root ARIA delegation/reflection のショートハンドになりうる

## まとめ

明日はについて紹介します。
