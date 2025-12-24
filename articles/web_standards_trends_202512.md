---
title: "Web 標準動向 2025年12月版"
emoji: "🎅"
type: "idea"
topics: ["frontend", "cybozuwebstandards"]
published: true
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
  - WebNN の項目を執筆
- [mehm8128](https://x.com/mehm8128)
  - 主にアクセシビリティに関連するトピックを執筆

:::

## HTML

### \[Draft\] Introduce the new menu elements

https://github.com/whatwg/html/pull/12011

Open UI から提案された、Menu 要素の仕様の Draft PR が作成されました。
Menu 要素はアプリケーションにおいて、コマンドを実行するためのボタンを配置する、画面上部のメニューバーを定義するものです。`<menubar>`, `<menuitem>`, `<menulist>`の 3 つの要素が新しく提案されています。

なお、Chrome では既に feature フラグを有効化することで試すことができます。

### Blink: Ship: Customizable select listbox

https://groups.google.com/a/chromium.org/g/blink-dev/c/i7bGjrFmuQU

Select Element において `size >=2` の場合における、Listbox(In-page select) UI の標準化です。Picker(Popup) UI から Listbox パーツを切り離した部分をイメージするとわかりやすいでしょう。
このパーツを Input と組み合わせたり独自で Popup させたりして、将来的には Combobox などにも流用できる、プリミティブなパーツです。

### Blink: Prototype: Canvas text writing mode support

https://groups.google.com/a/chromium.org/g/blink-dev/c/WxD-ssll9K4

Canvas のテキスト描画に書字方向（writing mode）のサポートを追加する提案です。Canvas のテキストスタイルに `writingMode` 属性と `textOrientation` 属性が追加され、CSS で既にサポートされている縦書きテキストの正しい文字整形が Canvas でも可能になります。

## DOM

### Blink: Ship: Trusted Types spec alignment

https://groups.google.com/a/chromium.org/g/blink-dev/c/OjQXhCZiXe0

Trusted Types の仕様が更新され、モンキーパッチで実装されていた Chrome が準拠しました。
Safari/Firefox も開発を進めています。

### Blink: Ship: Sanitizer API

https://groups.google.com/a/chromium.org/g/blink-dev/c/iu3VwMotMBc

Sanitizer API は、HTML 文字列からスクリプトなど危険な部分を削除する API です。
全ブラウザの Standard Position が Positive で WHATWG では Stage 2 となっている提案です。

## CSS

### Blink: Ship:`text-justify`CSS property

https://groups.google.com/a/chromium.org/g/blink-dev/c/7p3x9gwsB6U

`text-align: justify`（両端揃え）が付与されたテキストに対して、どのような方法で文字間隔を調整するか（割り付け方法）を制御する CSS プロパティです。
単語間/文字間の余白を用いた両端割り付け方法などを指定できます。

### What's new in DevTools, Chrome 143

https://developer.chrome.com/blog/new-in-devtools-143

Chrome 143 の DevTools では、 CSS 関連の新しい機能がサポートされました。

- `@starting-style` 時のスタイルモック
- `display: grid-lanes`（Masonry レイアウト）の、Flexbox や Grid と同様のエディタウィジェット対応

### Blink: Prototype and Ship: trigger-scope

https://groups.google.com/a/chromium.org/g/blink-dev/c/2hN81UUSQO8

Scroll-triggered Animations で利用する dashed ident のスコープを制限し、名前の衝突を防ぐプロパティです。
`anchor-scope` の Scroll-triggered Animations 版のようなイメージです。

### Introducing CSS Grid Lanes

https://webkit.org/blog/17660/introducing-css-grid-lanes/

Grid-Lanes の使い方と現状の解説です。

Item Flow の仕様はまだ固まっていませんが、 `display: grid-lanes` における `grid-template-[columns | rows]` のデフォルト挙動は固まり、基本的な Masonry レイアウトは組める段階になっています。[Technology Preview 234](https://webkit.org/blog/17674/release-notes-for-safari-technology-preview-234/) から試すことができます。
各エンジンでの実装状況は以下で確認できます。

- 302618 – Enable grid-lanes on stable
  - https://bugs.webkit.org/show_bug.cgi?id=302618
- Implement CSS Grid Lanes (aka Masonry) [343257585] - Chromium
  - https://issues.chromium.org/issues/343257585#comment180
- 1981604 - (masonry) [css-grid-3] [META] Implement css-grid-3 masonry 
  - https://bugzilla.mozilla.org/show_bug.cgi?id=1981604

## ARIA・WCAG

### aria-focus-combine

https://github.com/w3c/aria/issues/2693

`aria-focus-combine`という新しい ARIA 属性の提案です。
子要素が複数に分かれているリンクにおいて、スクリーンリーダーがそれぞれの要素をばらばらに読んでしまうため、複数回キーを押さなければならないという問題が挙げられています。そこで、`aria-focus-combine`を指定した要素の子要素を、ひとまとまりで読まれるようにさせたいとのことです。

`role="text"`という似たようなセマンティクスを提供する role があったようですが、現在は非推奨のようです。  
[Using the "text" role - TPGi — a Vispero company](https://www.tpgi.com/using-the-text-role/)

### Radio/radiogroup relationship

https://github.com/w3c/aria/issues/2685

radiogroup role の子要素に 1 つ以上の radio role 要素が必要であることを明示したいという提案です。
方法として、「必須の子ロール」という項目を追加するか、既に（WAI-ARIA 1.3 で追加予定の）[suggestion role](https://www.w3.org/TR/wai-aria-1.3/#suggestion)で行われているように、ロールの説明文の中で ”MUST” の要件として記載する案が提案されています。

### Accessibility Conformance Testing (ACT) Rules Format 1.1 to transition to W3C Recommendation

https://github.com/w3c/wcag-act/issues/611

ACT Rules Format とは、アクセシビリティテストの手動テスト及び自動テストのルールのフォーマットを定義するものです。

ACT Rules Format 1.1 が今まで Candidate Recomendation だったのが、Recommendation に移行することが[AGWG 内の CfC (Call of Consensus) で承認されました](https://lists.w3.org/Archives/Public/w3c-wai-gl/2025OctDec/0144.html)。

### COGA research modules

https://raw.githack.com/w3c/coga/FPWD-research-modules/research-modules/index.html

COGA Task Force は、認知障害や学習障害があるユーザーのためのアクセシビリティの取り組みを行っています。

今回、Cognitive Accessibility Research Modules というドキュメントの Editor’s Draft の初版公開について、APA WG 及び AGWG 内で CfC が行われており、意見や賛同が募集されています。
このドキュメントは、Task Force 内で研究されている COGA 関連領域のモジュールをまとめたものになっています。

## JavaScript

### ECMA402

meeting note: https://github.com/tc39/ecma402/blob/main/meetings/notes-2025-12-04.md

#### Intl Locale Info API Proposal が Stage4 に

https://github.com/tc39/proposal-intl-locale-info

TC39 の 2025 年 11 月のミーティングで、Intl Locale Info API Proposal が Stage4 に進み、次回の ECMAScript 仕様書に採用されることが決まりました。

Intl Locale Info API Proposal は Intl.Local インスタンスに様々なロケールに関連する情報を返すメソッド生やす提案です。ここでいう「ロケールに関連する情報」には以下のようなものが含まれます。

- 週の始まりや週末の曜日
- 24 時間表記の方法
- 文書を書く方向(RTL か LTR かなど)
- サポートする暦
- サポートする命数法
- サポートするタイムゾーン

今までも Intl.Locale のインスタンスプロパティには、`calendar` や `numberingSystem` がありましたが、これらは「その Intl.Locale のインスタンスに設定された値」を返すのに対して、今回の Intl Locale Info API はあくまで「そのロケールで一般的に使用される値」を返してくれます。そのため API によっては優先度順で複数の値を配列で返すものもあります。MDN にはすでに[ページがある](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Intl/Locale/getCalendars)ので、ぜひどのようなメソッドがあるか知っておきましょう。

サポート状況ですが、現在主要ブラウザでは FireFox 以外のブラウザがサポートしています。

#### "languageDisplay" can be undefined in Intl.DisplayNames.prototype.resolvedOptions

https://github.com/tc39/ecma402/pull/1037

`Intl.DisplayNames` の `resolvedOptions` において、`languageDisplay` がオプションで指定されない場合、仕様的には内部スロットが `undefined` のまま残ってしまうというバグがあり、これに対して編集上の修正 Editorial Change を行うことになりました。実際にこの矛盾した仕様をそのまま実装しているエンジンは存在しないため実際の挙動に沿う形での変更となるようです、

#### iso8601 as DateTimeFormat calendar

https://github.com/tc39/ecma402/issues/1036

`Intl.DateTimeFormat`では `iso8601` カレンダーをサポートすることになっていますが、CLDR 側でこの挙動が安定しておらず、実装間で差異が生じてしまっている(Firefox は強制ハイフン区切り、Chrome はロケール依存など)問題についてです。

基本的に ISO8601 文字列が欲しい場合は toISOString などを利用すれば取得できるものの、広いロケールに対応する英語表記などで「日付部分は ISO 形式（YYYY-MM-DD）にしつつ、曜日などはロケールに基づいた名称で表示したい」といったニーズもあるため、これらを解消すべく CLDR のデザイングループと協議を継続することになりました。`iso8601` カレンダー指定時は`YYYY-MM-DD`や `HH:MM:S` などの主要な数値フィールドを固定しつつ、曜日やタイムゾーン名はロケール依存とする方向で調整が進められる予定です。

**Normative: Ignore "islamic-rgsa" calendar in DateTimeFormat**

https://github.com/tc39/proposal-intl-era-monthcode/pull/99

[以前の議論](https://zenn.dev/cybozu_frontend/articles/web_standards_trends_202510#which-hijri-calendars-should-temporal-support%3F)で暦として`islamic` を指定した場合、 `islamic-tbla`にフォールバックさせる方向で合意されていました。今回はもう一つの変種である`islamic-rgsa`についての扱いについて議論されました。

この`islamic-rgsa`は過去への遡及は可能でも将来を予測することが難しい暦でもあり現状利用実態の証拠がないため、将来的に再定義する余地を残す意味でも一度仕様から完全に削除（無視）することになりました。

また同時に、サポートするカレンダーのリストをエンジンが勝手に拡張できない “Closed Set” にするかの議論もなされ、“Closed Set” にする方向で合意が得られました。

#### Updates from the W3C i18n Group

Eemeli Aro 氏から、11 月に行われた W3C TPAC での議論について報告がありました。特に`en-ZZ`（標準単位を使用する米国英語）導入の議論と`<amount>`要素については注目されており、`en-ZZ`に関しては今後 CLDR の議題として正式に提出する準備を進められています。また`<amount>`要素に関しても[JS の Amount の提案](https://github.com/tc39/proposal-amount)を推し進めるきっかけになる可能性があります。

### ECMA429

https://ecma-international.org/publications-and-standards/standards/ecma-429/

WinterTC の Minimum Common Web API standard の第 1 版が publish されました。
`global` に公開されている関数や、 URL, File, Stream などのオブジェクト、WASM 系の API などが登録されています。

## Baseline

### 📃 December 2025 release notes

https://web-platform-dx.github.io/web-features-explorer/release-notes/december-2025/

## Misc

### First Public Working Draft: SHACL 1.2 Rules

https://www.w3.org/news/2025/first-public-working-draft-shacl-1-2-rules/

Data Shapes Working Group が SHACL (Shapes Constraint Language) 1.2 Rules の First Public Working Draft を公開しました。

SHACL は Semantic Web の文脈で利用される、RDF のグラフを記述するための言語です。

### Blink: Intent to Experiment: WebNN

https://groups.google.com/a/chromium.org/g/blink-dev/c/5CWKSChYo98/m/xMw0U5NkAAAJ

Web NN API は、OS やハードウェアを抽象化し、ブラウザ上で機械学習を可能にする API です。  
MLContextOptions API を用い、実行速度と電力消費の優先度を指定して実行できます。  
攻撃リスクを減らすために、デバイスの情報を直接出すことはありません。

Chrome と Edge でトライアルを開始し、Firefox も実装を予定しています。

### Save the date for BlinkOn 21!

https://groups.google.com/a/chromium.org/g/blink-dev/c/k3qAz6ADbik

Blink の開発者が集まるイベントが、来年 4 月 20-21 に開催されることが決定しました。

場所はワシントンの Microsoft オフィスで、配信・録画もあるようです。

### Vote for the web features you want to see

https://web.dev/blog/upvote-features

より広い Developer Signals の収集を目指して、[Can I use](http://caniuse.com) や [webstatus.dev](https://webstatus.dev) などに、該当機能の Issue を指す Thumb-up リンク（👍🏻）が追加されました。
機能の相互互換性を調べるついでに、実装者に興味を伝えやすくなるでしょう。

### Introducing A2UI: An open project for agent-driven interfaces

https://developers.googleblog.com/introducing-a2ui-an-open-project-for-agent-driven-interfaces/

Agent が動的に UI を生成するための標準フォーマットが OSS として公開されました。
A2UI を用いると、テキストベースではなく UI ベースでの Agent と対話が可能になります。

### Mozilla’s next chapter: Building the world’s most trusted software company

https://blog.mozilla.org/en/mozilla/leadership/mozillas-next-chapter-anthony-enzor-demeo-new-ceo/

Anthony Enzor-DeMeo が Mozilla の新しい CEO に就任しました。
ユーザーを第一に考える企業としての Mozilla の理念に基づき、プライバシー、データ利用、AIの透明性を確保した AI への投資や、検索以外の収益源の多様化について述べられています。
Laura Chambers 前暫定 CEO の功績にも感謝が示されており、彼女は Mozilla 取締役会に復帰する予定だそうです。

### WebKit Features for Safari 26.2

https://webkit.org/blog/17640/webkit-features-for-safari-26-2/

Safari 26.2 における WebKit の新機能の紹介記事です。

- Button commands: Invoker Commands
- Shrink wrapping form fields
- Anchor positioning improvements
- Color improvements
- `@scope`
- Navigation API
- View Transitions
- Cookie Store
