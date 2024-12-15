---
title: "DateFieldについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

:::message
この記事は [React Aria の実装読むぞ - Qiita Advent Calendar 2024](https://qiita.com/advent-calendar/2024/react-aria) の 22 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は DateField について書いていきます。

https://react-spectrum.adobe.com/react-aria/useDateField.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx

```

## 本題

APG はこちらです。
https://www.w3.org/WAI/ARIA/apg/patterns/listbox/

### i18n

i18 記事出したあとになるので書く
https://react-spectrum.adobe.com/blog/date-and-time-pickers-for-all.html

### `useDateSegment`

`backspace`関数気になる
Only apply aria-describedby to the first segment
contenteditable
enterkeyhint
他に読み上げ関連のとかあれば

### date picker で使うときの注意点みたいな

If within a date picker or date range picker, the date field will have role="presentation"
When used within a date picker or date range picker, the field gets role="presentation"

## まとめ

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、 Calendar についての記事です。お楽しみにー
