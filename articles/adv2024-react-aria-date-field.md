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

> Rather than wrapping a Date object and providing an API on top, it implements all date arithmetic and utilities from scratch

CalendarDate オブジェクト
Temporal に inspire されてる

intl.datetimeformat は複数の暦をサポートしてるけど、js の Date オブジェクトはグレゴリオ暦のみサポートしてる
つまり、別の暦で表示すると正しく表示されない
よって、i18ned/date オブジェクトでいい感じに別の暦に変換できるようにしてる
その他一週間が何曜日に終わるかとか何曜日が休日かとか、タイムゾーンとかサマータイムとかの面倒とかも見てくれる
format 部分は intl を wrap したもの

### `useDateSegment`

Only apply aria-describedby to the first segment
->冗長な読み上げにならないように
enterkeyhint
->スマホのときに右下に表示されるボタンのテキスト
https://developer.mozilla.org/ja/docs/Web/HTML/Global_attributes/enterkeyhint
列挙型なことに注意（好きなのを入れられるわけではない）

### date picker で使うときの注意点みたいな

If within a date picker or date range picker, the date field will have role="presentation"
When used within a date picker or date range picker, the field gets role="presentation"

`descProps`: 選択した日付 : 2024 年 12 月 18 日
`fieldProps`: 普通にフィールドの説明文

date picker のときはさらに外側に、date field と calendar を両方合わせた group があるからついてないって話っぽそう
describedby は`fieldProps['aria-describedby']`で既に date picker 側で選択した日付もつけてくれてるから、それ単体で OK

## まとめ

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、 Calendar についての記事です。お楽しみにー
