---
title: "Web 標準動向 2026年7月版"
emoji: "🎆"
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
  - 国際化まわりのトピックを執筆
- [saku](https://x.com/sakupi01)
  - HTML, CSS に関連するトピックを執筆
- [くらっち](https://x.com/kuracchi04)
  - 主にECMA262 周りのトピックを執筆
- [mehm8128](https://x.com/mehm8128)
  - 主にアクセシビリティに関連するトピックを執筆
- [コサキン](https://x.com/karintou74073)
  - 主に editing 周りのトピックを執筆

:::

## HTML

### W3C Invites Implementations of HTML Ruby Markup Extensions

https://www.w3.org/news/2026/w3c-invites-implementations-of-html-ruby-markup-extensions/

ルビの HTML 仕様「HTML Ruby Markup Extensions」が W3C のCandidate Recommendation Snapshot として公開されました。

大きな変更は、ルビのマークアップモデルの作り直しです。現行 WHATWG HTML のルビは、複数レベルの注釈や熟語ルビ、インラインへのフォールバックをうまく表現できませんでした。今回はベースと注釈の区切り方・対応付けを定義し直しています。

その一環で、これまで非適合だった `rb` と `rtc` が復活しました。`rtc` は複数レベルの注釈をまとめるため、`rb` はベースを明示的に区切るために必要だからです。ただし必須ではなく、`<ruby>霧<rt>きり</ruby>` のような単純なルビは今まで通り書けます。

### Blink: Intent to Ship: Reference Target

https://groups.google.com/a/chromium.org/g/blink-dev/c/eBx3sXIfJnw/m/9pUPEgpiAgAJ

Shadow DOM内の要素のIDを、Shadow DOMの外から参照できるようにする`shadowrootreferencetarget`属性が追加されます。

Shadow DOMの中から外の要素を参照することはARIAMixin属性を使ってJSからできたものの、その逆ができなかった問題を解消する属性になっています。
`<label>`要素の`for`属性や`aria-labelledby`、`popovertarget`などが対象となっています。

詳しくは以下の記事をご覧ください（当時の提案に基づく記事なので、現実装では仕様が変更されている可能性があります）。

[Referencing HTML elements inside Shadow DOM - HTMHell](https://www.htmhell.dev/adventcalendar/2025/4/)

### Blink: Intent to Prototype: Multi-Range Selection

https://groups.google.com/a/chromium.org/g/blink-dev/c/it0aKKGD5A0

Selection APIで、同時に複数の選択ができるように拡張する提案です。

Selection APIは、もともと “Multi Range” を扱うことを想定したAPIでしたが、2011年に “Single Range” のみへと縮小されました。しかしFirefoxではMulti Rangeがサポートされています。

Web Editing Working Groupでは、Multi Rangeを復活させ、ブラウザ間での実装を本来の設計意図に戻すための議論が進んでいます。

### Localized time formatting without JavaScript

https://github.com/whatwg/html/issues/12591

`<time>` 要素に新しい `format` 属性を追加し、UA Shadow Root経由でローカライズされた日時を自動表示する提案がStage1になりました。これによりサーバーサイドレンダリング（SSR）や静的レンダリングされたHTMLでもユーザーのタイムゾーンを加味した時刻を正確に表示できるようにします。

具体的には以下のようにtime要素に`"date"`, `"time"`, `"datetime"`のいずれかをとる`format`属性を追加することで実現することを提案しています。

```html
<time datetime="2026-06-17T08:13:41.099Z" format="datetime">
  (フォールバック表示)
</time>
```

さらに以下のような属性で細かいformatの調整も可能にすることを目指しています。

- `datefields` : 表示する日付構成要素（weekday, year, month, day など）
- `datelength` : 日付の長さ（long / medium / short）
- `timeprecision` : 時刻の精度（hour / minute / second）
- `timezonestyle` : タイムゾーン表示（long / short / なし）
- `hour12` : 12時間 / 24時間 / ロケールデフォルト（三値）
- `calendar` : 表示に使用するカレンダー（または "input" で入力値のカレンダーを使用）
- `timezone` : 変換先タイムゾーン（または "input" で入力値のタイムゾーンを使用）

ロケールは DOM を祖先方向に辿って `lang` 属性から決定され、フォーマットは `Intl.DateTimeFormat` のサブセットに限定される予定です。内部的には解析に Temporal API を使用することを想定しているようです。

issueに続く議論では好意的な反応がありつつも、細かいformatオプションに対する指摘や[Date Time String Format](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date#date_time_string_format) でフォーマットを指定した方がわかりやすいのではといった声も寄せられており、今後の動きも注目です。

### DOM Localization

https://github.com/whatwg/html/issues/12584

CSSのスタイルシートに類似したローカライゼーションコンテキストという概念を HTML に導入する国際化のための野心的な提案であるDOM LocalizationがStage1になりました。

具体的には各メッセージをMessageFormatで書いたメッセージリソースファイルを以下のように読み込むことを可能にします。

```html
<!-- イメージ（詳細仕様は incubation repo で検討中） -->
<head>
  <link rel="localization" href="messages.mf2" />
</head>
```

リソースファイルを読み込んだ上で、HTMLに新しい属性（名前空間付きの可能性あり）を追加し、DOM要素をメッセージにバインドすることでバインドされた要素の内容・翻訳可能な属性値は、対応するメッセージのフォーマット結果に置き換えられるようになります。

また`Intl.MessageFormat` を基盤とした JavaScript API を提供する予定であり、CSS同様宣言的にも、プログラム的にも定義可能なような形を目指しています。

詳細な仕様や設計を検討するため、新しくWHATWG内に[インキュベーションリポジトリ](https://github.com/whatwg/proposal-dom-localization) が作成されたので今後の動向はこのリポジトリを追うとよさそうです。

## CSS

### Blink: Intent to Ship: Relative Alpha Colors (CSS Color 5 alpha() function)

https://groups.google.com/a/chromium.org/g/blink-dev/c/fl5TIBDLHuo

指定した色のアルファ値(透明度)だけを変えることができる機能です。

```css
--mycolor: #aabbcc;
/* 透明度を 50% にする */
--mycolor-half: alpha(from var(--mycolor) / 50%);
/* 以下も同じ */
--mycolor-half: alpha(from var(--mycolor) / calc(alpha * 0.5));
```

### Chrome 150 Beta

https://developer.chrome.com/blog/chrome-150-beta

Chrome 150 の Beta が公開されました。CSS まわりを中心に注目度の高い機能を取り上げます。

- **CSS** `text-fit` **property:** フォントサイズを、コンテナの幅にぴったり収まるよう自動でスケール可能に
- **CSS background-clip: border-area:** 背景を、ボーダーが描画する領域にクリップする
- **Comma-separated container queries:** `@container` ルールにカンマ区切りで複数のクエリを書けるようになり、1つでもマッチすれば適用される。未サポート機能向けのフォールバッククエリが書ける、という用途が考えられる
- **The** `focusgroup` **attribute:** メニューなどのコンポーネントにおいて、roving tabindex を実装せず、フォーカスの挙動を宣言的に実装できるように
- **popover=hint behavior changes:** Customizable Select を `popover=hint` 内に置けるように
- **Relative alpha colors:** 指定した色はそのままに、透明度を相対的に指定可能になる
- `flex-wrap:balance`**:** `text-wrap:balance` のように、フレックス行間でコンテンツをバランスよく分配
- `named-feature()` **function for CSS** `@supports`**:** 他機能との組み合わせ時や特定のコンテキストで期待される挙動など、`@supports` の仕組みでは検知できない挙動に対してラベルを定義し、`@support` で検知可能にする

このほか CSS では、`url()` に `cross-origin()` / `integrity()` / `referrer-policy()` を付けてフェッチ挙動を制御できる **CSS URL request modifiers**、印刷時の余白の安全領域を扱う `page-margin-safety` なども入っています。

### Web Technology Sessions at WWDC26 | WebKit

https://webkit.org/blog/17974/web-technology-sessions-at-wwdc26/

WWDC が今年も開催され、 WebKit チームからウェブ領域において注目の 6 つのセッションが公開されました。CSS の新しいレイアウトモデルである Grid Lanes、Customizable Select Element、3D モデルを描画するための model element、などをカバーしています。長年開発者から需要があった機能の追加に加え、Safari 27 では多くのバグ修正や改善が入り、今年は品質が前面に押し出されていたのが印象的でした。

特に Web UI として注目できる機能は次の2つです。

- [Introducing the Field Guide to Grid Lanes | WebKit](https://webkit.org/blog/18098/introducing-the-field-guide-to-grid-lanes/)
- [The golden rule of Customizable Select | WebKit](https://webkit.org/blog/18117/the-golden-rule-of-customizable-select/)

### CSS Linked Parameters Module Level 1

https://drafts.csswg.org/css-link-params-1/

外部リソース（特に SVG 画像）に CSS の値を渡し、リンク先で CSS Env Variables として使えるようにする仕様が、First Public Working Draft として公開されました。

外部の SVG 画像を、元ファイルを書き換えずにサイトのテーマカラーなどに合わせて再利用できるようになります。値を渡す側と受け取る側はおおむね以下のようになります。

```html
<!-- 受け取る側（SVG） -->
<svg>
  <path fill="env(--color, black)" d="..." />
</svg>
<!-- パラメータを渡す側 -->
<img src="image.svg#param(--color, blue)" />
```

```css
/* CSS から渡す場合 */
.icon {
  background-image: url("image.svg"param(--color, var(--primary-color)));
}
/* 要素や CSS 経由の外部リソースにまとめて渡す link-parameters プロパティも */
.icon {
  link-parameters: param(--color, blue);
}
```

Firefox は Nightly に Experimental で実装されているとのことです。

[2022783 - Implement the link-parameters property.](https://bugzilla.mozilla.org/show_bug.cgi?id=2022783)

## ARIA・WCAG

### Blink: Intent to Ship: aria-actions

https://groups.google.com/a/chromium.org/g/blink-dev/c/DNE6dB1AS0Y/m/Mqxjk6GwAAAJ

新しいARIA属性である`aria-actions`のIntent to Shipが出ました。

ARIA 1.3から入ることが検討されていて現在[議論中](https://github.com/w3c/aria/issues/2691)である属性で、インタラクティブな要素の補助的なアクションを、そのインタラクティブな要素に紐づけることができるものです。

例えば以下の例では、タブの中に閉じるボタンがあるように、インタラクティブな要素の中に二次アクションが存在する場合があります。

![VSCodeで、Tabの中にタブを閉じるバツアイコンのボタンがあるパターン](/images/web_standards-trends/202607-aria-actions.png)

このようなパターンでタブにフォーカスした際に、支援技術が閉じるボタンを認識できないといった課題がありました。

[以前も紹介したように](https://zenn.dev/cybozu_frontend/articles/web-standards-trends-202605#publishing-a-second-aria-1.3-working-draft)、APGには既にexperimentalな例があります。

[Experimental Example of Tabs with Action Buttons | APG | WAI | W3C](https://www.w3.org/WAI/ARIA/apg/patterns/tabs/examples/tabs-actions/)

`aria-actions`はこの二次的なアクションの存在を伝えるための属性です。

Firefoxはすでに実装しており、Safariもプロトタイプ実装を進めています。

### WCAG 2.2 日本語訳 更新のお知らせ | ウェブアクセシビリティ基盤委員会（WAIC）

https://waic.jp/news/20260608/

WCAG 2.2のWAICによる日本語訳が更新されました。

JIS X 8341-3の改正に向けた検討過程で翻訳が見直され、より原文に合うようになりました。記事中では達成基準の名称変更がリストアップされていますが、その他本文についても全体的に様々な改善が入っているようです。

詳しくは、実際に翻訳作業に関わっているお二人の記事をご覧ください。

- [WCAG 2.2 日本語訳の更新 (達成基準名称を含む全体的なブラッシュアップ) | Accessible & Usable](https://accessible-usable.net/2026/06/entry_260608.html)
- [WCAG 2.2日本語訳の大幅更新 | アクセシビリティBlog | ミツエーリンクス](https://www.mitsue.co.jp/knowledge/blog/a11y/202606/08_1003.html)

## JavaScript

### ECMA402

#### Normative: Allow Locale.p.getNumberingSystems to return >1 item

https://github.com/tc39/ecma402/pull/1074

Intl.Locale オブジェクトのインスタンスメソッドである`getNumberingSystems()`が仕様上最初の1つしか返していなかった問題についてです。

内部で使われているCLDRではロケールごとに複数の数字システムが定義されているのにも関わらず、既存のAPIでは情報が十分に提供されていなかったということで、CLDRと一致させ開発者がロケールで利用可能なすべての数字システムにアクセスできるようにする方針で合意されました。

#### Normative: Avoid default locale fallback within Locale.p.getCollations()

https://github.com/tc39/ecma402/pull/1072

言語が未定義(und)の時や見つからないときに、Intl.Locale オブジェクトのインスタンスメソッドである`getCollations()`がホスト（環境）のデフォルトロケールにフォールバックしてしまうのを修正したいというものです。

このような場合、基本的にはCLDRにおいて定義されているroot(特定の言語や地域の特徴を排除した、中立的な動作)の値を返すべきであることが確認され合意されました。

#### Editorial: Revise GetOption to trigger a TypeError for absent mandatory options

https://github.com/tc39/ecma402/pull/1068

必須オプションが欠けている場合に出るエラーが`TypeError` ではなく `RangeError` になっているという指摘です。仕様の抽象操作である`GetOption`が`RangeError`を使用していたことが起因しており、Temporalもこの仕様を引き継いでしまっていることも指摘されています。

ECMA402側では`TypeError`に揃えることに合意し、併せて Temporal 側でもエラー型を修正するためのPRを作成することが合意されました。

#### Normative: Synchronize RegionPreference and CanonicalUnicodeSubdivision with UTS 35 standards

https://github.com/tc39/ecma402/pull/1059

カレンダーや時間周期の取得で言語タグのUnicode拡張地域上書き(`u-rg`)において、どこまでサポートすべきかをUTS 35(LDML)と同期して明確化することに対する議論がされました。

当初は規範的な変更(Normative Change)として変更することも検討していましたが、変更の影響範囲が広く、CLDRとの詳細な調整が必要であるため独立した「プロポーザル」として議論を継続することになりました。

#### Examine the consequences of Henri's findings regarding the cessation of support for language subtags exceeding three characters in ECMA-402

https://github.com/tc39/ecma402/issues/951

3文字を超える言語サブタグ（例：「posix」など）のサポートを停止すべきかという長期的な課題について話し合われました。

全ページロードの0.0021%で依然としてこれらのタグが使用されているというデータがあるものの、実際にそのデータが実際のレガシーな用途で使われてるかどうかは怪しく、引き続きHTTP Archiveなどを用いたさらなる調査が必要であることが確認されました。

#### Temperature check: Integration of identityFallback option in Intl.NumberFormat#formatRan

https://github.com/tc39/ecma402/issues/1064

Intl.NumberFormatの`formatRange`の現在の挙動では、開始と終了の値が一緒の場合、「約2」と言った概数であることを示すフォーマット結果になります。数値を丸めた結果であればこのような表記は妥当ですが、本当に２値が同一だった場合あまり適切であるとは言えないため、「入力値や出力結果が同一だった場合のフォールバック挙動を制御するオプション」の導入を検討するのはどうかという提案です。

会議を通して範囲（range）または単一値（single）へのフォールバックすることに対しては前向きであるものの、比較ロジックの複雑さから明示的な近似表記の導入は慎重に検討することになりそうです。

#### Temperature check: Advancing Intl.PersonNameFormat for culturally aware name formatting

https://github.com/tc39/ecma402/issues/1064

文化的に適切な姓名の順序や敬称の扱いを実現する Intl.PersonNameFormat APIの提案です。ひとまずは提案者に対し、具体的な需要や「現状のWebで名前のフォーマットが失敗している例」などを収集するべきだというフィードバックがされています。

#### Evaluation of time unit support

https://github.com/tc39/proposal-intl-sequence-units/issues/8

複合単位（Sequence Units）プロポーザルに「時間単位（時、分、秒）」を含めるかどうかの検討がされました。

時間単位のフォーマットは既に `DurationFormat` で解決されており、`Amount`（数値と単位のペア）にこれらを含めるとタイムゾーンやカレンダーの問題が発生するという懸念が上がったものの、これらの単位は「緯度・経度」などでも使われており、CLDRにデータが存在する以上含めるべきだという意見も挙げられました。

結果的に緯度経度を表す「度・分・秒（arcminute/arcsecond）」をサポート対象テーブルに追加する方向で調整することになりました。

### ECMA262

#### Normative: add Iterator.zip and Iterator.zipKeyed

https://github.com/tc39/ecma262/pull/3000

複数のイテレータを同時に回して 1 つにまとめる `Iterator.zip` と`Iterator.zipKeyed` が、`Stage 4` に到達し、PRがマージされました。

`Iterator.zip` は各イテレータから 1 つずつ値を取り出して配列にまとめます。

```
const a = [1, 2, 3]; const b = ["x", "y", "z"]; for (const [n, s] of Iterator.zip([a, b])) { console.log(n, s); } // 1 "x" // 2 "y" // 3 "z"
```

`Iterator.zipKeyed` は配列ではなくキー付きオブジェクトでまとめます。

```
const users = Iterator.zipKeyed({ id: [1, 2], name: ["Alice", "Bob"], }); for (const user of users) { console.log(user); } // { id: 1, name: "Alice" } // { id: 2, name: "Bob" }
```

#### Layering: Allow ParseScript and ParseModule to be called with string input

https://github.com/tc39/ecma262/pull/3849

`ParseScript` と `ParseModule` は、ソーステキストをパースして `Script / Module` レコードを生成する `ECMAScript` 仕様内の抽象操作です。

従来の仕様では引数の前提条件が実態とズレており、ホスト側はすでに `ParseText` を介して文字列を渡して利用していました。今回の変更で`ParseScript / ParseModule` が文字列入力を正式に受け取れるよう記述が整えられ、仕様の整合性が取れた形です。

## Baseline

### 📃 June 2026 release notes

https://web-platform-dx.github.io/web-features-explorer/release-notes/june-2026/

## Misc

### W3C initiates leadership transition

https://www.w3.org/press-releases/2026/w3c-leadership-transition/

W3CのCEOが、Seth Dobbs氏からDominique Hazaël-Massieux氏に交代しました。

### Web Engines Hackfest 2026

https://webengineshackfest.org/

6/15～17にスペインにて、Web Engines Hackfest 2026が開催されました。

[wptにおけるAccessibility APIのテスト](https://webengineshackfest.org/#wpt-accessibility)や[DOM Localization](https://webengineshackfest.org/#dom-localization)、[HTML in Canvas](https://github.com/Igalia/webengineshackfest/issues/76)、[Servo](https://github.com/Igalia/webengineshackfest/issues/96)などTPAC 2025でも議題に上がっていたようなトピックが多く見られました。

- [That's a Wrap on the 2026 Web Engines Hackfest | Igalia](https://www.igalia.com/2026/webengineshackfest.html)
- [agenda: Home · Igalia/webengineshackfest Wiki](https://github.com/Igalia/webengineshackfest/wiki)
- [Web Engines Hackfest 2026 - YouTube](https://www.youtube.com/live/EFp-A7T4c0U)

### Introducing the MDN MCP server | MDN Blog

https://developer.mozilla.org/en-US/blog/introducing-mdn-mcp-server/

MDNの情報を取得できるMCPサーバーが公開されました。

このMCPサーバーを使わずにWeb技術についての情報をAIで取得すると、技術自体の説明についてはMCPサーバーを使った場合とあまり変わらないものの、ブラウザの現在のサポート状況については古いものを参照してしまうとのことです。このMCPサーバーを使うことで、ブラウザのサポート状況についても速く正確に取得できるようです。

### Call for Participation in No Build Community Group

https://www.w3.org/community/nobuild/2026/06/21/call-for-participation-in-no-build-community-group/

W3Cに新しいCommunity GroupであるNo Build Community Groupが発足しました。  
依存関係やビルド手順を最小化することを探求するグループです。  
既にいくつか提案が行われており、JavaScriptのType Annotationや`<link>`要素の機能追加、Declarative Partial Updates、Open UI componentsなどが議論の候補として挙げられています。

- [Spec proposals of interest: What emerging web standards should we be pushing for? · no-build-cg · Discussion #2](https://github.com/orgs/no-build-cg/discussions/2)
- [What standards should we propose for Interop 2027? · no-build-cg · Discussion #3](https://github.com/orgs/no-build-cg/discussions/3)

### Unlock runtime insights: Introducing third-party developer tools for Chrome DevTools for agents

https://developer.chrome.com/blog/devtools-for-agents-3p-tools

Chrome DevTools for agents に、フレームワークの内部状態やコンポーネントの詳細を AI エージェントへ共有できる機能が追加されました。フレームワーク側が Discovery API を使ってツールを実装することで、これまで難しかった内部状態の絡むデバッグをできるようにする、というものです。

実装パートナーとして Angular チームが協力しており、状態とビューの依存関係を可視化して無限ループや更新失敗の原因を特定する Signal Graph tool などを実装しています。本機能は `chrome-devtools-mcp` v0.25.0 以降の実験的機能で、`--categoryExperimentalThirdParty` フラグで有効化することで利用可能です。

### A developer toolkit to make your website agent-ready

https://developer.chrome.com/blog/agent-ready-toolkit

AI エージェントにも高い操作体験を提供するための開発者向けツールキットの紹介です。注目として、 Lighthouse の新カテゴリ Agentic browsing と、Chrome DevTools for agents の組み合わせです。

Lighthouse の Agentic browsing の項目では、「アクセシビリティ」「CLS」「Web MCP のツール登録状況」から、サイトがどれほど AI フレンドリーかを測定するとのことです。

ただし、エージェント Web の標準がまだ形成途上のため、あくまでもランキングやスコアリングの目的というよりも、データ収集と実用的なシグナル提供を優先するという方針とのことです。

### PACT: Anonymous Credentials for the Web - Mozilla Hacks - the Web developer blog

https://hacks.mozilla.org/2026/06/pact-anonymous-credentials-for-the-web/

Mozilla が、サイトがユーザーの身元やデバイス情報に依存せず、ボットの大量アクセスを防げる匿名クレデンシャルの仕組み **PACT（Private Access Control Tokens）** の設計を発表しました。Cloudflare や Chrome など他ベンダーなどと連携して進めていくとのことです。

[Keeping the web open and private in the bot era | The Mozilla Blog](https://blog.mozilla.org/en/privacy-security/keeping-the-web-open-and-private-in-the-bot-era/)

### Faster updates, enterprise-friendly schedule: the new Microsoft Edge release cycle - Microsoft Edge Blog

https://blogs.windows.com/msedgedev/2026/06/11/faster-updates-enterprise-friendly-schedule-the-new-microsoft-edge-release-cycle/

Microsoft Edge も Chrome と同様にリリースサイクルを短縮し、Edge 152（Stable は 8/27）から、Stable チャネルが 2 週間ごとのリリースになります。1回あたりの変更量は半分になり、より小さく頻繁な更新にするとのことです。

ただし、エンタープライズ向けの Extended Stable は 8 週間サイクルのまま変更なしで、4 リリースごとに更新されます。Chromium ベースゆえに Chromium のセキュリティパッチが Chrome のスケジュールで流れてくるため、これまでの 4 週間サイクルだと同じパッチが Chrome より最大 4 週間遅れる可能性があり、Chrome に合わせて 2 週間にすることでそのギャップを埋める意図です。なお Chrome 自身の 2 週間化は Chrome 153（9/8）からなので、Edge（8/27）の方がわずかに早い切り替えになります。

### W3C Japan 30年の軌跡、そして次世代へ： 村井先生×青野社長が問う、日本が人類に果たすべき役割とは？ - Cybozu Inside Out | サイボウズエンジニアのブログ

https://blog.cybozu.io/entry/w3c-cybozu-discussion-jp-meeting

2026 年 5 月 14 日に慶應義塾大学で開催された第1回 W3C 日本会員会議の中で、「W3C Japan 30 周年特別対談」として、慶應義塾大学の村井純先生とサイボウズの青野慶久社長の対談が行われました。
当日のレポート記事が、Cybozu Inside Outにて公開されました。
