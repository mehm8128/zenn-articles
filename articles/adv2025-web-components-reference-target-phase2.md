---
title: "Reference Target Phase2 - Web Components a11y 1人 Advent Calendar"
emoji: "🎯"
type: "tech"
topics: ["frontend", "webcomponents", "a11y", "waiaria"]
published: false
---

:::message
この記事は [Web Components a11y 1 人 Advent Calendar - Qiita Advent Calendar 2025](https://qiita.com/advent-calendar/2025/web-components-a11y) の n 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。

## phase2

phase1 は 1 つの要素を参照するためのものだったけど、複数の要素をターゲットとして、かつコンポーネントを使うときにそれを指定できるようにする

`shadowRootReferenceTargetMap="aria-attr: inner-id"`で、この属性から参照されたらこの要素を参照する、というマッピングができる
shadowrootreferencetarget と shadowrootreferencetargetmap を一緒に使ったら、後者で指定されている属性からの参照は後者に、指定されていなければ前者への参照となり、前者がフォールバック的に働く
→ 前者いる？消すことはできる
比較のセクションに書いてある

複数要素への delegate
`shadowrootreferencetargetmap=aria-describedby: message tooltip`のようにスペース区切りにすることで、`aria-describedby="message tooltip"`のように書くと同じように、対象の要素の text alternatives を合体させて参照することができるようになる

label 要素の for は htmlFor として扱える
→ じゃあ commandfor とかは？

## 比較

cross-root ARIA reflection

exportid

## 実装例

## まとめ

明日はについて紹介します。
