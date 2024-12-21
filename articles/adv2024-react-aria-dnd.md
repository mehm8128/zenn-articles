---
title: "DnDについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

:::message
この記事は [React Aria の実装読むぞ - Qiita Advent Calendar 2024](https://qiita.com/advent-calendar/2024/react-aria) の 24 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は DnD について書いていきます。

## 本題

https://react-spectrum.adobe.com/react-aria/dnd.html

キーボード操作で dnd ができるようになっている
どういうキーボード操作で dnd ができるのかが説明されている

https://react-spectrum.adobe.com/blog/drag-and-drop.html

操作中に操作の方法がアナウンスされる

https://react-spectrum.adobe.com/react-aria/useDrag.html
https://react-spectrum.adobe.com/react-aria/useDrop.html
https://react-spectrum.adobe.com/react-aria/useClipboard.html

実装読むところから
特にキーボード操作とか、アナウンスより操作方法の説明とかに着目したい
useClickboard でどのくらい共通化されているかとかも
collection の hooks で何をどうしてるかとかも

RFC あったよくらいの説明
https://github.com/adobe/react-spectrum/blob/main/rfcs/2020-v3-dnd.md

###

## まとめ

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、まとめの記事です。お楽しみにー
