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
https://react-spectrum.adobe.com/blog/accessible-color-descriptions.html
getAriaValueTextForChannel

各値の組み合わせに対して呼び方のリストを作成したけど、i18n なども考慮するとバンドルサイズがでかすぎた
→ もっと減らして、13 色の色とその中間色、明度、彩度の組み合わせを読み上げるようにした（もちろん各チャンネルの値も一緒に）
OKLCH という 2023 に newly available になった色空間を使っていて、HSL に対する利点が述べられている
https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/oklch

https://github.com/adobe/react-spectrum/blob/993de98adad65e48bcebad8ac835f5c9e0c94c85/packages/%40react-stately/color/src/Color.ts#L132-L197
で色の名前取ってきてそう

input2 つで x と y を管理していて、それ全体を group role で囲ってる

## まとめ

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、TabList についての記事です。お楽しみにー
