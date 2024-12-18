---
title: "Calendarについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

:::message
この記事は [React Aria の実装読むぞ - Qiita Advent Calendar 2024](https://qiita.com/advent-calendar/2024/react-aria) の 23 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は Calendar について書いていきます。

https://react-spectrum.adobe.com/react-aria/useCalendar.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx

```

## 本題

APG はこちらです。
https://www.w3.org/WAI/ARIA/apg/patterns/listbox/

###

i18n
range は intl の range 使ってる
あとは date field で書いた通り

useCalendarBase
読み上げ。Announce when the visible date range changes
そのまま。selectedDateDescription が変わったときに読み上げる

useCalendarGrid
Column headers are hidden to screen readers to make navigating with a touch screen reader easier
そのまま。ヘッダー（曜日部分）はスクリーンリーダーによる読み上げをスキップして、日付部分にフォーカスするごとに曜日が読み上げられるようになってる

useCalendarCell
anchorDate って何
->range calendar のときの range の開始日時
drag の処理とか色々してるので、できればもうちょっと見てみる

range
「クリックして日付範囲の選択を開始」「クリックして日付範囲の選択を終了」の読み上げ。aria-description につけることで、フォーカス時に読み上げ
スクリーンリーダーユーザー向けの読み上げなのに、「クリックして」でいいのか？
https://github.com/adobe/react-spectrum/blob/adae13c78e7085df4b2d39817def75f35df4f6c9/packages/%40react-aria/calendar/src/useCalendarCell.ts#L145-L156

## まとめ

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、 DnD についての記事です。お楽しみにー
