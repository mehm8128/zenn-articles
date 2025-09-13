---
title: "Reference Target Phase1 - Web Components a11y 1人 Advent Calendar"
emoji: "🎯"
type: "tech"
topics: ["frontend", "webcomponents", "a11y", "waiaria"]
published: false
---

:::message
この記事は [Web Components a11y 1 人 Advent Calendar - Qiita Advent Calendar 2025](https://qiita.com/advent-calendar/2025/web-components-a11y) の n 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。

## Reference Target の概要

Chrome と Edge では label 要素による紐づけ以外使える状態
Firefox と Safari はまだ
https://wpt.fyi/results/shadow-dom/reference-target/tentative?label=experimental&label=master&aligned

goals

non goals

- Allow attributes on the host to be "forwarded" to the enclosed element.
  - For example, to allow role or aria-label on the host to be applied to the enclosed element.
  - semantic delegate 的な？
- Straightforward form association for enclosed form-associated elements.
  - 分からん
- Provide a serializable way to create references from elements in shadow DOM to elements in light DOM.
  - IDL reflection を宣言的に使えるようにしたい

phase が 2 つに分かれていて、今回は phase1 のみ紹介することの説明

## phase1

ShadowDOM 内の要素の id を指定するもの。aria-label などのサポートはしない

attachShadow 時に`referenceTarget: "input"`するか、HTML で`shadowrootreferencetarget="input"`

commandfor や label の for などもサポートできる
CSS では shadowhost を参照する

label でネストしたり for に指定したりすると、ターゲットしている input に名前がついてくれる

IDL reflection では shadowhost を返す

イベントの話
https://github.com/WICG/webcomponents/issues/1098

## まとめ

明日はについて紹介します。
