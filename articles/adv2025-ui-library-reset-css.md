---
title: "Reset CSS - UIライブラリ a11y"
emoji: ""
type: "tech"
topics: ["frontend", "a11y", "wcag", "css"]
published: false
---

:::message
この記事は [UI ライブラリ a11y - Qiita Advent Calendar 2025](https://qiita.com/advent-calendar/2025/ui-library-a11y) の 3 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。今回は Reset CSS について書いていきます。

## Reset CSS とは

何を目的としたものとか
いくつか具体例の紹介
https://github.com/necolas/normalize.css
https://www.tak-dcxi.com/article/introduce-kiso-css/
http://github.com/fokus-dev/uaplus

where とか unset とか

🍮 で reset css 選んだときのログ見てみる

## a11y との関係

https://www.tak-dcxi.com/article/introduce-kiso-css/

フォーカスリング: https://zenn.dev/s_a_k_u/articles/guide-to-design-wcag-compat-focus-indicators
https://github.com/elad2412/the-new-css-reset#accessibility-recommendation だと自分でやってくださいねって書いてある
→ これは、本当に全部リセットする系の reset CSS だから消してるらしい: https://github.com/elad2412/the-new-css-reset/issues/25

list-style: none
https://www.tak-dcxi.com/article/introduce-kiso-css/#%E3%82%A2%E3%82%AF%E3%82%BB%E3%82%B7%E3%83%93%E3%83%AA%E3%83%86%E3%82%A3%E3%81%B8%E3%81%AE%E9%85%8D%E6%85%AE

https://www.tak-dcxi.com/article/introduce-kiso-css/#%E3%83%95%E3%82%A9%E3%83%B3%E3%83%88%E3%81%AE%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%AB%E3%82%92%E6%97%A5%E6%9C%AC%E8%AA%9E%E5%90%91%E3%81%91%E3%81%AB%E3%81%99%E3%82%8B とか細かい調整も a11y の一貫
斜体について
https://solutionware.jp/2025/06/02/%E6%97%A5%E6%9C%AC%E8%AA%9E%E3%83%95%E3%82%A9%E3%83%B3%E3%83%88%E3%81%AE%E3%80%8C%E6%96%9C%E4%BD%93%E3%80%8D%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6/

強制カラーモード
https://www.tak-dcxi.com/article/introduce-kiso-css/#%E3%83%AA%E3%82%BB%E3%83%83%E3%83%88%E3%81%AE%E5%8E%B3%E9%81%B8
https://github.com/tak-dcxi/kiso.css/blob/main/kiso.css#L214-L224
とか

## まとめ

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、「」についての記事です。お楽しみにー
