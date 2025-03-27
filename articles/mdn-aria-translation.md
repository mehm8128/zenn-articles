---
title: "1ヶ月間毎日MDNの`aria-`属性を翻訳して知ったこと"
emoji: "🌏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["翻訳", "OSS"]
published: false
---

こんにちは、フロントエンドが好きな無職の mehm8128 です。

2/26 に社内 Slack で「3 月の無職期間、何しようかな」みたいなことを言ってたら先輩から「2025 年 1Q バージョンのアドベントカレンダー書きなよ」みたいな圧をかけられて、さすがに厳しいと思ってたのですが、a11y 周りの知識はもっと溜めておきたいと思っていたこともあり、アドベントカレンダーと比べると楽ですが、MDN の `aria-` 属性のページを毎日 1 ページ以上翻訳することにしました。

3/1 の時点で全部で 53 ページ、そのうち翻訳済みが 14 ページだったので 39 ページ残っていました。毎日 1 ページだとして 31 ページ翻訳できる計算でした。残ったものは 4 月以降にやるつもりです。

また、MDN の翻訳は翻訳後ページが存在しないときが一番ハードルが高く、ページ自体が存在しているときには手元に clone しなくても GitHub 上で編集して PR を送ることも可能なので、今回はそのハードルの高い部分を行うということを重視しました。そのため、1 ページ 1 ページの質はそこまで高くないですが、翻訳に気になる部分があれば色んな人が少しずつ直してもらえればと思います。

## MDN の新規翻訳のしかた

ここらへんを読んで環境構築したり書き方を確認したりしました。

https://github.com/mozilla-japan/translation/wiki/Get-started-with-translation-of-Mozilla-documentations

https://mozilla-japan.github.io/mdn-translation-guide/translation/0_preparation.html

細かい記法とか表記揺れとかは、なんとなく目を通すだけ通して、あとはレビュー時に指摘されたら直せばいいやくらいの気持ちで翻訳していました。

あとは ↓ の記事でも書いたように、最初に Google 翻訳で翻訳かけて、それをちょっと直す感じで進めていったので、比較的速く進めることができました。

https://zenn.dev/mehm8128/articles/oss-document-translation#biome

TODO: "原文の表現が不適切あるいは不明瞭なところは独自表現に変えることも厭わない。" について
https://github.com/mozilla-japan/translation/wiki/L10N-Guideline

## 面白かったやつ

1 ヶ月翻訳してみて、面白かったものをいくつか紹介します。

TODO: 翻訳してないところも読んでおく

### `aria-braille*` 属性

[aria-braillelabel](https://developer.mozilla.org/ja/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-braillelabel) と [aria-brailleroledescription](https://developer.mozilla.org/ja/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-brailleroledescription) という `aria-` 属性が、[WAI-ARIA 1.3](https://w3c.github.io/aria/) で追加される予定です。
これは点字ディスプレイのユーザーがコンテンツを快適に読めるように使用するもので、`aria-label` および `aria-roledescription` を点字ディスプレイ用に簡略化したようなものです。例えば `3 out of 5 stars` というテキストを `aria-braillelabel="***"` と表現したり、`aria-roledescription="slide"` を `aria-brailleroledescription` では `sld"` と略す例などが挙げられていて、点字ならではの表記方法をすることでユーザー体験を向上させることができることが分かり面白かったです。

### ElementInterenals

関連インターフェイスのセクションに出てくるやつです。
Baseline 2023 で Newly Available になった、Web Components 関連の API で、Custom Elements を HTML の `input` 要素などと同等に扱えるようにし、フォームバリデーションなどをできるようにするものらしいです。

https://developer.mozilla.org/ja/docs/Web/API/ElementInternals

### aria-owns と aria-controls

- あとでちゃんと読む: https://www.makethingsaccessible.com/guides/aria-controls-vs-aria-owns/

### `ariaControlsElements`

[aria-controls](https://developer.mozilla.org/ja/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-controls) の関連インターフェイスのセクションにあります。

これを参照すると、`aria-controls` に指定している id の実際の要素（[Element](https://developer.mozilla.org/ja/docs/Web/API/Element)）を取得することができるらしいです。

関連インターフェイスのセクションには、大体 `aria-` 属性の現在の値をそのまま取得するプロパティが書かれているだけだったので、その値が示している要素を取得できるプロパティがあるのは珍しかったです。

ただ、まだ全てのブラウザでサポートされているわけではないので注意が必要です。

https://caniuse.com/mdn-api_element_ariacontrolselements

### `dl`, `dt`, `dd` に対応するロール

[dl](https://developer.mozilla.org/ja/docs/Web/HTML/Element/dl), [dt](https://developer.mozilla.org/ja/docs/Web/HTML/Element/dt), [dd](https://developer.mozilla.org/ja/docs/Web/HTML/Element/dd) のロールが、日本語版 MDN だとそれぞれ `list`, `term`, `definition` になっているのに対し、英語版 MDN だとどれも `	No corresponding role` となっていました。

これについては html-aria の方で [関連 Issue](https://github.com/w3c/html-aria/issues/539) が生えていました。内容としては僕が気づいたものとは少し違っていて、[html-aria](https://w3c.github.io/html-aria/) では `No corresponding role` だけど [html-aam](https://w3c.github.io/html-aam/) では上記の各ロールが掲載されているというものです。

WAI-ARIA 1.3 で `dl`, `dt`, `dd` に対してそれぞれ `associationlist`, `associationlistitemkey`, `associationlistitemvalue` というロールが追加される予定だったのですが、名前が長すぎるということで [Issue](https://github.com/w3c/aria/issues/1662) が立ち、一度 revert されたようです。
おそらくこれらのロールが追加される予定だった背景としては、[dl/dt/dd のスクリーンリーダーの読み上げをなんとかする](https://zenn.dev/yusukehirao/articles/dl-dt-dd-wai-aria) にあるように、`dt` 要素と `dd` 要素が同等な `li` 要素であるように読み上げられる問題を解決するということがあったのだと思います。そのためこの Issue にある minutes によると、`dl` を `list`、`dd` を `listitem` のまま、それに加えて`dt`に対する新たなロールを 1 つ追加するような方針が提案されています（現状 `definition` や `term` ロールが割り当てられているはずなのになぜ `listitem` のような読み上げ方がされるのかは分かりませんでした...）。

TODO: html-aria と html-aam の違いは？なんで 2 つ別々の場所に書かれてる？

### 抽象ロール

[抽象ロール](https://developer.mozilla.org/ja/docs/Web/Accessibility/ARIA/Reference/Roles#6._%E6%8A%BD%E8%B1%A1%E3%83%AD%E3%83%BC%E3%83%AB) という概念を知りませんでした。
抽象ロールは開発者が使用するロールの基礎となっているもので、例えば `heading` ロールは `sectionhead`、`button` ロールは `command`、`tablist` ロールは `composite` という抽象ロールを継承しているロールらしいです。

[aria-valuemax](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-valuemax) で、`range` は抽象ロールなので、この属性は `range` ロール自体ではなくて、`range` のサブタイプロールで使いましょうという警告がありました。

### `comment` ロール

https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Roles/comment_role

[aria-setsize](https://developer.mozilla.org/ja/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-setsize) で出てきました。[WAI-ARIA 1.3](https://w3c.github.io/aria/#comment) で追加予定のロールらしく、今回初めて知りました。

WAI-ARIA 1.3 で追加されるロールは他に [mark](https://developer.mozilla.org/ja/docs/Web/Accessibility/ARIA/Reference/Roles/mark_role) と [suggestion](https://developer.mozilla.org/ja/docs/Web/Accessibility/ARIA/Reference/Roles/suggestion_role) があるようです。

## レビューについて

日本語の翻訳 PR は日本人のメンテナーの方が見てくれます。
レビュー時にはおそらく正確に・分かりやすく翻訳されているかはあまり見られていなくて、フォーマットや翻訳規則などに従っているかどうかを特に確認していそうでした。翻訳内容で不安な点があれば、明示的に書いておくと見てもらえると思います。

レビュワーの方々、ありがとうございました。

## まとめ
