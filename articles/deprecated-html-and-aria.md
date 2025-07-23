---
title: "a11y 上の理由で Deprecated になった HTML と ARIA まとめ"
emoji: "🗑️"
type: "tech"
topics: ["frontend", "a11y", "html", "aria"]
published: false
publication_name: "cybozu_frontend"
---

:::message
この記事は、[CYBOZU SUMMER BLOG FES '25](https://cybozu.github.io/summer-blog-fes-2025/) の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。
今回は敢えて、a11y 上の理由から Deprecated になった HTML と ARIA をまとめてみようという記事です。

## Deprecated の定義

今回 "Deprecated" は、基本的に MDN において Deprecated の表示があるものを指すこととします。ただし、MDN からも既に削除されているものについては例外となります（＝ Deprecated の表示がないけど今回の定義に含みます）。

MDN の "Deprecated" 表示については以下のページに記載されています。[mdn/browser-compat-data](https://github.com/mdn/browser-compat-data) からデータを参照しているので、[mdn/browser-compat-data のドキュメント](https://github.com/mdn/browser-compat-data/blob/main/docs/data-guidelines/index.md#setting-deprecated) も参照されています。

https://developer.mozilla.org/en-US/docs/MDN/Writing_guidelines/Experimental_deprecated_obsolete#deprecated

また、今回は主に a11y 上の理由で Deprecated になったものを扱いますが、それ以外の理由で Deprecated になったものには、主に以下の理由があります。

- HTML のマークアップではなく、CSS でスタイルをつけるべきもの
  - 文字の大きさや色、フォントを指定できた [`<font>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/font) 要素や、中央寄せするための [`<center>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/center) 要素など
- セキュリティ上の問題があるもの
  - Java アプレットを埋め込む [`<applet>`](https://udn.realityripple.com/docs/Web/HTML/Element/applet) 要素や、フォームで公開鍵を送信するための [`<keygen>`](https://udn.realityripple.com/docs/Web/HTML/Element/keygen) 要素など
  - `<applet>` 要素については a11y 上の問題もあったようですが、今回は省略しています
- 他の要素で代替可能なもの
  - [`<abbr>`](https://developer.mozilla.org/ja/docs/Web/HTML/Reference/Elements/abbr) と役割が重複している [`<acronym>`](https://developer.mozilla.org/ja/docs/Web/HTML/Reference/Elements/acronym) 要素や、[`<s>`](https://developer.mozilla.org/ja/docs/Web/HTML/Reference/Elements/s) 及び [`<del>`](https://developer.mozilla.org/ja/docs/Web/HTML/Reference/Elements/del) と役割が重複している[`<strike>`](https://developer.mozilla.org/ja/docs/Web/HTML/Reference/Elements/strike) 要素など

それでは、a11y 上の理由から Deprecated になったものを見ていきます。

:::message alert
この記事では実際に a11y 上の問題があるサンプルを紹介しています。前後の文章をよく読んでから参照先リンクを開くようにしてください。
:::

## Deprecated な HTML

これから紹介するいくつかの HTML 要素は、より詳細な背景が以下の Medium の記事に書かれています。
https://medium.com/@sumudusiriwardana/the-accessibility-story-behind-deprecated-html-elements-98396a877893

### `<marquee>`

https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/marquee

`<marquee>` 要素は、テキストが横に流れていくようなアニメーションを表現することのできる要素です。
MDN 内に例があり、アクセシビリティ上の問題を体験することができるサイト「駒瑠市」の [「動き続ける駒瑠市」の例](https://a11yc.com/city-komaru/practice/?preset=2.2.2-pause&wcagver=22) でも確認することができます（実際に `<marquee>` 要素が使われています）。

このような UI は [WCAG 2.2 の達成基準 2.2.2 一時停止、停止、非表示](https://waic.jp/translations/WCAG22/Understanding/pause-stop-hide.html) に違反するため、a11y 上の問題があります。また前述したような、HTML によるマークアップではなくて CSS によるスタイリングで表現するべきものでもあります。
どうしてもこのような UI を実現したい場合は、MDN に記載のある通り CSS アニメーションや `prefers-reduced-motion` による対応で実現したり、ボタンを押してアニメーションを一時停止できるようにしたりする必要があります。

### `<blink>`

https://developer.mozilla.org/en-US/docs/Glossary/blink_element

`<blink>` 要素は、テキストを点滅させ続けることができる要素です。
点滅させ続けるような UI は `<marquee>` 要素と同様に [WCAG 2.2 の達成基準 2.2.2 一時停止、停止、非表示](https://waic.jp/translations/WCAG22/Understanding/pause-stop-hide.html) に違反するため、a11y 上の問題があります。

UDN（MDN のアーカイブサイト） で、動作している gif を見ることができます。

https://udn.realityripple.com/docs/Web/HTML/Element/blink

なお、点滅による発作などの症状については [Web accessibility for seizures and physical reactions - Accessibility | MDN](https://developer.mozilla.org/en-US/docs/Web/Accessibility/Guides/Seizure_disorders) で解説されているようです。

### `<bgsound>`

https://udn.realityripple.com/docs/Web/HTML/Element/bgsound

`<bgsound>` 要素は、ページのバックグラウンドで音声を再生するための要素です。
そもそも IE でしかサポートされていなかったという点にも問題がありますが、音声を自動再生して流し続けることは [WCAG 2.2 の達成基準 1.4.2 音声の制御](https://waic.jp/translations/WCAG22/Understanding/audio-control.html) に違反するため、a11y 上の問題があります。
特にスクリーンリーダーユーザーは音に頼って Web サイトを閲覧するので、音声が再生され続けると Web サイトを閲覧できなくなってしまいます。

「駒瑠市」の [「自動再生の音声がある駒瑠市」の例](https://a11yc.com/city-komaru/practice/?preset=1.4.2-audio&wcagver=22) で音声が自動再生され続ける例を確認できますが、`<bgsound>` はブラウザでサポートされていないため、代替となった `<audio autoplay loop>` を用いることで自動再生を実現しています。

### `<frameset>`、`<frame>`、`<noframes>`

https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/frame
https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/frameset
https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/noframes

`<frameset>`、`<frame>`、`<noframes>` 要素は、現在の `<iframe>` 要素のように、別の HTML ドキュメントを表示できるように使われていた要素です。

a11y 上の問題としてはスクリーンリーダーの読み上げにおいて不都合があったようです。
https://webaim.org/techniques/frames/
https://www.washington.edu/accesscomputing/are-frames-accessible

### `<dir>`

https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/dir

`<dir>` 要素は、ディレクトリ（フォルダ）を表すための要素でした。
しかし、`<ul>` や `<ol>` のような、よりセマンティックでアクセシブルな要素が出てきたため、非推奨となりました。

## Deprecated な ARIA

### `directory` ロール

https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Roles/directory_role

`directory` ロールは、目次などといった参照リストに使われるロールでした。
しかし、`list` ロールがよりアクセシブルであるため、WAI-ARIA 1.2 で非推奨になりました。

### `aria-grabbed`

https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-grabbed

`aria-grabbed` は、ドラッグ＆ドロップで要素が掴まれた状態を表す属性でした。

しかし、将来的に新しい機能に置き換わるため、WAI-ARIA 1.1 で非推奨になりました。

https://github.com/w3c/aria/issues/1447

非推奨になるまでの流れや新しい機能についてはおそらく以下のメーリングリストを追うと分かるのですが、追いきれませんでした。
https://www.w3.org/Search/Mail/Public/search?keywords=%40aria-grabbed+and+%40aria-dropeffect&lists=public-pfwg&sortby=date-asc

### `aria-dropeffect`

https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-dropeffect

`aria-dropeffect` は、ドラッグされたオブジェクトがドロップされるときに何が起こるかを伝えるための属性でした。
例えば `aria-dropeffect="copy"` であれば、ドロップされたオブジェクトがコピーされることを示します。

`aria-grabbed` と同様、将来的に新しい機能に置き換わるため、WAI-ARIA 1.1 で非推奨になりました。

## 静的解析

markuplint を用いることで、Deprecated な HTML や ARIA に対して警告を表示することができます。

https://markuplint.dev/docs/rules/deprecated-attr
https://markuplint.dev/docs/rules/deprecated-element

## まとめ

HTML 要素や ARIA はしっかり [MDN](https://developer.mozilla.org/ja/docs/Web) や [HTML Living Standard](https://html.spec.whatwg.org/multipage/)、[WAI-ARIA](https://www.w3.org/TR/wai-aria/) を参照し、非推奨でないものを正しい使い方で使うようにしましょう。
