---
title: "ColorPickerについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

:::message
この記事は [React Aria の実装読むぞ - Qiita Advent Calendar 2024](https://qiita.com/advent-calendar/2024/react-aria) の 15 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は ColorPicker について書いていきます。

https://react-spectrum.adobe.com/react-aria/useColorPicker.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx

```

## 本題

https://www.w3.org/WAI/ARIA/apg/patterns/slider/

読み上げの話メインになりそう
あとは

https://react-spectrum.adobe.com/blog/accessible-color-descriptions.html
getAriaValueTextForChannel

コンポーネントが追加された経緯
https://github.com/adobe/react-spectrum/pull/6199

## まとめ

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、TabList についての記事です。お楽しみにー
