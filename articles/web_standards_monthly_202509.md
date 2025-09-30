---
title: "Web 標準動向 2025年9月版"
emoji: "🍡"
type: "idea"
topics: ["frontend", "cybozuwebstandards"]
published: false
publication_name: "cybozu_frontend"
---

こんにちは！ サイボウズ株式会社 フロントエンドエンジニアの [mehm8128 (@mehm8128)](https://x.com/mehm8128) です。

## はじめに

今月から「Web 標準動向」が始まります。

サイボウズは 2025 年 4 月より、W3C のメンバーに加入しました。

https://blog.cybozu.io/entry/joining-w3c

標準化プロセスに関わることができるようになるための最初の一歩として、フロントエンドエンジニアの一部のメンバーは積極的に Web 標準のキャッチアップを行っています。

そこで、毎月メンバーが興味を持った Web 標準に関する話題や、実際に標準化プロセスに関わることができた場合にはその報告などを 1 つの記事としてまとめ、紹介していきます。
また、ここでは W3C に限らず、TC39 や WHATWG などの標準化団体のトピックについても扱います。

:::message
今月の執筆者は以下の 4 名です。

- [Saji](https://x.com/sajikix)
  - ECMA402(Intl)周りのトピックを執筆
- [daiki](https://x.com/k1tikurisu)
  - ECMA262 周りのトピックを執筆
- [saku](https://x.com/sakupi01)
  - HTML, CSS, Interop
- [mehm8128](https://x.com/mehm8128)
  - 主に a11y に関連するトピックを執筆

:::

## HTML

### headingoffset & headingreset

https://github.com/whatwg/html/pull/11086

[https://html.spec.whatwg.org/multipage/sections.html#heading-levels-&-offsets](https://html.spec.whatwg.org/multipage/sections.html#heading-levels-&-offsets)

`headingoffset`と`headingreset`属性を仕様に追加する PR がマージされました。
モチベーションについては以下の記事で解説されています。

[`headingoffset` と `headingreset` 属性による見出しレベルの調整](https://blog.w0s.jp/entry/732)

テンプレートエンジンやコンポーネントを用いてマークアップを行うときに、使う側で heading level を指定し、その中で条件分岐することが必要になっていたことが課題であったと説明されています。

それに対する解決策として今回の属性が導入されました。`headingoffset`が指定された要素の子孫の見出し要素のレベルは「`headingoffset`+見出し要素の level」として計算されます。これにより、使う側で`headingoffset`を設定することで、条件分岐が不要になります。

関連して、CSS の疑似要素に`:heading`, `:heading(An+B)`を追加する PR もマージされました。

[[css-selectors-5] add :heading, :heading(An+B) pseudo classes](https://github.com/w3c/csswg-drafts/pull/11836)

### Intent to Prototype: Focusgroup

https://groups.google.com/a/chromium.org/g/blink-dev/c/DGFYzid2Qw8

Scoped Focusgroup の Prototype の意向が Blink で示されました。これは、OpenUI で以前インキュベーションされていた Focusgroup が、 Scoped Focusgroup として再出発したものです。

Scoped Focusgroup では、キーワードによる role の自動推論を可能にし、role に基づいた正しい focus 利用が強制されるよう、 API デザインの変更が加わっています。

そのほかにも、最後にフォーカスされた要素を記憶する no-memory、特定の子要素をフォーカス対象外にする focusgroup="none"の追加も検討されているようです。

Focusgroup は現在、 Non-active proposal となっています。

### Intent to Ship: Interest Invokers (the `interestfor` attribute)

https://groups.google.com/a/chromium.org/g/blink-dev/c/bX1G_yDt6W4

特定の要素にユーザが「（Hover や Focus などによって）Interest 」を示した際に、その要素が参照する要素をトリガーする、という挙動の標準化を目指す「Interest Invokers」が Blink で Ship される動きがありました。

TAG（Technical Architecture Group）が [Supportive な意向](https://github.com/w3ctag/design-reviews/issues/1058)を示したことによるものと思われます。しかし、主にタッチデバイスでの挙動に対する標準化について難しさが残り、WebKit は[慎重な姿勢](https://github.com/WebKit/standards-positions/issues/464#issuecomment-3345213176)を示しています。

Chrome は昨年からプロトタイプのイテレーションを重ねてきましたが、今回の Ship が標準化の前進を後押しするものとなれば良いなと思います。

### Customized built-in elements

現在、[is](https://developer.mozilla.org/ja/docs/Web/HTML/Reference/Global_attributes/is) 属性を用いて作成することができるカスタム組み込み要素の新しい形を求めて [MSEdgeExplainers](https://github.com/MicrosoftEdge/MSEdgeExplainers "https://github.com/MicrosoftEdge/MSEdgeExplainers") の更新が行われています。

カスタム組み込み要素は、`<button>`や`<input>`などを拡張する形でカスタム要素を作成することができる機能です。

[[Custom Elements with Native Element Behaviors] Add other considered alternative by anaskim · Pull Request #1153 · MicrosoftEdge/MSEdgeExplainers](https://github.com/MicrosoftEdge/MSEdgeExplainers/pull/1153)
[[Custom Elements with Button Activation Behaviors] Add minimal decomposed alternative by anaskim · Pull Request #1162 · MicrosoftEdge/MSEdgeExplainers](https://github.com/MicrosoftEdge/MSEdgeExplainers/pull/1162)

その中でも #1162 については w3c/aria のリポジトリで issue が開かれ、9/25 のミーティングで議論されました。

[Custom Elements with Button Activation Behaviors · Issue #2637 · w3c/aria](https://github.com/w3c/aria/issues/2637)
[ARIA WG – 25 September 2025](https://www.w3.org/2025/09/25-aria-minutes.html#d0af)

主に [anaskim が提示している 2 パターン](https://github.com/w3c/aria/issues/2637#issuecomment-3330165369) のうちどちらが良いかという議論でした。1 つ目のものは`buttonActivationBehaviors = true`と設定するだけで、デフォルトで`role=”button”`が割り当てられたりフォーカス可能になったりします。2 つ目のものは 1 つ目のものと比べて、role や tabindex などを開発者が自分で設定する必要がありますが、その分柔軟にカスタム要素を作成することができるようになっています。

ミーティングでは role や tabindex についてはデフォルトで割り当てられる 1 つ目の案で同意されたようですが、tabindex のリセット方法や formAssociated のデフォルト値についてはまだ議論が続きそうです。

また、WHATNOT ミーティングでも議論が行われ、[stage1](https://whatwg.org/stages) となりました。
[Upcoming WHATNOT meeting on 2025-09-18 · Issue #11664 · whatwg/html](https://github.com/whatwg/html/issues/11664#event-19901767241)
[Proposal: Customized built-in elements via `elementInternals.type` · Issue #11061 · whatwg/html](https://github.com/whatwg/html/issues/11061)

関連して、interop 2026 の proposal として Customized built-in elements が提出されています。
[Customized built-in elements · Issue #1152 · web-platform-tests/interop](https://github.com/web-platform-tests/interop/issues/1152)

## CSS

### **Group Note: CSS Snapshot 2025**

https://www.w3.org/news/2025/group-note-css-snapshot-2025/

CSS 仕様の安定性を示す「CSS Snapshot」が今年も公開されました。

特に、以下に代表される仕様の安定性が新しく明記されています。

- Reliable Candidate Recommendation
  - Cascade Layers を含む Cascading and Inheritance 5,
  - セレクタのチェックを含む Conditional Rules 4
- Safe to Release pre-CR Exceptions
  - Selector Level4 の `:is()`, `:where()`, `:has()`
  - Media Queries 5 の `prefers-reduced-motion`, `prefers-contrast`, `forced-colors` など

2025 年のスナップショット全体に関しては、以下の Group Note を参照ください。

[CSS Snapshot 2025](https://www.w3.org/TR/css-2025/)

### **Apple has a private CSS property to add Liquid Glass effects to web content**

https://alastair.is/apple-has-a-private-css-property-to-add-liquid-glass-effects-to-web-content/

Liquid Glass のためのプライベートな CSS Property が、iOS26 から WebKit WebView に限定されて追加されています。ただし、あくまでも Apple が内部的に利用するために用いられており、非公開の設定フラグをオンにすることが利用条件となります。

### Intent to Prototype: Declarative CSS module scripts

https://groups.google.com/a/chromium.org/g/blink-dev/c/aflQmg68ISU/m/ArMqnZvVAAAJ

Declarative CSS Module Scripts が Blink で Intent to Prototype になりました。

Chromium で利用可能な [CSS import attributes](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/import/with) の機能を宣言的に利用できるようにしたものです。
これにより、1 つのスタイルシートを複数の Shadow Root 間で効率的に共有できるようになります。

## ARIA・WCAG

### ARIA Notify

https://groups.google.com/a/chromium.org/g/blink-dev/c/QCtWzIPgcCY/m/RSuXobocDAAJ

[Add ariaNotify by janewman · Pull Request #2577 · w3c/aria](https://github.com/w3c/aria/pull/2577)
[Chrome 141 ベータ版  |  Blog  |  Chrome for Developers](https://developer.chrome.com/blog/chrome-141-beta?hl=ja)

ARIA Notify が Chrome 141 で利用可能になります。

ARIA Notify は、これまで ARIA ライブリージョンをハック的に用いて実装されていたスクリーンリーダーへの通知をブラウザネイティブで命令的な API で行うことができるようになるものです。

その他詳細は以下の記事で解説しています。
[命令的な ARIA ライブリージョン：ARIA Notify の紹介](https://zenn.dev/mehm8128/articles/aria-notify-introduction)

また、MDN のページも誕生しました。
[Element: ariaNotify() method - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Element/ariaNotify)

### WCAG 3 が 2025 年 9 月 4 日付で更新

https://www.w3.org/TR/2025/WD-wcag-3.0-20250904/

現在策定作業が行われている WCAG 3 の Working Draft が、2025 年 9 月 4 日付で更新されました。

WCAG 3 の概要や今回の変更点などは、以下のブログ記事にまとめられています。
[WCAG 3.0 (W3C Working Draft 2025 年 9 月 8 日版) | Accessible & Usable](https://accessible-usable.net/2025/09/entry_250915.html)

### **ISO/IEC 40500 が WCAG2.2 を採用する形で改定**

https://www.iso.org/standard/91029.html

WCAG2.0 がそのまま国際規格となっていた『[ISO/IEC 40500:2012](https://www.iso.org/standard/58625.html)』が、WCAG2.2 を採用する形で『[ISO/IEC 40500:2025](https://www.iso.org/standard/91029.html)』に改定されました。

これに合わせて、日本の規格である『JIS X 8341-3』の改定も動き出すことが考えられます。

『JIS X 8341-3』や ISO との関係については、WAIC の以下の記事が参考になります。
[アクセシビリティとは | ウェブアクセシビリティ基盤委員会（WAIC）](https://waic.jp/knowledge/accessibility/)
[JIS X 8341-3:2016 解説](https://waic.jp/docs/jis2016/understanding/201604/)

### aria-keyshortcuts: Do multiple shortcuts define a set of alternatives, or a single sequence?

https://github.com/w3c/aria/issues/2578

`aria-keyshortcut`の値を`aria-keyshortcuts="ctrl+a ctrl+b"`のようにスペースで区切った場合に、「ctrl+a **もしくは** ctrl+b」なのか、「ctrl+a **を押してから** ctrl+b」なのかで仕様の解釈が分かれるという議論です。

[ARIA WG – 18 September 2025](https://www.w3.org/2025/09/18-aria-minutes.html#402f) で議論されましたが、明確な結論は出ていないようです。

### Reference Target for Cross-Root ARIA · Issue #1035 · mozilla/standards-positions

https://github.com/mozilla/standards-positions/issues/1035

Reference Target gfor Cross-Root ARIA について、Mozilla が position: positive を示しました。

これは Shadow DOM 境界を超えて`aria-labelledby`や`aria-activedescendants`、label 要素の`for`属性などで HTML 要素を参照できるようにするものです。

これを受け、Webkit も position について今後再検討する可能性がありそうです。
[Reference Target for Cross-Root ARIA · Issue #356 · WebKit/standards-positions](https://github.com/WebKit/standards-positions/issues/356)

## JavaScript

### ECMA402

https://github.com/tc39/ecma402/blob/main/meetings/notes-2025-09-11.md

#### Updates from the MessageFormat Working Group

MessageFormat Working Group が Unicode CLDR Technical Committee (CLDR TC) に提出した仕様が、CLDR 48 として承認されました。

また Message 内の[Attributes](https://github.com/unicode-org/message-format-wg/blob/main/spec/syntax.md#attributes)についてより細かい仕様の検討も始まりました。Attributes はフォーマットに影響せず、コードコメントのような役割を果たすことが期待されている機能です。(下の例の`@translate=no`の部分)

> A _message_ including a _literal_ that should not be translated:
>
> ```
> In French, "{|bonjour| @translate=no}" is a greeting
> ```

#### **Updates from the W3C i18n Group**

前提 : ８月の meeting 時点で W3C i18n WG の Review を ECMA-402 提案の Stage を進める要件（Stage 2 で主要なレビューをリクエストし、Stage 3 前に再レビュー）として追加することに合意されていました。

今回`Intl.NumberFormat`および`Intl.PluralRules`で末尾の 0 を保持するという[提案](https://github.com/tc39/proposal-intl-keep-trailing-zeros)に関して W3C i18n Group にレビューをしてもらうよう調整しているようです。

#### Standard Measures with U.S. English

https://github.com/mozilla/explainers/blob/main/standard-measures-en-us.md

##### **前提**

「Standard Measures with U.S. English」は「U.S. English」を UI 言語として使用している非米国ユーザーが、米国の測定単位や日付形式などの慣習に起因する混乱や不便を解決するために`en-ZZ`という特殊なロケールを導入したいという提案です。

`en-ZZ`は「地域の不明な英語」というロケールを意味し、標準的な測定単位や日時の規則(週の始まりなど)を使用する U.S. English として扱われます。

##### 今回の議論

`en-ZZ`のようなロケールのニーズには一定の理解が得られたものの、以下のような内容について懸念が残るため更なる調査と議論が必要であるという結論になりました。

- このような解決策(ロケール外の使用)を汎用的な仕組みとすべきか、en に特化すべきなのか
- web 互換性のテストが必要であること(既存の言語タグに対する Unicode 拡張などの方法と比べてより互換性があるのか)
  - 参考: unicode 拡張には rg key(Region Override)がある
    - [Unicode Locale Data Markup Language (LDML)](https://www.unicode.org/reports/tr35/#Key_Type_Definitions)
- そもそも CLDR や ICU が日付パターンの override 機能をサポートするのか

#### Normative: Increase limits on Intl MV and explicitly limit significant digits

https://github.com/tc39/ecma402/pull/1022

`Intl.NumberFormat`における String Formatting の最小/最大許容値の制限を増やし、`Decimal`の全範囲をカバーすることを目的とした提案について議論されました。

基本的には`Decimal`との整合性などを鑑みて数値制限を拡大する方向で進めるものの、TG1(ECMA262 全体の仕様策定を行うグループ)からのフィードバックを求めた方が良いという結論になりました。

#### Normative: Make Intl.PluralRules ResolvePlural and associated AOs take Intl mathematical values rather than Numbers

https://github.com/tc39/ecma402/pull/1026

`Intl.PluralRules`において、[`ResolvePlural`](https://402.ecma-international.org/12.0/index.html?_gl=1*1ybkicy*_ga*Njc3ODA5MDUyLjE3NTUyNDU3NjY.*_ga_TDCK4DWEPP*czE3NTkxMTU1MDYkbzQkZzAkdDE3NTkxMTU1MDYkajYwJGwwJGgw#sec-resolveplural)などの関連する AbstractOperation は単なる Number だけでなく、Intl mathematical Value (≒ Number/ BigInt を含む)を受け取るべきだという修正案に関してです。

ToNumber は BigInt で例外をスローするので、この変更が入ると、BigInt を`Intl.PluralRules`で利用しても例外がスローされ無くなります。

今回の会議では大きな懸念がないとして、この修正案が承認されました。

#### Add an overflow option that rejects on lunisolar leap months in common years

https://github.com/js-temporal/proposal-temporal-v2/issues/36

[Intl era and monthCode Proposal](https://github.com/tc39/proposal-intl-era-monthcode) から派生した、太陽太陰暦において Temporal の`overflow`オプションの挙動をどうするのかという議題です。(Temporal の`overflow`オプションは日時の parse や計算メソッドに渡すことができるもので、日時として range を超えてしまった不正値を「自動で補正(constrain)」するか「例外を投げるか(reject)」を選べるようにするものです。)

太陽太陰暦においては閏月(leap month)が存在するため、「その年にあった閏月が翌年にはない」ということがしばしば起こります。このような状況で option として`reject`を選択し「閏月からちょうど１年後」といったような計算をした場合に、「エラーが投げられるべきなのか？」というのがこの問題の争点でした。

結論として、「`Overflow: reject`が厳格なエラー検出を意味する」という開発者の期待に基づき、厳格にエラーをスローするのが適切であるという合意がなされました。

一方でこの結果は以前 2.7 で承認された[Intl era and monthCode Proposal](https://github.com/tc39/proposal-intl-era-monthcode)への NormativeChange となるため、今後 TG1 の会議で検討されることになりました。また、場合によっては Temporal のカレンダー操作のアルゴリズムなどに広く影響する可能性があるのでより細かく分割して引き続き議論することになっています。

#### Editorial: Simplify NumberFormat

https://github.com/tc39/ecma402/pull/978

Intl.NumberFormat の仕様を簡略化したいという提案についてです。具体的には`Rounding Mode`に関する Table の統合や AbstractOperation の inline 化などにより記述量の削減削減と簡素化が見込めるとしています。

会議では可読性向上という方向性に関しては共有されたものの、以下のような点から結論は次回へと持ち越されました。

- [Amount Proposal](https://tc39.es/proposal-amount/)の影響により、`Rounding Mode`に関する記述が将来的に ECMA262 側に移動する可能性があること
- 具体的な変更アプローチについて意見の相違があったこと

### ECMA262

#### Uint8Array to/from base64 and hex が Baseline Newly Available に

7 月の TC39 meeting で Stage 4 になった、 [Uint8Array to/from base64 and hex](https://github.com/tc39/proposal-arraybuffer-base64) が、 [Chrome 140](https://developer.chrome.com/release-notes/140#uint8array_to_and_from_base64_and_hex) で実装され、 Baseline Newly Available になりました。

追加されたメソッドは次の通りです。

- [Uint8Array.fromBase64()](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array/fromBase64)：base64 文字列から Uint8Array を作成
- [Uint8Array.fromHex()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array/fromHex)：hex 文字列から Uint8Array を作成
- [Uint8Array.prototype.toBase64()](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array/toBase64)：Uint8Array を base64 文字列に変換
- [Uint8Array.prototype.toHex()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array/toHex)：Uint8Array を hex 文字列に変換
- [Uint8Array.prototype.setFromBase64()](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array/setFromBase64)：Uint8Array に base64 データを設定
- [Uint8Array.prototype.setFromHex()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array/setFromHex)：Uint8Array に hex データを設定

`toBase64()` / `fromBase64()` は、それぞれ `Window.btoa()` / `Window.atob()` よりも優先されます。

## Baseline

### Newly Available

- [content-visibility](https://developer.mozilla.org/ja/docs/Web/CSS/content-visibility)
- [rel="dns-prefetch"](https://developer.mozilla.org/ja/docs/Web/HTML/Reference/Attributes/rel/dns-prefetch)
- [parseHTMLUnsafe()](https://developer.mozilla.org/en-US/docs/Web/API/Document/parseHTMLUnsafe_static)
- [Uint8Array.fromBase64()](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array/fromBase64)
- [Uint8Array.fromHex()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array/fromHex)
- [Uint8Array.prototype.toBase64()](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array/toBase64)
- [Uint8Array.prototype.toHex()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array/toHex)
- [Uint8Array.prototype.setFromBase64()](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array/setFromBase64)
- [Uint8Array.prototype.setFromHex()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array/setFromHex)
- [URL Pattern API](https://developer.mozilla.org/docs/Web/API/URL_Pattern_API)

### Widely Available

- [font-variant-alternates](https://developer.mozilla.org/docs/Web/CSS/font-variant-alternates)
- [@font-feature-values](https://developer.mozilla.org/docs/Web/CSS/@font-feature-values)
- [HTML translate global attribute](https://developer.mozilla.org/docs/Web/HTML/Reference/Global_attributes/translate)
- [sin()](https://developer.mozilla.org/docs/Web/CSS/sin), [cos()](https://developer.mozilla.org/docs/Web/CSS/cos"), [tan()](https://developer.mozilla.org/docs/Web/CSS/tan), [asin()](https://developer.mozilla.org/docs/Web/CSS/asin"), [acos()](https://developer.mozilla.org/docs/Web/CSS/acos), [atan()](https://developer.mozilla.org/docs/Web/CSS/atan), [atan2()](https://developer.mozilla.org/docs/Web/CSS/atan2)

## Misc

### Request For Comments: new Resolver specification, groups & Aliases updates

https://www.w3.org/community/design-tokens/2025/09/12/request-for-comments-new-resolver-specification-groups-aliases-updates/

Design Tokens Community Group から、テーマ・モード関連の spec RFC が発表されています。

### interop/proposal_guide.md - web-platform-tests/interop

https://github.com/web-platform-tests/interop/blob/main/proposal_guide.md

クロスブラウザでの実装完了状態（Interoperable）を目指す 「Interop プロジェクト」の重点項目（Focus Areas）と調査項目（Investigation Areas）のプロポーザル募集が開始され、24 日に募集が締め切られました。

[全 124 件](https://github.com/web-platform-tests/interop/issues?q=is%3Aissue%20state%3Aopen%20label%3Afocus-area-proposal%20OR%20label%3Ainvestigation-effort-proposal)が Propose され、これから 12 月にかけて、選考期間に入るとのことです。

### Experiment: Proofreader API

https://groups.google.com/a/chromium.org/g/blink-dev/c/Gboyyec4qmg

Gemini Nano を活用した、入力テキストの校正と修正提案を行う Web API です。

### Unicode 17.0.0

https://www.unicode.org/versions/Unicode17.0.0/

毎年 9 月ごろに発表される Unicode の仕様が、今年も公表されました。

８つの新しい絵文字の追加、CJK （Chinese, Japanese, Korean） の拡充も進められています。

### **Ship: ICU 77 (supporting Unicode 16)**

https://groups.google.com/a/chromium.org/g/blink-dev/c/zzN7erylAc4

Chromium が汎用的な国際化機能の実装や Unicode サポートのために使用するサードパーティライブラリ ICU （International Components for Unicode）を v77 にアップデートしました。このアップデートは直近でリリースされた Unicode 16 や CLDR (Common Locale Data Repository) 46 へのサポートを含んだものとなっています。

### HTTP server API brainstorming · Issue #137 · WinterTC55/admin

https://github.com/WinterTC55/admin/issues/137

WinterTC において、 fetch API のレビューが一段落したので HTTP Server の API の策定が始まりました。

まずは必要な API について上げていくフェーズです。

### Gecko: Intent to prototype:Trusted Types

https://groups.google.com/a/mozilla.org/g/dev-platform/c/zQaRDA68e5A/m/XX_CRC4mAQAJ

Trusted Types が Gecko で Intent to prototype となりました。
Trusted Types は、`element.innerHTML`など脆弱性の原因となる DOM プロパティに文字列を代入する前に、安全な文字列に変換する処理を行ったという型付けができるようになるものです。

Safari には先日の Safari 26 のリリースで入ったので、Firefox で Ship されると全ての主要ブラウザでサポートされることになります。
