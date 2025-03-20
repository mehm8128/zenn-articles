---
title: "3月中毎日MDNの`aria-`属性を翻訳した"
emoji: "🌏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["翻訳", "OSS"]
published: false
---

こんにちは、フロントエンドが好きな無職の mehm8128 です。

2/26 に社内 Slack で「3 月の無職期間、何しようかな」みたいなことを言ってたら先輩から「2025 年 1Q バージョンのアドベントカレンダー書きなよ」みたいな圧をかけられて、さすがに厳しいと思ってたのですが、a11y 周りの知識はもっと溜めておきたいと思っていたこともあり、アドベントカレンダーと比べると楽ですが、MDN の `aria-` 属性のページを毎日 1 ページ以上翻訳することにしました。

3/1 の時点で全部で 53 ページ、そのうち翻訳済みが 14 ページだったので 39 ページ残っていました。毎日 1 ページで 31 ページ翻訳できる計算で、残りは暇なときに 1 日 2 ページやったり、4 月以降にやればいいかなくらいの気持ちでした。

## MDN の新規翻訳のしかた

ここらへんを読んで環境構築したり書き方を確認したりしました。

https://github.com/mozilla-japan/translation/wiki/Get-started-with-translation-of-Mozilla-documentations

https://mozilla-japan.github.io/mdn-translation-guide/translation/0_preparation.html

細かい記法とか表記揺れとかは、なんとなく目を通すだけ通して、あとはレビュー時に指摘されたら直せばいいやくらいの気持ちで翻訳していました。

あとは ↓ の記事でも書いたように、最初に Google 翻訳で翻訳かけて、それをちょっと直す感じで進めていったので、比較的速く進めることができました。

https://zenn.dev/mehm8128/articles/oss-document-translation#biome

## 面白かったやつ

1 ヶ月翻訳してみて、面白かったものをいくつか紹介します。

- braille
  - 点字ディスプレイ
- ariaControlsElements
  - 要素のリストを取得できる。まだ Safari しかサポートしてない
- ElementInterenals
  - WebComponents の文脈らしい
- aria-owns と aria-controls
  - あとでちゃんと読む: https://www.makethingsaccessible.com/guides/aria-controls-vs-aria-owns/
- dl,dt,dd について
  - https://github.com/w3c/html-aria/issues/539
- role の継承

レビューについて
中身はほとんど見られてなくて、フォーマット的なところだけ見られてそうなので、翻訳内容が不安だったら明示的に書いておくべき

## まとめ
