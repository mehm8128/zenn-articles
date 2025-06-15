---
title: "a11y 上の理由で Deprecated になった HTML と ARIA まとめ"
emoji: "🗑️"
type: "tech"
topics: ["frontend", "a11y", "html", "aria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今回は敢えて、a11y 上の理由から Deprecated になった HTML と ARIA をまとめてみようという記事です。

## Deprecated の定義

https://github.com/mdn/browser-compat-data/blob/main/docs/data-guidelines/index.md#setting-deprecated

a11y 以外の理由だとどんなのがあるか

- 例えば HTML じゃなくて CSS でスタイルつけようねとか
- セキュリティの問題
- 他の要素で代替可能
- ブラウザ間の互換性の問題

https://chatgpt.com/c/684e44b4-7aac-8002-a9d1-c889e0a6fe12

## Deprecated な HTML

https://medium.com/@sumudusiriwardana/the-accessibility-story-behind-deprecated-html-elements-98396a877893
https://html.spec.whatwg.org/multipage/obsolete.html#non-conforming-features

### `marquee`

https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/marquee

### `blink`

https://developer.mozilla.org/en-US/docs/Glossary/blink_element
https://udn.realityripple.com/docs/Web/HTML/Element/blink

### `bgsound`

https://udn.realityripple.com/docs/Web/HTML/Element/bgsound

### `frameset`, `frame`, `noframes`

https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/frame
https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/frameset
https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/noframes

### `dir`

https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/dir

### `plaintext`, `xmp`

https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/plaintext
https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/xmp

## Deprecated な ARIA

### `directory` role

https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Roles/directory_role

### `aria-dropeffect`

https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-dropeffect

### `aria-grabbed`

https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-grabbed

## 静的解析

https://markuplint.dev/docs/rules/deprecated-attr
https://markuplint.dev/docs/rules/deprecated-element

## まとめ
