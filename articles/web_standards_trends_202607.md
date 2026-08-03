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
  - 主に ECMA262 周りのトピックを執筆
- [mehm8128](https://x.com/mehm8128)
  - 主にアクセシビリティに関連するトピックを執筆

:::

## HTML

### Intent to Prototype: `<input type="checkbox" switch>`

https://groups.google.com/a/chromium.org/g/blink-dev/c/QqbOHXhS8Rk/m/xKVpOwNiBAAJ

input要素をトグルスイッチとして扱うことができるようになるswitch属性のIntent to Prototypeが出ました。

`switch`属性を付与することでトグルスイッチの見た目に変わり、`role="switch"`が付与され、独自のCSS疑似要素を用いて見た目をカスタマイズできます。

もともと Open UI では `<switch>` 要素が検討されていましたが、現在 Explainer は inactive となっており、既存の `<input>` を拡張する方向へ移行しています。

Chrome での Prototype が進んでおり、現在はWebKitでのみ利用可能です。

### Intent to Ship: Renewed HTML insertion&streaming methods

https://groups.google.com/a/chromium.org/g/blink-dev/c/FAqz78iLGdc

HTML を更新するメソッドは、歴史的にバラバラでわかりにくく、そこにSanitizer APIやTrusted Typesを組み合わせたり、Streamを接続するようになり、より難しくなってしまいました。

そこで、`before`/`after`/`append`/`prepend`/`replaceWith`という位置指定のメソッドや、ストリームを繋ぐ`stream{Append}HTML{Unsafe}`などを統一的に使えるように整理する取り組みです。

### Intent to Prototype: Declarative fragments

https://groups.google.com/a/chromium.org/g/blink-dev/c/rjBSVWAGoHo

HTMLフラグメントを宣言的に定義する機能で、 `<template src>`などを用いて、従来は `<template>` に直書きする必要があったフラグメントを、外部からimport可能にするAPIです。

Sanitizeなども制御しやすいため、従来より安全に扱うことができるようになります。

### Intent to Prototype: Background work API

https://groups.google.com/a/chromium.org/g/blink-dev/c/GoTXujrV3YI

メディアアップロード、同期、更新など、ページがバックグラウンドにあっても止まって欲しくないタスクを実行し切るために、バックグラウンドタスクへのリソース制限を緩和するAPIが提案されています。

これまでは、音声再生やダミーのWebRTCを用いるハックで行っていましたが、明示的なAPIをネイティブで用意することで、ユーザの制御下で使えるようにする目的です。

## CSS

### Blink: Intent to Prototype: CSS mixins

https://groups.google.com/a/chromium.org/g/blink-dev/c/g0dSC7CvdlA

`@mixin`で再利用可能な宣言を`@apply`で使い回す、SASSなどで行われてきたCSSのMixinをネイティブで可能にするAPIです。

すでにフラグ付きで行われていた初期実装を、CSSWGの作業に合わせて調整するためのIntentsという目的です。

### Gecko: Intent to Ship: text-box-trim / text-box-edge

https://groups.google.com/a/mozilla.org/g/dev-platform/c/yCAFcvQ-Tkc/m/vUnKuduqBAAJ

テキスト要素の上下のスペース（ハーフレディング）を切り取ることができるtext-box-trim / text-box-edgeプロパティがGeckoでIntent to Shipになりました。

これによって主要ブラウザで実装が揃いますが、ブラウザごとに細かい挙動の違いが残っているようです。

### Blink: Prototype: Web Haptics API

https://groups.google.com/a/chromium.org/g/blink-dev/c/QTLE03g25oU

CSS と JavaScript から、ウェブインタラクションに対してセマンティックな Haptic Feedback を提供する API です。

デバイス固有の実装をせずに、「hint」「tick」「align」といった Haptic Intent を指定でき、ブラウザや OS が最適なフィードバックを提供します。

CSS による宣言的な指定と、JavaScript による命令的な制御の両方に対応しており、現在の `navigator.vibrate()` よりも幅広いデバイスへの対応が期待されています。

### Blink: Prototype: CSS `shrink-to-fit` container

https://groups.google.com/a/chromium.org/g/blink-dev/c/H8iIAHDHijc

ラップされたテキストの実際のレイアウトに合わせて、コンテナ幅を縮小できる提案中の機能です。

`fit-content` キーワードでは改行後も余白が残る場合がありますが、この提案では実際にレンダリングされた最大行長に合わせてコンテナサイズを決定できます。

`max-content-sizing: shrink-to-fit` が提案されており、「コンテナを内容に合わせる `shrink-to-fit`」と、「文字サイズをコンテナへ合わせる `text-fit`」を対になる機能として位置付けています。

## ARIA・WCAG

### Group Note: WCAG Evaluation Methodology (WCAG-EM) 2.0

https://www.w3.org/news/2026/group-note-wcag-evaluation-methodology-wcag-em-2-0/

ウェブサイトがWCAGの達成基準を満たしているかどうかを評価する手順をまとめたドキュメントであるWCAG-EMの2.0が公開されました。  
WCAG-EM 2.0ではウェブサイトだけでなく、ネイティブアプリ、キオスク端末、EPUB、PDFなど広範囲のデジタル製品を対象とするようになりました。  
また、WCAG-EM 1.0ではWCAG 2.0だけを対象の基準としていましたが、WCAG-EM 2.0ではWCAG 2.2までも対象としています。  
その他の変更点は以下のchange logをご確認ください。  
[w3c/wai-wcag-em](https://github.com/w3c/wai-wcag-em/#changelog)

### Add explicit language and direction metadata to `AriaNotificationOptions` · Issue #2828 · w3c/aria

https://github.com/w3c/aria/issues/2828

ariaNotifyにおいて、langとdirの情報を指定できるようにしたいという提案が出されています。  
ariaNotifyはdocumentもしくはelementに紐づけて実行できるので、基本的には紐づくdocumentもしくはelementの最も近い祖先要素で指定されているlangやdirの情報を用いて解釈されます（ref: [Element: ariaNotify() method - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Element/ariaNotify#language_selection)）。  
しかし、Google翻訳によるページ翻訳などを用いた場合に、ドキュメントのlang属性とDOM上の要素が変わったものの、ariaNotifyでアナウンスされるテキストが翻訳されなかった場合に、lang属性が変化する前の言語で読み上げられてしまいます。このような場合にはariaNotify自体でテキストのlangを指定する必要があるのではないかというようなユースケースが挙げられています。

### AGWGのCharterにFormal Objectionが出ている模様【7/25追記あり】 - 水底の血

https://momdo.hatenablog.jp/entry/20260720/1784538297

AGWGのCharterに対してFormal Objectionが出ていることを紹介している記事です。  
主に、WCAG3の進捗が遅いことに対する懸念や異議、それに関連してスコープの明確化などが挙げられているようです。

## JavaScript

### ECMA402

#### Take language subtag into account in locale hour cycle lookup — Scope expansion of the two-tiered lookup algorithm to include CalendarsOfLocale.

https://github.com/tc39/ecma402/pull/1086

Intl.Locale における時間周期（hour cycle）やカレンダーの検索において、言語サブタグが適切に考慮されていないという問題に対する対策が議論されました。

これらの時間周期やカレンダーなどは同じ地域でも使用する言語によって好まれるフォーマットが違うことがあります。

今回の議論でECMA402を担当するTG2としては言語サブタグを考慮に入れる方向に修正することで合意されました。これにより特定の言語文化に即したより正確なロケール情報の取得が可能になります。

#### Consider adding arcminute and arcsecond — Whether to include these units in this proposal vs. a standalone proposal.

https://github.com/tc39/proposal-intl-sequence-units/issues/13

複合単位（Sequence Units）のプロポーザルとして緯度・経度などで使われる「分」や「秒」を追加すべきか、それとも単体のProposalとして提案すべきかどうかの検討です。

似たような事例としてエネルギー単位（Energy Units）のProposalなどがあることも踏まえ、こちらの提案も必要であれば複合単位のプロポーザルとは別に提案することで合意されました。

#### Handling of zero-valued units in sequences — Architectural layer for zero-omission (NumberFormat option vs. Amount usage patterns).

https://github.com/tc39/proposal-intl-sequence-units/issues/11

複合単位を表示する際、値がゼロのフィールド（例：「5フィート0インチ」の0インチ部分）をどのように扱うか議論がなされました。

`Intl.DurationFormat` では0になるフィールドを表示するかで個別にオン/オフを切り替えるオプションがありますが、複合単位は対象となる単位数が多いため同様のアプローチは困難です。

議論の結果、APIをシンプルに保つためにもデフォルトの挙動としてゼロ値のフィールドを表示することに決定しました。その上で、必要があれば将来的にオプションの追加を検討する形となりました。

#### Should sequence units allow non-integer multiples? — Alignment with CLDR on permitting fractional unit multiples.

https://github.com/tc39/proposal-intl-sequence-units/issues/9

現在の複合単位の仕様案では「マイルとフィート」のように同じ単位系で整数倍の関係にある単位の組み合わせのみを許可していますが、CLDR側からは「メートルとインチ」のような組み合わせを禁止するのは厳しすぎるのではないかという意見も出ていました。

これに対して「マイルとメートル」のような組み合わせは実用上のメリットが薄く、多くの場合でユーザーの入力ミスである可能性が高いのを踏まえ、 Stage 2で承認された厳格な制限を維持することで合意されました。

#### Should we support time units?

https://github.com/tc39/proposal-intl-sequence-units/issues/8

複合単位や `Amount` プロポーザルにおいて、時間単位（時、分、秒など）をサポートすべきかという議論が行われました。

この議題に対して「既に `DurationFormat` や `Temporal.Duration` という優れた解決策が存在するため、重複する中途半端な機能を設けるべきではない」という立場と「CLDRにデータが存在し `Amount` としても表現可能である以上、特定の単位だけを例外的に除外するのは一貫性を欠く」という立場で大きく意見が分かれることになりました。

そのためこのTG2内での議論で結論を出すのではなく、TG1へエスカレーションすることとなりました。

#### Harmonize formatting option standarization between Unicode, ECMA, and WHATWG

https://github.com/tc39/ecma402/issues/1089

UnicodeのMessageFormat 2.0や、WHATWGが進めているHTMLの `<time>` 要素など、Webプラットフォーム全体で国際化対応が進む中、日時の書式設定オプションの名称や構造がバラバラになりつつあるという懸念について話し合われました。

この中でMessageFormat 2.0で導入された「Semantic Skeletons」という仕組みをJSの`Intl.DateTimeFormat` にも導入し、JSの側からこれを共通のビルディングブロックとして提供することでWebプラットフォーム全体での一貫性を保つべきではという意見が出ました。

この議論を踏まえ「Semantic Skeletons」をJSに導入するためのStage 1プロポーザルを作成することで合意がなされました。

#### Proposal: Intl.Locale.prototype.getExemplars()

https://github.com/tc39/ecma402/issues/1076

`Intl.Locale`にロケールで使用される典型的な文字セットを取得するAPIを追加する提案についてです。

こちらはユースケースによって利用する文字セットが違うロケールなどがある中で、開発者の真のニーズに合致しているかを精査するためCLDRと再確認することになりました。

### ECMA262

#### Await dictionary to stage 3

https://github.com/tc39/proposal-await-dictionary

複数の Promise をキー付きオブジェクトとして並列に await できる `Promise.allKeyed` と `Promise.allSettledKeyed` が Stage 3 に進んでいます。

`Promise.all` も処理自体は並列ですが、結果が配列の位置で対応しています。

分割代入の順序を取り違えてもエラーにならないということでバグの温床になっていました。

`Promise.allKeyed` では入力時に付けたキー名がそのまま結果に維持されるため、順序に依存せず名前で値を受け取れます。

```js
// Promise.all: 左辺と右辺の順序を揃える必要がある
const [shape, color, mass] = await Promise.all([
  getShape(),
  getColor(),
  getMass(),
]);

// Promise.allKeyed: キー名で対応づくので順序の取り違えが起きない
const { shape, color, mass } = await Promise.allKeyed({
  shape: getShape(),
  color: getColor(),
  mass: getMass(),
});
```

`Promise.allSettled` に対応する `Promise.allSettledKeyed` も同時に提案されています。

こちらは失敗しても throw せず `results.shape.status` のようにキーごとに結果を判別できます。

#### Fused Multiply-Add to stage 2

https://github.com/tc39/proposal-fma

IEEE 754 の融合積和演算を `Math.fma(x, y, z)` として追加する提案が Stage 2 に進んでいます。

`x × y + z` を計算する際に積の中間丸めを行わず、最終結果だけを 1 回丸めるのが特徴です。

IEEE 754 が必須とする 6 つの基本算術演算のうち、融合積和演算のみJavaScriptに欠けていました。

正しく実装するには数百行のコードが必要で、C/C++・Java・Python・Rust など他言語からの移植の障壁になっていました。

一般的な用途としては、内積の計算、行列の乗算、多項式の評価、およびニューラルネットワークへの応用などが挙げられます。

```js
// 2つの Number の正確な積を「上位 + 誤差」の2つに分解できる
const a = 100000001;
const b = 100000001;

// 真の積は 100000001² = 10000000200000001
// だが 2^53 を超える奇数は double で表現できない
const high = a * b; // 10000000200000000（丸めで末尾の 1 が消える）
const err = Math.fma(a, b, -high); // 1 ← 丸めで失われた誤差項を回収

// high + err = 10000000200000001 = 真の積（a × b = high + err が厳密に成立）
```

## Baseline

### 📃 July 2026 release notes

https://web-platform-dx.github.io/web-features-explorer/release-notes/july-2026/

## Misc

### Web Sustainability Guidelinesを読む会 #1/#2

https://zenn.dev/wsgj/articles/c856e5e5fe06fd

https://zenn.dev/wsgj/articles/web-sustainability-guidelines-2

W3CのSustainable Web Interest Groupが2025年10月に公開したWeb Sustainability Guidelinesを読む会の開催報告記事です。  
主にアクセシビリティに興味があるメンバーが定期的に集まって読んでおり、mehm8128もメンバーの1人として参加させていただいています。  
サステナビリティというとCO2や消費電力削減というイメージが強いかもしれませんが、アクセシビリティやプライバシー、セキュリティなど広範囲に渡ってガイドラインが整備されています。

第1回はAbstractからIntroductionまでを読んでガイドラインの目的や全体像を把握し、議論・意見交換しました。第2回はFilters機能を触り、Prosperity: Highに分類されているガイドラインを読み進めました。

Web Sustainability Guidelines自体の解説記事も合わせて公開されているので、ご覧ください。  
[Web Sustainability Guidelinesとは](https://zenn.dev/wsgj/articles/1f79582f108e02)

### Blink: ChromeStatus に Adoption ステージが追加

https://groups.google.com/a/chromium.org/g/blink-dev/c/7LlGyWsZs10

ChromeStatus の Shipping Stage に、新たに Adoption が追加されました。

機能を Ship するだけでなく、MDN や Baseline などのドキュメント整備やエコシステムへの普及状況もトラッキングし、開発者への機能浸透を促進することが目的です。

Chrome 150 以降の Feature Status から利用されています。

以下は[aria-actions - Chrome Platform Status](https://chromestatus.com/feature/5161589307867136)におけるAdoption ステージフィールドの例です。

![aria-actions での Adoption ステージフィールド。Adoption expectationとAdoption planの項目がある。](/images/web_standards-trends/202607-adoption-aria-actions.png)

### Blink: Baseline Features audit が Lighthouse に追加

https://web.dev/blog/baseline-lighthouse-features-audit

Lighthouse で、ページが利用している Web Platform 機能の Baseline 対応状況を一覧できる Audit が追加されました。

利用中の API が Baseline に含まれているかを確認でき、互換性の高い機能だけを利用しているかの確認が容易になります。

### Blink: Email Verification Protocol Origin Trial

https://developer.chrome.com/blog/email-verification-protocol-origin-trial

Email Verification Protocol の Origin Trial が開始されました。

メールに含まれる検証リンクをブラウザが安全に処理するための新しい仕組みで、メールアプリとブラウザ間で認証フローを標準化することを目的としています。

パスワードレス認証やメールリンクログインなどの UX 改善が期待されています。

### Gecko: Beta リリースフローを簡素化・2 週間リリースへ

https://groups.google.com/a/mozilla.org/g/dev-platform/c/2MxNM8zmNaI

https://groups.google.com/a/mozilla.org/g/dev-platform/c/qlaQ1YSlOP8

Firefox の Beta 版では、これまでの early beta / late beta を廃止し、単一の Beta へ統合されるようになります。

また、2026 年 9 月からはリリースサイクルを 4 週間から 2 週間へ短縮する実験が開始されます。

開発スピードを上げるのではなく、現在と同じ開発ペースのまま、より細かくリリースすることが目的で、Firefox 155 は 9 月 15 日ではなく 9 月 1 日のリリースを目標としています。

これで、実質 Chrome, Edge, FireFox のリリースは毎月 2 回行われることになります。

### Mozilla: State of Open Source AI Report

https://blog.mozilla.org/en/mozilla/mozilla-state-of-open-source-ai-report/

https://stateofopensource.ai/

Mozilla が、オープンソース AI の現状をまとめた初のレポートを公開しました。

オープンモデルとプロプライエタリモデルの性能差は約 3% まで縮小し、推論コストは過去 3 年で最大 50 分の 1 まで低下しています。

一方で、利用者の 79% がオープンモデルを採用しているものの、本番運用できているのは 53% に留まっており、インフラコストやセキュリティ、運用の複雑さが主な課題として挙げられています。

また、AI の競争軸はモデル自体から、モデルを活用するためのハーネスや周辺エコシステムへ移りつつあると分析しています。
