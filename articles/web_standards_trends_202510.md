---
title: "Web 標準動向 2025年10月版"
emoji: "🎃"
type: "idea"
topics: ["frontend", "cybozuwebstandards"]
published: false
publication_name: "cybozu_frontend"
---

こんにちは！ サイボウズ株式会社 フロントエンドエンジニアの [mehm8128 (@mehm8128)](https://x.com/mehm8128) です。

## はじめに

サイボウズは 2025 年 4 月より、W3C のメンバーに加入しました。

https://blog.cybozu.io/entry/joining-w3c
https://blog.cybozu.io/entry/w3c-interview-part1
https://blog.cybozu.io/entry/w3c-interview-part2

標準化プロセスに関わることができるようになるための最初の一歩として、フロントエンドエンジニアの一部のメンバーは積極的に Web 標準のキャッチアップを行っています。

そこで、毎月メンバーが興味を持った Web 標準に関する話題や、実際に標準化プロセスに関わることができた場合にはその報告などを 1 つの記事としてまとめ、紹介していきます。
また、ここでは W3C に限らず、TC39 や WHATWG などの標準化団体のトピックについても扱います。

:::message
今月の執筆者は以下の 4 名です。

- [Saji](https://x.com/sajikix)
  - ECMA402 (Intl) 周りのトピックを執筆
- [daiki](https://x.com/k1tikurisu)
  - ECMA262 周りのトピックを執筆
- [saku](https://x.com/sakupi01)
  - HTML, CSS に関連するトピックを執筆
- [mehm8128](https://x.com/mehm8128)
  - 主に a11y に関連するトピックを執筆

:::

## HTML

### Blink: Intent to Ship: Scoped Custom Element Registry

https://groups.google.com/a/chromium.org/g/blink-dev/c/mAteNymnc_s/m/3donjH1CBwAJ

Scoped Custom Element Registry が Blink で Intent to Ship になりました。

同じドキュメント内で同じ名前の違うカスタム要素が定義されてしまうのを防ぐために、カスタム要素の名前空間を区切って定義できるようにするものです。

主な用途として、3rd Party 製 Web Components を組み込む場合に、カスタム要素の命名の競合を防ぐといった使い方が挙げられます。

Explainer: [webcomponents/proposals/Scoped-Custom-Element-Registries.md at gh-pages · WICG/webcomponents](https://github.com/WICG/webcomponents/blob/gh-pages/proposals/Scoped-Custom-Element-Registries.md)

### Proposal: use relatedTarget for click events originated from labels

https://github.com/whatwg/html/issues/11800

`<input>`に紐づけている`<label>`をクリックしたとき、`<input>`側で発火される間接的なクリックイベントの`relatedTarget`に、クリックされた`<label>`を割り当てるようにしたいという proposal です。

これが実現されると、`<input>`を直接クリックしたときと`<label>`経由でクリックされたときが区別できるようになります。

### Blink: Intent to Prototype: Customizable combobox

https://groups.google.com/a/chromium.org/g/blink-dev/c/lRZgUn9evQo/m/hrlHAM0RAQAJ

Customizable Select の拡張版 UI パターンとも言える、Customizable Combobox のプロトタイプの意向が示されました。Combobox は、テキスト入力とそれに応じたフィルタリングが可能な、拡張版 Select パターンとも言うことができるものです。

直近の議論にて、これまでの `<combobox>` 要素を導入する提案から、既存の `<input>` + `<datalist>`/ `<select>` を拡張する方針に設計が大きく変更されました。これは、`appearance: base;` を利用する Customoizable Select の設計を踏襲したものでしょう。

このように、Customoizable Combobox は鋭意仕様策定段階です。

### Gecko: Intent to prototype and ship: HTMLInputElement showPicker() for textual input with datalist

https://groups.google.com/a/mozilla.org/g/dev-platform/c/hn3MGx3GBRY/m/q_S7ChtqCAAJ

`<input input="text" list="id">`に対して`showPicker()`が実行されたときに、紐づけられている`<datalist>`が表示されるようにするものです。

今回の Gecko での Ship により、主要４ブラウザでのサポートが期待されます。

### Deprecate and Remove: Deprecate and remove XSLT

https://groups.google.com/a/chromium.org/g/blink-dev/c/CxL4gYZeSJA

以前から議論されていた XSLT Removal のプロポーザルが、 WHATWG で stage3 に達し、Chrome で Deprecation&Removal の意向が示されました。

コミュニティのポリフィルと拡張の提供、console warning での警告、Stable ビルド以外でのデフォルト Removal、キルスイッチとも言える OT や Enterprise Policy の提供を経て、 2027 年にようやく完全廃止を試みるプランです。

2 年間かけた非常に慎重なロールアウトでの Removal が見込まれます。

### Proposal: moveOrInsertBefore() · Issue #1406 · whatwg/dom

https://github.com/whatwg/dom/issues/1406
状態を保持したまま DOM の移動が行える `moveBefore()` は、フレームワーク開発者側からの期待も大きい機能です。しかし、仕様上の制約が故に、フレームワーク側が同等の条件式や `try-catch` を記述して `moveBefore()` を用いた実装をせねばならない状況でした。

そのため、`moveBefore()` が使える場合は使うようにし、使えない場合は `insertBefore()` にフォールバックするための、新しい DOM API として、 `moveOrInsertBefore()` が提案されています。

### **Disposable AbortController · Issue #1405 · whatwg/dom**

https://github.com/whatwg/dom/issues/1405

Explicit Resource Management では、スコープを抜ける際にリソースを自動的にクリーナップできるようになります。

dispose 時に自動的に`abort()`を呼んで、実行中の非同期処理をキャンセルしたいという提案です。

## CSS

### Blink: Intent to Prototype: The `inherit()` CSS function

https://groups.google.com/a/chromium.org/g/blink-dev/c/nCUNGmfAOMU/m/YOCfBQV6BwAJ

`inhert()` が Blink で Intent to Prototype になりました。

`var()` ではプロパティ、特にカスタムプロパティの自己参照はできません。しかし、`inherit()` では、指定された「任意プロパティ」の「直親要素」での「計算値」を適用可能です。

ただし、今回の Prototype では「任意プロパティ」ではなく、**カスタムプロパティのみ** のサポートとなっています。

親要素に相対的な `border-radius`、カラー、親要素の前景色と背景色を反転させた色合わせなど、考えられるユースケースは多岐に渡るため、Issue の Original Post でも多くのリアクションを集めていたものでした。

詳細は以下の記事を参照ください。

https://blog.sakupi01.com/dev/articles/css-inherit

### Gecko: Intent to ship: CSS text-autospace property

https://groups.google.com/a/mozilla.org/g/dev-platform/c/69xH8RcOULw/m/CJ1j9F9qAAAJ

`text-autospace` プロパティが Gecko で Intent to ship になりました。

これで主要ブラウザ全てで `text-autospace`プロパティが利用できるようになりますが、初期値に関しては Firefox も他と同じく `no-autospace`（デフォルトで間隔をあけない） にするようです。  
これはどのブラウザも仕様に準拠しない（仕様のデフォルトは `normal` で、間隔をあける）という状況になりますが、この実装は CSSWG の RESOLUTION により許容されたものです。初期値に関しては、 CSSWG にて引き続き議論が行われています。

[[css-text] Reconsider the initial value of the `text-autospace` property · Issue #12386 · w3c/csswg-drafts](https://github.com/w3c/csswg-drafts/issues/12386#issuecomment-3028532137)

### Prototype: `text-justify` CSS property

https://groups.google.com/a/chromium.org/g/blink-dev/c/507si_plqIw

`text-align: justify;` は、テキストの均等割付けを行いますが、 `text-justify` はその種類を設定するものです。

### \[css-overflow\] Line-clamp and screen readers

https://github.com/w3c/csswg-drafts/issues/12859

[APA Weekly Teleconference – 01 Oct 2025](https://www.w3.org/2025/10/01-apa-minutes.html#8178)

[line-clamp プロパティ](https://developer.mozilla.org/ja/docs/Web/CSS/line-clamp "https://developer.mozilla.org/ja/docs/Web/CSS/line-clamp")で、clamp された部分がスクリーンリーダーで読み上げられるべきかどうかという議論です。  
挙動としては overflow プロパティと似ているため、それと合わせる（＝ clamp した部分も読み上げる）のが良いのではないかという意見や、clamp された部分は読み上げず、ツールチップで補うようにする案などが出ています。

## ARIA

### Expose implicit ARIA semantics (browser-defaults and ElementInternals)

https://github.com/w3c/aria/issues/2663

[Add `getComputedAria` to window · Issue #2656 · w3c/aria](https://github.com/w3c/aria/issues/2656)
[Add `getComputedRole` method to window · Issue #2655 · w3c/aria](https://github.com/w3c/aria/issues/2655)

`window.getComputedStyle`と同じように、計算された aria 属性や role を取得できるメソッドが提案されています。

これにより、ElementInterenals で定義されて HTML 上には現れていない aria 属性や role の値、`<section>`要素のように accessible name が付与されているかどうかで role の値が変わるような要素の role の値などを簡単に取得できるようになり、テストや Axe などのアクセシビリティチェックツールなどで役に立つことが予想されます。  
ただ、パフォーマンスの問題や[Web Platform Design Principles 2.11. Don’t reveal that assistive technologies are being used](https://www.w3.org/TR/design-principles/#do-not-expose-use-of-assistive-tech "https://www.w3.org/TR/design-principles/#do-not-expose-use-of-assistive-tech")に基づくプライバシーの問題により、実現方法が複数検討されていて、まだ議論中です。

[ARIA WG – 23 October 2025](https://www.w3.org/2025/10/23-aria-minutes.html#51e0)
[Read only `window.getComputedStyle()` equivalent for accessible state · Issue #212 · WICG/aom](https://github.com/WICG/aom/issues/212)
[.elementInternals accessor property · Issue #11040 · whatwg/html](https://github.com/whatwg/html/issues/11040)

### [aria-valuenow]: default to native value when one exists

https://github.com/w3c/aria/issues/2631

`spinbutton`のような role で`aria-valuenow`が指定されていないとき、0 に fallback するのではなくて`value`属性の値をそのまま`aria-valuenow`にすればいいのではないかという提案です。

議論の後、提案通りの仕様に変更するような draft PR が出されました。

[ARIA WG – 02 October 2025](https://www.w3.org/2025/10/02-aria-minutes.html#36f9)
[Update aria-valuenow to use native value as the default instead of 0 by smhigley · Pull Request #2643 · w3c/aria](https://github.com/w3c/aria/pull/2643)

### CSS: handing the a11y of anchor positioning

https://github.com/w3c/html-aam/issues/545

[ARIA WG – 16 October 2025](https://www.w3.org/2025/10/16-aria-minutes.html#b697)

Anchor Positioning が主要全ブラウザで利用可能になるのに向けて、アクセシビリティの観点での検討が行われています。

Popover API では、`popovertarget`属性を付与したトリガー要素に、`aria-details`の値としてターゲットとなるポップオーバー要素が割り当てられます。それと同様に Anchor Positioning で紐づけた要素も`aria-details`で紐づけることになりそうですが、`aria-describedby`で既に紐づけている場合には不要なのではないかという話や、Popover API の紐づけとの優先順位などが議論されています。

## JavaScript

9 月の TC39 meeting notes が公開されました。

https://github.com/tc39/notes/pull/384

また月例の ECMA402 meeting の note も公開されています。

[ecma402/meetings/notes-2025-10-13.md at main · tc39/ecma402](https://github.com/tc39/ecma402/blob/main/meetings/notes-2025-10-13.md)

### ECMA262

今月公開された、9 月の meeting notes の内容です。

#### [Iterator Chunking for Stage 2.7](https://github.com/tc39/proposal-iterator-chunking)

`Iterator.prototype.chunks` と `Iterator.prototype.windows` を追加する提案が Stage 2.7 になりました。前回の meeting では `sliding` と `windows` の 2 つのメソッドが提案されていましたが、挙動の違いが小さいため `windows` に統合され、第 2 引数の `undersized` で区別する形になりました。また、`undersized` の値は `Atomics.wait` の返り値や[Web プラットフォームの慣習](https://w3ctag.github.io/design-principles/#casing-rules "https://w3ctag.github.io/design-principles/#casing-rules")などに合わせて、kebab-case が採用されました。

#### [Import Bytes for Stage 2.7](https://github.com/tc39/proposal-import-bytes)

画像やフォントなどの任意のバイト列を import できる提案が Stage 2.7 になりました。

```js
import bytes from "./photo.png" with { type: "bytes" };
```

この提案は [Immutable ArrayBuffers](https://github.com/tc39/proposal-immutable-arraybuffer) に依存しているため、現状それ以上の Stage には進めません。

#### [Non-extensible Applies to Private for Stage 3](https://github.com/tc39/proposal-nonextensible-applies-to-private)

`Object.preventExtensions` で拡張不可能になったオブジェクトに対して [プライベートフィールド](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_properties) の追加を禁止する提案が Stage 3 になりました。

#### [Array.prototype.pushAll for Stage 1](https://github.com/DanielRosenwasser/proposal-array-push-all/)

既存の配列に大量の要素を安全に追加できる、 `Array.prototype.pushAll` の提案が Stage 1 になりました。現在は、大量の要素を `push(...array)` のように追加するとスタックオーバーフローを引き起こす可能性があります。

#### [Native Promise Predicate for stage 1 or 2](https://github.com/mhofman/proposal-native-promise-predicate)

ネイティブの Promise インスタンスかどうかを判定するメソッドの提案が Stage 2 になりました。メソッド名は `Promise.isPromise` が提案されていますが、誤用を避けるため Stage 2 で引き続き検討されます。

#### [Native Promise Adoption for Stage 1](https://github.com/mhofman/proposal-native-promise-adoption)

Promise の扱いを統一するための提案が Stage 1 になりました。

現在、`Promise` コンストラクタや async 関数の `return` では、解決値がネイティブ Promise であっても必ず `then` メソッドを呼び出すため、プロトタイプ汚染の影響を受けます。この提案では、これらの場面でもネイティブ Promise の状態を内部的に直接採用（adoption）するようにします。

#### [Normative: change PromiseResolve species check](https://github.com/tc39/ecma262/pull/3689)

`PromiseResolve` の仕様を変更する Normative PR が条件付きで原則承認されました。

現在、`Promise.prototype.then` をラップしても、`Promise.prototype.constructor` を汚染しない限り `await` 時にラップした `then` が呼び出されません。これは `PromiseResolve` が `constructor` をチェックしてネイティブ Promise かどうか判定しているためです。この問題を解決するため、`constructor`のチェックをやめて、内部プロトタイプのチェックに変更します。Web 互換性の問題がないかエンジン実装者によるデータ収集が行われることになりました。

### ECMA402

#### Intl Locale Info for Stage 4

Intl Locale Info Proposal は[Intl.Locale オブジェクト](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Intl/Locale)から得られるロケールの情報を拡張しようというプロポーザルです。具体的にはロケールに応じた週の情報や文字の記述の方向(LTR or RTL)、有効なカレンダー情報などを得られるようにする提案です。

この Intl Locale Info Proposal を Stage4 にすべく最終調整が行われました。残った調整としては以下の２点でしたが、どちらも変更が承認され、TG2(ECMA402 を担当する TaskGroups)内で Stage4 への承認が降りました。

- Editorial: Add note about inferring text direction from locale #103
- Normative: Add more explicit algorithms for lookups #92

残るは TG1(ECMAScaript 全体の仕様策定を行う)の meeting で承認されれば晴れて来年の仕様となりそうです。

#### [Normative: Delete superfluous era aliases to align with CLDR](https://github.com/tc39/proposal-intl-era-monthcode/pull/72)

Intl era and monthCode Proposal は iSO カレンダー以外の暦での特殊な月(閏月など)や独自の時代(日本でいう元号など)の指定方法を仕様に盛り込む提案です。

直近でリリースされた CLDR の 48 に合わせ、時代を指定するコード(era code)のエイリアスを削除する変更が行われました。具体的には`mundi`や`incar`などのエイリアスが削除されることになります。

#### [Normative: Add wording for buddhist calendar complexities for pre-1941 dates](https://github.com/tc39/proposal-intl-era-monthcode/pull/76)

タイ仏教暦が 1941 年以前までは 4 月 1 日を新年の年としていたことについて、1941 年より前を遡るときの挙動について議論がなされました。

結果として ICU は他のシステムとの整合性を鑑みて、1941 年以前の年月日であっても現在と同じように 1 月始まりとして遡れるように調整する方向で合意がなされました。

#### [Editorial: Add calendar authority for Japanese Imperial era names](https://github.com/tc39/proposal-intl-era-monthcode/pull/79)

仕様において日本の元号の読みと範囲の権威(参照すべき定義元)をどこに置くかという議論がなされました。

CLDR には後述するように明治時代の開始日が間違っているというバグがあることも踏まえ、時代名は CLDR の定義に準拠するものの、各時代の範囲に関しては日本政府の定義に依拠するよう仕様に明記することで同意がなされました。

#### [Editorial: Be explicit about the Gregorian to Meiji switchover month and day in the Japanese calendar](https://github.com/tc39/proposal-intl-era-monthcode/issues/86)

上記の変更に関連して、CLDR で誤った明治時代の開始日が広まっていたため、仕様に正確な開始日を明記するべきだという変更提案がなされました。

今回は変更最小限に止める意味も含め規範的な変更ではなく、読者の理解を助ける(誤解を解く？)ための純粋な仕様上での表記の追加として承認されました。

#### [Which Hijri calendars should Temporal support?](https://github.com/tc39/proposal-intl-era-monthcode/issues/29)

アラブ諸国などのイスラム圏で利用されるヒジュラ暦の伝統的な定義は、観測する場所や気候に左右されるため、計算で正確に決定することがそもそも難しいという事情があります。これに対し、ある程度計算可能な形にするアルゴリズムが何種類か提案されており、国や地域などによっても利用されるものが異なります。

今回の議題では Temporal などで単に`islamic`と指定された場合、複数ある変種のなかでどれを選択すべきなのかということが話し合われました。「地域に依存して最も適切なイスラム暦を選択する」といった提案もなされましたが、域依存性による可変性や将来的な Web 互換性破壊のリスクを鑑みて以下のような挙動とすることで合意がなされました。

- そもそも細かい種別を指定しない`islamic`だけでのカレンダーの指定を避けるべき
  - 場合によっては warning を出す
- その上で修飾のない`islamic`だけの指定や`islamic-rgsa`の場合`islamic-tbla`に fallback する

[Stage 2.7 review feedback: Be more specific about safe date ranges](https://github.com/tc39/proposal-intl-era-monthcode/issues/64)

非グレゴリオ暦カレンダー（特に中国暦など）において、Temporal が正確な計算を保証する日付の範囲を、カレンダーごとの制約に基づいて定義することについて話し合われました。

各カレンダーには独自の歴史的、実装上の制約があるため、普遍的原則の適用は困難で不適切であることが指摘され、実装の労力や信頼性などのカレンダーごとの制約に応じて定義されることで合意しました。

## Baseline

### 📃 October 2025 release notes

https://web-platform-dx.github.io/web-features-explorer/release-notes/october-2025/

## Misc

### Interop Feature Ranking

https://interop-rank.jakearchibald.com/

Interop の proposal の中でどれが重要だと考えているか、ランキング付けすることができるページを Firefox チームが公開しました。  
結果は Interop チームに共有されるとのことです。

### Add trusted contacts for Google account recovery

https://blog.google/technology/safety-security/recovery-contacts-verify-google-account/

Google Account にアクセスできなくなったときのために、信頼できる人をリカバリコンタクトとして登録しておける機能が公開されています。

### The World Wide Web Consortium (W3C) adopts a new logo to signal positive changes

https://www.w3.org/press-releases/2025/new-logo/

W3C（WorldWideWeb Consortium） が新しいロゴをリリースしました。

### W3C logo refresh: more than a cosmetic change, a small step towards durable and sustainable success

https://www.w3.org/blog/2025/w3c-logo-refresh-more-than-a-cosmetic-change-a-small-step-towards-durable-and-sustainable-success/

World Standards Day に際して、W3C CEO より、W3C 新ロゴに込められた想いが公表されました。

### Servo 0.0.1 Release

https://servo.org/blog/2025/10/20/servo-0.0.1-release/

Servo ブラウザの v0.0.1 がリリースされました。これから毎月リリースを重ねる予定とのことです。

### CMA confirms Apple and Google have strategic market status in mobile platforms

https://www.gov.uk/government/news/cma-confirms-apple-and-google-have-strategic-market-status-in-mobile-platforms  
Google と Apple のモバイルプラットフォームが、CMA から Strategic Market Status に指定されました。  
SMS に指定されると、CMA がその対象に対して、行動要件を課したり、特定のアクションに対して事前承認を要求したりできるようになります。
