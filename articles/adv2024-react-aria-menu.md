---
title: "Menuについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

:::message
この記事は [React Aria の実装読むぞ - Qiita Advent Calendar 2024](https://qiita.com/advent-calendar/2024/react-aria) の 1 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は Menu について書いていきます。

https://react-spectrum.adobe.com/react-aria/useMenu.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx

```

## 本題

WAI-ARIA はこちらです。
https://www.w3.org/WAI/ARIA/apg/patterns/menubar/

### キーボード操作

大体 listbox とかと同じ

### サブメニュー

https://react-spectrum.adobe.com/react-aria/Menu.html のコンポーネントの方参照するのがよさそう

フォーカス移動、useSafelyMouseToSubmenu など
state の組み合わせ方は RAC を読む

safepolygon
https://floating-ui.com/docs/useHover#safepolygon

https://react-spectrum.adobe.com/blog/creating-a-pointer-friendly-submenu-experience.html

## まとめ

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、 Text Field についての記事です。お楽しみにー
