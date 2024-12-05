---
title: "Componentについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

:::message
この記事は [React Aria の実装読むぞ - Qiita Advent Calendar 2024](https://qiita.com/advent-calendar/2024/react-aria) の 1 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は Component について書いていきます。

https://react-spectrum.adobe.com/react-aria/useComponent.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx

```

## 本題

こちらが APG です。
https://www.w3.org/WAI/ARIA/apg/patterns/listbox/

###

unavailable な cell をクリックしたときに、最も近い日付が選択されてしまうの、あんまりよくなさそう

https://react-spectrum.adobe.com/react-aria/useCalendar.html#controlling-the-focused-date
最初フォーカスされてなくない？

useCalendarBase
role: 'application'
読み上げ。Announce when the visible date range changes

useCalendarGrid
Column headers are hidden to screen readers to make navigating with a touch screen reader easier
キーボード操作

useCalendarCell
anchorDate って何

## まとめ

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、 Text Field についての記事です。お楽しみにー
