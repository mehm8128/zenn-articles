---
title: "色とコントラスト比 - UIライブラリ a11y"
emoji: "🎨"
type: "tech"
topics: ["frontend", "a11y", "wcag", "color"]
published: false
---

:::message
この記事は [UI ライブラリ a11y - Qiita Advent Calendar 2025](https://qiita.com/advent-calendar/2025/ui-library-a11y) の 2 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。今回は色とコントラスト比について紹介していきます。
色に関係のある WCAG の SC をいくつか紹介したあと、色空間やダークモードについて見ていき、最後にコントラスト比の話をします。

## WCAG で関係ありそうなやつ

https://www.w3.org/WAI/WCAG22/Understanding/use-of-color.html
https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html
https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.
https://www.w3.org/WAI/WCAG22/Understanding/contrast-enhanced.html

## 色空間

色空間とはから OKLCH まで軽くまとめる

https://evilmartians.com/chronicles/exploring-the-oklch-ecosystem-and-its-tools

## darkmode

a11y の文脈でのダークモードの必要性や UI ライブラリでの対応方法について

https://accessible-usable.net/2022/02/entry_220201.html
https://accessibility-tech.blogspot.com/2019/06/ui.html

## コントラスト比

基本的なコントラスト比の a11y 上の説明や、auto contrast の話など
コントラスト比チェックツールの紹介
色の組み合わせ抽出してチェックしてくれるツールも紹介したい

https://mantine.dev/theming/theme-object/#autocontrast: この前あった safari のあれを紹介
https://mui.com/material-ui/customization/color/#accessibility
https://ant.design/docs/spec/colors#neutral-color
https://www.radix-ui.com/themes/docs/theme/color#high-contrast

https://webkit.org/blog/16929/contrast-color/

APAC は別記事

## まとめ

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、「スタイリング方法」についての記事です。お楽しみにー
