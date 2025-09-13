---
title: "Web フレームワークとの融合・周辺ライブラリ - Web Components a11y 1人 Advent Calendar"
emoji: "🦴"
type: "tech"
topics: ["frontend", "webcomponents", "lit", "react", "vue"]
published: false
---

:::message
この記事は [Web Components a11y 1 人 Advent Calendar - Qiita Advent Calendar 2025](https://qiita.com/advent-calendar/2025/web-components-a11y) の n 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。

今回は Web Components に関連する周辺ライブラリと、Web フレームワークとの融合について紹介します。

Web Components の利点の 1 つとして、標準の機能を用いてコンポーネントを作成できるということで、フレームワークに依存せずに UI のパーツを作ることができるという点が挙げられます。これによりデザインシステムなど複数の Web フレームワークで使われる可能性のあるコンポーネントを作るときに、それぞれの Web フレームワークを用いてコンポーネントを作る手間が省けます。

そこで、Web Components を Web フレームワークで用いる方法や、逆に Web フレームワークを用いて作成したコンポーネントを Web Components に変換する方法を紹介します。方法だけでなく、そのときに起こる・過去に起こっていた問題やそれに対する解決法なども見ていきます。

## 周辺ライブラリ

### Lit

開発をどう効率化させているのか
これに依存することは問題ないのか (https://www.tonyward.dev/articles/web-components-design-system の"Whoa whoa whoa, Tony,")

ほとんどのデザインシステムで使われてるとかも

https://github.com/stenciljs/core

サンプルコード

### その他周辺ライブラリ

- lint
  - https://github.com/open-wc/open-wc/blob/master/docs/docs/linting/eslint-plugin-lit-a11y/overview.md
- cem
  - https://github.com/webcomponents/custom-elements-manifest
- lsp
  - https://wc-toolkit.com/integrations/web-components-language-server/
- test
  - https://modern-web.dev/guides/test-runner/getting-started/

## Web フレームワークとの融合

### React

### Vue

### Svelte

### Solid

### Astro

## まとめ

明日は Web Components 開発を便利している周辺ライブラリについて紹介します。
