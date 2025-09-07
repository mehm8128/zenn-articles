---
title: "フォーカス制御 - Web Components a11y 1人 Advent Calendar"
emoji: "🔍"
type: "tech"
topics: ["frontend", "webcomponents", "a11y"]
published: false
---

:::message
この記事は [Web Components a11y 1 人 Advent Calendar - Qiita Advent Calendar 2025](https://qiita.com/advent-calendar/2025/web-components-a11y) の n 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。

フォーカス制御は、スクリーンリーダーユーザーを含め、キーボードを用いてブラウザを操作するユーザーが Web コンテンツにアクセス可能にするために必須のものです。また、`<label>`要素をクリックすると、紐づいている`<input>`要素などにフォーカスが当たりそのまま入力可能になるなど、マウスユーザー以外にもフォーカス制御が有用な場面があります。
今回は、Web Components におけるフォーカス制御について、2 つの proposal を見ながら紹介します。

## `delegatesFocus`

デザインシステムで label のフォーカスがこれ使えばいけた話とか
https://github.com/WICG/webcomponents/blob/gh-pages/proposals/ShadowRoot-delegatesFocus-Proposal.md

## Default focus behaviors for custom elements

https://github.com/WICG/webcomponents/blob/gh-pages/proposals/custom-element-default-focus.md

## まとめ

明日はイベント制御について紹介します。
