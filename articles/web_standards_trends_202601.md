---
title: "Web 標準動向 2026年1月版"
emoji: "🎍"
type: "idea"
topics: ["frontend", "cybozuwebstandards"]
published: false
publication_name: "cybozu_frontend"
---

こんにちは！ サイボウズ株式会社 フロントエンドエンジニアの [mehm8128 (@mehm8128)](https://x.com/mehm8128) です。

## はじめに

サイボウズは 2025 年 4 月より、W3C のメンバーに加入しました。

https://blog.cybozu.io/entry/joining-w3c

標準化プロセスに関わることができるようになるための最初の一歩として、フロントエンドエンジニアの一部のメンバーは積極的に Web 標準のキャッチアップを行っています。

そこで、毎月メンバーが興味を持った Web 標準に関する話題や、実際に標準化プロセスに関わることができた場合にはその報告などを 1 つの記事としてまとめ、紹介していきます。
また、ここでは W3C に限らず、TC39 や WHATWG などの標準化団体のトピックについても扱います。

:::message
今月の執筆者は以下の 4 名です。

- [Saji](https://x.com/sajikix)
  - ECMA402 (Intl) 周りのトピックを執筆
- [saku](https://x.com/sakupi01)
  - HTML, CSS に関連するトピックを執筆
- [くらっち](https://x.com/kuracchi04)
  - ECMA262 周りのトピックを執筆
- [mehm8128](https://x.com/mehm8128)
  - 主にアクセシビリティに関連するトピックを執筆

:::

## HTML

### Blink: Prototype: Canvas text writing mode support

https://groups.google.com/a/chromium.org/g/blink-dev/c/WxD-ssll9K4

Canvas 上でのテキスト描画において、ついに縦書き（writing-mode）のネイティブサポートがプロトタイプ段階に入りました。

ただ、議論が進む中で Writing Mode をフルにサポートするコストが高いため、 `textOrientation` で文字だけ回し、ボックスを `transform()` で回すことで、縦書きを実現すればよいのでは？という意見も出ています。

[Writing Mode for Canvas text](https://github.com/whatwg/html/issues/11449)

「っ」や「。」といった縦書き特有グリフや、およびロジカルプロパティに対応するためにも、`writingMode` と `textOrientation` 属性を Canvas API に追加することで、CSS 同様の正しい文字整形を目指しています。

### Blink: Prototype: 'step-up' and 'step-down' Invoker Commands

https://groups.google.com/a/chromium.org/g/blink-dev/c/iwXoPUJhjIw

Invoker Commands の拡張である、 `<input type="number">` や range, date などの値を増減させる step-up / step-down コマンドのプロトタイプです。 これにより、JavaScript を書くことなく、HTML Button 要素から対象要素の `.stepUp()` / `.stepDown()` メソッドを宣言的に呼び出せるようになります。

### Introducing the HTML `<geolocation>` element

https://developer.chrome.com/blog/geolocation-html-element

Page Embedded Permission Control (PEPC) から切り出される形で、位置情報取得専用の `<geolocation>` 要素が提案されました。 PEPC API の複雑化を避けるため、一つの `permission` 要素に全ての PEPC 機能を詰め込むのではなく、専用要素として仕切り直した形です。`<geolocation>` 要素により、許可状態やエラーの処理をブラウザに任せ、宣言的かつよりユーザフレンドリーな Permission コントロールを実装することができます。

同様に `<usermedia>` 要素なども検討されており、セキュリティと UI の拡張性を保つための試行錯誤が続いています。

### Filtering support for customizable select

https://github.com/whatwg/html/issues/12050

Customizable Select における、選択肢のフィルタリング（検索）機能を実現するためのマークアップ方針に関する議論です。  
Github のラベル選択 UI のような「テキスト入力で選択肢を絞り込む」機能は、現状では独自に Input と Listbox を実装し、WAI-ARIA 属性やキーボード操作を適切に管理する必要があります。これを `<select>` 要素の機能として標準化する検討が進んでいます。  
当初は `<select>` 内に `<input>` を配置する案が検討されましたが、パーサーの互換性問題（既存サイトへの影響）により困難であるとの見解が示されました。  
現在は、外部の `<input>` 要素に `filter` 属性を追加し、それによって `<select>`（Listbox）と紐付ける方針（Option 3）が有力視されている状況です。

### AI Content Disclosure 

https://github.com/WICG/proposals/issues/261

2026年8月に施行される「EU AI法」を見据え、AI生成コンテンツであることを機械判読可能な形式で明示する仕組みが議論されています。

WICG で提案された `ai-*` はページ内の一部（要素単位）がAI生成であることを示す属性群です。  
また、WHATWG/HTML の方でも[ドキュメント全体がAI生成であることを示す Meta タグ](https://github.com/whatwg/html/issues/9479)が提案されています。

AI 関連法規制への適合がブラウザ仕様に直接影響を与える、2026年らしい動向でしょう。

### Blink: Intent to Experiment: Focusgroup

https://groups.google.com/a/chromium.org/g/blink-dev/c/4vVuaMwMCTQ/m/BZRK6VQaAwAJ

[9月に取り上げたScoped Focusgroup](https://zenn.dev/cybozu_frontend/articles/web_standards_monthly_202509#intent-to-prototype%3A-focusgroup)のIntent to Experimentが示されました。

Chrome 146～149でOrigin Trialとして利用可能になります。機能については[Open UIのExplainer](https://open-ui.org/components/scoped-focusgroup.explainer/)をご覧ください。

## CSS

### Proposed web platform gap: motion

https://github.com/w3c/a11y-tracking/issues/290

[Scroll-Triggered Animationsのレビュー](https://github.com/w3ctag/design-reviews/issues/1167#issuecomment-3667157396)を発端として、ユーザーの好み・特性に合わせて多様な方法でモーションの制御をできるようにし、それをUA間で統一したいという提案です。

例えば以下のような制御方法が挙げられています。

- モーションを減らす
- モーションを完全に止める
- モーションを処理するまでの時間的余裕を与える
- モーションを一時停止できるようにする
- モーションの速度を調節できるようにする

実現された場合、併せて`prefers-reduced-motion`の取りうる値も追加されることになりそうです。

### CSS Wrapped 2025

https://developer.chrome.com/blog/css-wrapped-2025

毎年恒例の、Chrome チームのメンバーによる 2025 年の CSS の主要項目をまとめた記事です。

### Intent to Ship: Decouple border-width, outline-width and column-rule-width values from their corresponding style values

https://groups.google.com/a/chromium.org/g/blink-dev/c/_50sVoTmZ3I

以前は、 `border-style` のような `*-style` プロパティが `none` または `hidden` に設定されていた場合、たとえ幅が明示的に `10px` に設定されていても、計算される `border-width` のような `*-width` は `0px` とされていました。

今後は `border-width`、`outline-width`、 `column-rule-width` の計算値は、関連するスタイルプロパティの値に関わらず、指定された値が反映されるようになります。

Gecko / Webkit はすでにこの挙動なので Chrome におけるリスクも低いとされています。

### Prototype: CSS Image Animation

https://groups.google.com/a/chromium.org/g/blink-dev/c/0kUk5-6TAfM

`image-animation` が新しく導入され、CSS でGIF などアニメーション画像の再生制御が可能になるという提案のプロトタイプです。  
現在のアニメーション画像自動再生の挙動はアクセシビリティの基準に違反する可能性があり、開発者が再生を制御する手段が不足していることが端を発しました。

### Prototype: `named-feature()` function for CSS `@supports`

https://groups.google.com/a/chromium.org/g/blink-dev/c/yDqCRNiGZZs/m/rHj097DdBwAJ

既存の「プロパティ: 値」という単純なチェックだけでは判別できない、特定の機能や組み合わせのサポート状況を確認するための仕組みとして、`named-feature()`とその内部で利用されるキーワードが検討されています。  
特定条件下での機能について、仕様側で定義したキーワードを用いて、それを @suport でチェックできるようにする試みです。  
[CSS Conditional Rules Module Level 5](https://drafts.csswg.org/css-conditional-5/#typedef-supports-named-feature-fn)

例えば、 `align-content` が `display: block` に対して効くようになった版かどうかを、 `@supports named-feature(align-content-on-display-block)` といった記法で判定できるようになるとのことです。

### Prototype: CSS contrast-color()

https://groups.google.com/a/chromium.org/g/blink-dev/c/MRCj4zyxOgI

背景色に合わせて「黒か白か、よりコントラストが高い方」をブラウザが自動選択する `contrast-color()` が Chrome でもプロトタイプ開始されました。 先行する Safari、Firefox に続き、Chrome で実装されれば「Newly Available」となり、アクセシビリティ改善の自動化に大きく貢献する CSS 関数です。

### Support for wrapped columns in multi-column layout 

https://developer.chrome.com/blog/multicol-wrapping

Chrome 145 より、Multi-column Layout Level 2 の `column-wrap` および `column-height` プロパティがサポートされます。

これまで Multi-column レイアウトでは、コンテナの高さが制限されている場合、溢れたコンテンツはインライン方向に新しいカラムとして追加され、横スクロールが発生する原因となっていました。  
今回の変更により、溢れたカラムをブロック方向に折り返して配置することが可能になります。

### Using 100vw is now scrollbar-aware (in Chrome 145+, under the right conditions)

https://www.bram.us/2026/01/15/100vw-horizontal-overflow-no-more/

長年 Web 制作者を悩ませてきた、Windows 等の「非 Fluent Scrollbar 環境で `100vw` を使うと横スクロールが発生する」問題がに対して、 Chrome 145+ より、ビューポート単位の計算からスクロールバーの幅が除外されるプルリクエストがマージされました。これにより、意図しない横スクロールバーの出現を防ぐことができます。

[Subtract scrollbars from viewport during viewport unit calculation - Chromium](https://issues.chromium.org/issues/354751900)

破壊的変更度合いについても調査されており、[0.0003143195899% のページに影響する](https://github.com/w3c/csswg-drafts/issues/6026#issuecomment-1919428550)とのことです。

### [css-scrollbars-1] Add `scrollbar-style` property for `overlay` scrollbars

https://github.com/w3c/csswg-drafts/issues/13218

ウェブサイトのスクロールバーの見た目と挙動を制御する新しい CSS プロパティ `scrollbar-style` を導入しようとする提案です。

ちなみに、Windows 版 Chrome などでは、Fluent Scrollbars の導入が検討されており、一定以上のバージョンにおいてフラグ付き（ `chrome://flags/#fluent-overlay-scrollbars`）で利用可能です。

[Fluent Scrollbars Visual Spec](https://docs.google.com/document/d/1haDpb1QIh2PaLwsQD1i4WHFq_5_jSK3XK9lhgSs4WkM/edit?tab=t.0)

### [css-grid] Decide on a name for `item-slack`

https://github.com/w3c/csswg-drafts/issues/10884

Flexbox や Grid レイアウトに利用されるプロパティ群を統一的に説明し、Masonry レイアウト（Grid-Lanes）の制御行う Item Flow プロパティに関してです。アイテムの「たるみの許容度」を指定できるとされていた `item-slack` でしたが策定過程で `item-toralance` になり、最終的に `flow-toralance` に決定されました。  
これから Item Flow は `item-*` 系ではなく `flow-*` 系で策定する可能性が高まりそうです。

[When will CSS Grid Lanes arrive? How long until we can use it? | WebKit](https://webkit.org/blog/17758/when-will-css-grid-lanes-arrive-how-long-until-we-can-use-it/)

### Blink: Intent to Ship: meta name="text-scale"

https://groups.google.com/a/chromium.org/g/blink-dev/c/0yp2ygJK5HE

ルート要素のデフォルトのフォントサイズを、ブラウザとOSの両方の文字サイズ設定を反映して決めるようにするものです。

今までOSの設定で文字サイズを調節しても、ブラウザで閲覧するWebページ上の文字サイズには反映されていませんでした。しかし今回、`<meta name="text-scale" />`というmeta要素が付与されているWebサイトでは端末の設定が反映されるようになります。

Chrome 146から利用可能になります。

## ARIA・WCAG

### Gecko: Intent to Prototype: ariaNotify

https://groups.google.com/a/mozilla.org/g/dev-platform/c/7UOkFqbeH7o/m/IozzYfuGAAAJ

Chrome 141から利用可能になるとして[9月に取り上げた](https://zenn.dev/cybozu_frontend/articles/web_standards_monthly_202509#aria-notify)ariaNotifyが、GeckoでもIntent to Prototypeになりました。

### aria-current and scrolling...

https://github.com/w3c/css-aam/issues/15

アクティブなスクロールマーカー（`:target-current`にマッチする要素）に、`aria-current=”true”`のような暗黙のマッピングを与えるべきなのではないかという、CSS WGで提案されたものがcss-aamのリポジトリにもissueが立てられました。

その後ariaのリポジトリにも[issue](https://github.com/w3c/aria/issues/2710)が立てられ、[先日のミーティング](https://www.w3.org/2026/01/29-aria-minutes.html#dbf4)で提案通りの内容で合意が取られました。

また、[clarify css generated content alternative text](https://github.com/w3c/css-aam/issues/16)のような、CSSのcontentプロパティの代替テキストとaccname仕様についてのissueが挙がっていたりと、TPAC以来試しに再始動しているcss-aamリポジトリが利用される動きが出ています。

## JavaScript

### ECMA402

#### iso8601 as DateTimeFormat calendar

https://github.com/tc39/ecma402/issues/1036

Intl.DateTimeFormatで`iso8601` カレンダーを指定した時の挙動がCLDR側で安定しておらず、結果的にブラウザ間でも挙動差異が生まれてしまっていました。

これに対して今回の議論でTC39側からCLDRに対し、`iso8601` カレンダー使用時は全てのロケールで日付に `-`、時刻に`:`を維持し、デフォルトの時間を24時間制（h23）とするよう推奨することで合意されました。

#### Intl Energy Units: Unit Choices

https://github.com/tc39/ecma402/issues/739

`Intl.NumberFormat`にワット、キロワット、キロワット時などのエネルギーに関する単位サポートを追加する提案について話し合われました。この提案は以前のTC39 TG1 Meetingで「ジュール」を含めるべきというフィードバックがありつつ、Stage 1へ進むことが承認されています。

これらに対して、「カロリー」などの日常的な単位も検討すべきと言う意見や、逆にSI単位をすべて許容すると管理不可能になるリスクがあることが指摘され、エネルギー単位でどこまでをサポートするかが議論となりました。

[このプロポーザル自体が主に電気自動車や電力消費の表示のニーズからスタートしている](https://github.com/tc39/ecma402/issues/739)ことも踏まえ、用途をこれらに限定し、CLDRに対してで優先度の高い単位を検討してもらってからサポートする単位を検討する方針で合意されました。

#### Intl Era Month Code : 複数あるイスラム暦の扱いの最終結論

https://github.com/tc39/proposal-intl-era-monthcode/issues/99

2025年11月の会議において合意された結論を元に、Temporalがサポートする暦のリストを「閉じたリスト」（Closed Set / エンジンの拡張を許さないセット）とし、islamic-rgsaカレンダーは含めないことで合意しました。

参考：2025年11月の会議における議論については去年の「[Web 標準動向 2025年12月版](https://zenn.dev/cybozu_frontend/articles/web_standards_trends_202512#ecma402)」でも触れています。

#### Be explicit about the Gregorian to Meiji switchover month and day in the Japanese calendar

https://github.com/tc39/proposal-intl-era-monthcode/issues/86

日本暦の「明治」の開始日について、CLDRに太陰太陽暦と太陽暦を取り違えた古いデータが存在し、実装間で混乱を招いていた点について、定義をどうするかの議論がなされました。

補足：日本における太陽暦への改暦とCLDR問題(zennのinfo記法がいいかも)

日本が太陽暦(グレゴリウス暦)を導入したのは西暦1837年の1月1日で、これを明治6年の1月1日として制定しました。ややこしいですが西暦1836年の12月31日は日本の旧暦(太陰太陽暦)における明治5年12月2日であるため、「明治5年12月2日(旧暦)の次の日が明治6年の1月1日(グレゴリウス暦)」という形になっています。

今回の問題は古いCLDRが、明治5年12月2日以前の年月日もグレゴリウス暦かのように扱ってしまっていたために、古いCLDRを参照しているシステムで改暦以前の計算がズレるというものです。

これに対し、1872年12月31日まではグレゴリオ暦の元号（AD/CE相当）を使用し、1873年1月1日以降を日本の太陽暦元号として定義することに合意しました。

### ECMA262

### tc39/proposal-iterator-sequencing: Stage 4

https://github.com/tc39/proposal-iterator-sequencing

既存のイテレータを連結してイテレータを作成する提案です。
Iterator.concatを利用すると直感的で分かりやすい記述が可能になります。

```
let lows = Iterator.from([0, 1, 2, 3]); let highs = Iterator.from([6, 7, 8, 9]); let digits = Iterator.concat(lows, [4, 5], highs);　// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### tc39/proposal-import-text: Stage 2

https://github.com/tc39/proposal-import-text

JSON, Binary, CSS に続いて Text も UTF8 として import できるようにする提案です。

```
import text from "path/to/file.txt" with { type: "text" };
```

### tc39/proposal-composites: Stage 1

https://github.com/tc39/proposal-composites

失敗した Record / Tuple のコンセプトに少しでも近づけるための折衷案です。

`Composite()` を関数にし、同じ形のオブジェクトに同じインスタンスを返すようにします。
返ったインスタンスは `===` が同じです。

```
const pos1 = Composite({ x: 1, y: 4 }); const pos2 = Composite({ x: 1, y: 4 }); Composite.equal(pos1, pos2); // true
```

グローバルに Map を持ち保存しておく内部実装なので、 GC や WeakMap など課題はまだあります。

### tc39/proposal-amount: Stage 1

https://github.com/tc39/proposal-amount

数字と単位を持つオブジェクトです。

一度取り下げた単位の変換に再チャレンジしています。
CLDR が持つ SI 単位系を対象にし、計算については一旦言及がありません。

## Baseline

### 📃 January 2026 release notes

https://web-platform-dx.github.io/web-features-explorer/release-notes/january-2026/

## Misc

### PSA: Overscroll effect on non-root scrollers

https://groups.google.com/a/chromium.org/g/blink-dev/c/0rMc8bTNVCo

Chrome 145 より、ルート以外のスクロールコンテナ（ `overflow: scroll` を持つ要素など）においても、オーバースクロール効果（Elastic overscroll /バウンススクロール）が有効になります。

macOS などの OS では標準的な挙動ですが、これまで Chrome のネストされたスクローラでは無効化されていました。Firefox と Safari は既に実装済みであり、今回の変更によってブラウザ間での挙動が統一されます。  
この挙動を無効化したい場合は、従来通り CSS の `overscroll-behavior` プロパティで制御可能です。

### Chrome 145 adds Experimental Support for Vertical Tabs

https://www.bram.us/2026/01/16/chrome-145-adds-experimental-support-for-vertical-tabs/

Edge 同様、Chrome にも Vertical Tabs が入るとのことです。Chrome 145+ で `chrome://flags/#vertical-tabs` を有効にすることで利用できます。

### Joint statement from Google and Apple

https://blog.google/company-news/inside-google/company-announcements/joint-statement-google-apple/

Apple のプラットフォーム（特に Siri）に Google の Gemini が統合されることが、共同声明として正式に発表されました。

### Mozilla Localization in 2025 – Mozilla L10N

https://blog.mozilla.org/l10n/2026/01/07/mozilla-localization-in-2025/

### EPUB and HTML - Survey results and next steps

https://www.w3.org/blog/2026/epub-and-html-survey-results-and-next-steps/

長年議論されていた「EPUBのベースをXMLからHTMLへ移行する」提案が、出版エコシステムの既存資産（XMLベースのワークフロー）との乖離が大きすぎるとして見送りとなりました。

### 2025 Web Almanac

https://almanac.httparchive.org/ja/2025/

HTTP Archive による、Web の状態に関する年次レポート「Web Almanac」の 2025 年版が公開されました。

今回は、Fonts, Accessibility, Performance, Privacy, Security, Cookies など全 16 章から構成されています。今年は新たに生成 AI に関する章が追加された点で、例年と異なりました。

### Doing Our Share for the Web in 2025

https://www.igalia.com/2026/01/05/Doing-Our-Share-for-the-Web-in-2025.html

Igalia による、2025 年の活動報告です。  
Chromium プロジェクトにおいては、Google に次ぐ第 2 位のコミット数を記録し、Microsoft を上回る貢献を行っています。WebKit においても Apple に次ぐ第 1 位の貢献者であり、レンダリングエンジンのパフォーマンス改善や Skia への移行などを主導したと報告しています。その他、Firefox や Servo、TC39 (Test262) への貢献についても言及されています。

### State of HTML 2025

https://2025.stateofhtml.com/en-US/

State of HTML 2025 の結果が公開されました。  
Form、グラフィックス、各種 API 利用状況など、Web プラットフォームの機能に関する開発者の利用実態や満足度が毎年集計されています。

今回は自由記述回答の分析が強化され、開発者が抱える具体的な「Pain points」がより詳細に可視化されたとのことです。こうした現場の声は、Web 標準の策定プロセスにおいても重要な判断材料となります。

### Some Thoughts on the Open Web

https://www.mnot.net/blog/2026/01/20/open_web

TPAC で Future of the Open Web Breakout Session を開いた Mark Nottingham 自身からの、The Open Web に対する見解です。

> **we have to create an Internet where people want to publish content openly – for some definition of “open.”**
