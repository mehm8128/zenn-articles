---
title: "ElementInternalsによるHTMLフォームへの参加 - Web Components a11y 1人 Advent Calendar"
emoji: ""
type: "tech"
topics: ["frontend", "webcomponents", "a11y"]
published: false
---

:::message
この記事は [Web Components a11y 1 人 Advent Calendar - Qiita Advent Calendar 2025](https://qiita.com/advent-calendar/2025/web-components-a11y) の n 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。

## ElementInternals とは

https://developer.mozilla.org/ja/docs/Web/API/ElementInternals
https://webkit.org/blog/13711/elementinternals-and-form-associated-custom-elements/
https://web.dev/articles/more-capable-form-controls?hl=ja#defining_a_form-associated_custom_element

単純にフォーム検証とかができるようになるのに加えて、a11y 的なメリットがあったり、state を持てるようになったり、別の記事で ElementInternals.type が検討されていたり、色々と内部状態を扱うことができるようになった

## できること

### state

https://developer.mozilla.org/ja/docs/Web/API/ElementInternals/states

CSS の記事で`:state()`を解説してる前提

### ARIA

https://developer.mozilla.org/ja/docs/Web/API/ElementInternals#aria_%E3%81%8B%E3%82%89%E5%8F%96%E3%82%8A%E8%BE%BC%E3%81%BE%E3%82%8C%E3%81%9F%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%83%B3%E3%82%B9%E3%83%97%E3%83%AD%E3%83%91%E3%83%86%E3%82%A3

今までの問題点も：属性が消されちゃったときにデフォルト値が消えちゃうとか
https://github.com/sakupi01/sakupi01.com/blob/main/apps/studio.sakupi01.com/whatwg/element-internals/index.html

### バリデーション

https://developer.mozilla.org/ja/docs/Web/API/ElementInternals#%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%83%B3%E3%82%B9%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89

## まとめ

明日はについて紹介します。
