---
title: "【番外編】テストについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は ~~枠稼ぎ~~番外編として、React Aria で行われているテストについて書いていきます。

## React Aria に導入されているテスト

React Aria では主に 2 種類のテストが導入されています。
1 つ目は Storybook で、開発中に使うのはもちろん、おそらく Chromatic も利用されています。CI で回しているようなコードが見当たらなかったのですが、Chromatic 用の Storybook が別で用意されているのでおそらく使われています。
2 つ目は Testing Library & Jest で、今回はこちらをメインに紹介していきます。

## Testing Library & Jest

role を確かめてるとかなんとか
ちょっと特殊なのがあれば面白い
まだ書いてない記事の hooks を紹介しちゃうと分からない&ネタバレになるので、（i18n もだけど）1 週間くらい遅らせてもいいかも

packages/dev/test-utils

playwright の aria snapshots についてもちょっと書く

## まとめ

明日は の話です。お楽しみにー
