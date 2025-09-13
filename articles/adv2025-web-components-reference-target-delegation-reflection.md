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

shadowrootdelegatesariaattributes や shadowrootreflectsariacontrols、reflectariacontrols でつなげる
問題点としては、同じ aria-属性を複数の要素に別の値で紐づけることができないこと
ただし、カスタム要素は atomic であることが多いので、問題にならない説もある（つまり、問題になる場合は大体カスタム要素自体を複数に分けられる可能性がある）

## まとめ

明日はについて紹介します。
