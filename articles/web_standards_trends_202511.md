---
title: "Web 標準動向 2025年11月版"
emoji: "🍂"
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
今月の執筆者は以下の 2 名です。

- [saku](https://x.com/sakupi01)
  - HTML, CSS に関連するトピックを執筆
- [mehm8128](https://x.com/mehm8128)
  - 主にアクセシビリティに関連するトピックを執筆

:::

## TPAC 2025 が神戸で開催

https://www.w3.org/2025/11/TPAC/Overview.html

W3C の全グループが一同に会す国際会議である TPAC 2025 が 11/10 ～ 11/14 に神戸で開催されました。
サイボウズからはオンサイトで 8 名、オンラインでも 2 名ほど参加しました。

以下の 2 つの記事に、概要や感想がまとまっています。

https://blog.cybozu.io/entry/2025/11/27/170000

https://blog.cybozu.io/entry/2025/11/28/113000

また、弊社の参加者や今回引率してくださった Jxck さんが、個人のブログでも学びや感想を発信しています。

https://blog.sakupi01.com/dev/articles/tpac-2025-diary

https://portfolio.hm8128.me/blog/tpac2025/

https://blog.jxck.io/entries/2025-11-18/tpac2025.html

## HTML

### Native Footnote/Endnote Element in HTML

https://github.com/whatwg/html/issues/11921

現状の HTML には脚注を表すためのセマンティックな方法がありません。`<a>`要素を用いて脚注を作成する方法もありますが、アクセシビリティ上の問題があったり、作成者が行わなければならない紐づけや値の指定が多いということが指摘されています。よって、この issue で`<noteref>`及び`<note>`要素を新しく追加することが提案されています。

### Blink: Intent to Ship: Focus's focusVisible option

https://groups.google.com/a/chromium.org/g/blink-dev/c/OptbQ7cGx4A/m/4aWLXMpWAwAJ

Blink で`focus()`メソッドの`focusVisible`オプションのサポートが Intent to Ship になりました。
これは`true`を渡すとフォーカス時にフォーカスリングが表示されて`:focus-visible`疑似クラスとマッチし、`false`を渡すとその逆になります。
既に Gecko と Safari でサポートされているので、これで全主要ブラウザでサポートされることになります。

### Proposal: HTML in Canvas

https://github.com/WICG/html-in-canvas

`<canvas>` の中で Element を描画可能にする提案です。`<input>` などのインタラクティブ要素を挿入可能になったり、アクセシビリティツリーの構築が可能になります。アクセシブルでインタラクティブな `<canvas>` を作成可能として、期待の声が高まっていた機能の一つです。

Chromium での実装が進んでおり、Stable バージョンでも `chrome://flags/#canvas-draw-element` を有効化することで利用可能です。

https://raw.githack.com/mrdoob/three.js/htmltexture/examples/webgl_materials_texture_html.html

![Canvas 要素と WebGL を用いて作られた立方体に、HMTL でテキストが描画されている](/images/web_standards-trends/202511-canvas.png)

## Prototype: Declarative scroll commands for HTMLButtonElement

https://groups.google.com/a/chromium.org/g/blink-dev/c/3jwcQg6u90k

Invoker Commands にスクロール用コマンド（`page-up`、`page-down`、`page-left`、`page-right`）を追加する提案のプロトタイプです。  
元々は、CSS Carousel のボタン要素を、（擬似要素ではなく）要素で実装可能にするために提案されたものです。デザインシステムなど既成のボタンをスクロールのトリガーに拡張可能になります。

## CSS

### \[css-ui\] interactivity: focusable

https://github.com/w3c/csswg-drafts/issues/13040

CSS カルーセルなどで使われることを想定して最近追加された`interactivity: inert`の反対の挙動をする値として、`interactivity: focusable`が提案されています。
しかし、ユースケースが明確でないことや、CSS だけの問題に留まらず HTML やアクセシビリティなどが深く関係していることから、APA WG で今後議論される流れになっています。

### Blink: Intent to Ship: Enable monochrome emoji rendering in forced colors mode.

https://groups.google.com/a/chromium.org/g/blink-dev/c/A57SyMHayKM/m/-eWIfZqBCAAJ

強制カラーモード時に、絵文字がモノクロでレンダリングされるようになります。

### Prototype and Ship: View transition types

https://groups.google.com/a/mozilla.org/g/dev-platform/c/XZ4P63YCbZA/

これまで、特定の ViewTransition 要素に対して異なる Transition を指定することができませんでした。

`startViewTransition()` に `type` が指定可能になり、ViewTransition に型づけすることができるため、型に応じた Transition の指定が可能になります。利用する際は、型づけされた ViewTransition に対して、CSS 擬似要素 `:active-view-transition-type(<custom-ident>#)`で Transition を指定します。

また、Cross-document ViewTransition においても、 `@view-transition` に `type` 指定子の追加が検討されています。これにより、MPA における Back/Forward Navigation においてより柔軟な Transition を指定できるでしょう。

さらに、JavaScript を用いた `type` での分類のみならず、 CSS で宣言的な URL パターンマッチングを可能にする `@navigation` の提案が進行しています。こちらも非常に興味深い提案ですので、ぜひご覧ください。

[https://drafts.csswg.org/css-navigation-1/](https://drafts.csswg.org/css-navigation-1/)
[https://github.com/w3c/csswg-drafts/issues/12594](https://github.com/w3c/csswg-drafts/issues/12594)

### Firefox 145 release notes for developers (Stable) & Firefox 146 release notes for developers (Beta)

https://developer.mozilla.org/en-US/docs/Mozilla/Firefox/Releases/145
https://developer.mozilla.org/en-US/docs/Mozilla/Firefox/Releases/146

Firefox 145 (Stable) と 146 (Beta) がのリリースされました。ディベロッパーから多くの期待が集まっていた CSS の機能がいくつかリリースされたため、取り上げます。

- `text-autospace`：初期値に関しては他と同じく `no-autospace`（デフォルトで間隔をあけない） です。どのブラウザも仕様に準拠しない（仕様のデフォルトは `normal` で、間隔をあける）という状態で、クロスブラウザサポートが完了しています。
- `<hr>` in `<select>`：HTMLSelectElement のパーサ緩和の手始めとして、 `<hr>` が `<select>` に挿入可能になりました。Customizable Select Element に向けた実装が進行しています。
- `CSS Anchor Positioning`：Interop 2025 Focus Areas の一つであり、ディベロッパーからの期待も多く寄せられていた待望の機能が、 Firefox の実装で揃いました。ただし、クロスブラウザでのサポートが保証されている機能は `anchor-name`、`position-anchor`、 `anchor()`関数となっています。それ以外の機能は、一部のブラウザでのみサポートされています。
- CSS `@scope`：こちらも Interop 2025 Focus Area の機能で、Firefox の実装によりクロスブラウザサポートが完了しました。Cascade に近接性（Scope Proximity）をもたらし、DOM 構造上の”近さ”を加味したスタイルの優先度づけが可能になります。
- `contrast-color()`：任意の色を引数に取り、WCAG 2.2 AA のコントラスト基準を満たす white/black いずれかのカラーを返す関数です。
- `display-p3-linear` 色空間： `display-p3` 色空間よりも、表示される色の精度が高くなります。

そのほか、 `Atomics.waitAsync()` や Navigation API といった待望の機能もリリースに含まれています。このリリースによってクロスブラウザでのサポートが完了した機能も多く、今年の Firefox の素晴らしい功績が表れたリリースだったとも言えるでしょう。

### **\[css-grid-3\] Masonry Switch Syntax**

https://github.com/w3c/csswg-drafts/issues/12022

CSS Masonry レイアウトに切り替える display プロパティのキーワードが決定されました。

fantasai が実施したサーベイの結果、`grid-stack`、`packed-grid` 、`grid-lanes` 、`lane-grid` が有力となりました。しかし以下のような理由から、 最終的には `grid-lanes` と `masonry-grid` で投票が行われました。

- Stack 系は stacking context と勘違いされる
- Pack 系は density プロパティと勘違いされる

この投票は TPAC 2025 で行われ、投票の結果、 `grid-lanes`という新しいレイアウト手法が誕生しました。

長年決着がつかないままでいた CSS の新しいレイアウトが一区切りした瞬間として、非常にめでたいものでした。

## Web API

### Intent to Ship: deprecate BackgroundFetch

https://groups.google.com/a/chromium.org/g/blink-dev/c/CpXXaJh5Rq8

Background fetch API の利用率が非常に低いことを理由に、 API を削除する提案が出されました。
メンテナンスコストが理由でしたが、少なくともユーザがいることや、明確なセキュリティの問題があるわけでもないことから議論になり、最終的には撤回されました。

### Deprecate Privacy Sandbox APIs

https://privacysandbox.google.com/blog/update-on-plans-for-privacy-sandbox-technologies

Privacy Sandbox の終了を受け、複数の API の削除が始まりました。
対象は、 Topics API や Private Aggregation API などの、広告関連の API です。

## ARIA

### Change tooltip to name prohibited

https://github.com/w3c/aria/pull/2670

tooltip ロールの name from が`author and contents`から prohibited に変わります。

tooltip ロールは多くの場合、`aria-describedby`によって呼び出し元要素から中身を参照されるような使い方がされます（[参考](https://www.w3.org/TR/wai-aria-1.2/#tooltip)）。しかし`aria-label`などで tooltip ロールの要素に accessible name をつけてしまうと、`aria-describedby`したときにその`aria-label`の内容しか読まれず、ツールチップの詳細な中身が読まれないというバグが発生してしまう可能性があることから、author による名前付けが禁止されました。
また、name from は`author`, `author and contents`, `prohibited`のどれかで`contents`のみの場合には`prohibited`になることから、`prohibited`になったようです。

name from については Accessible Rich Internet Applications (WAI-ARIA) 1.2 の[5.2.8 Accessible Name Calculation](https://www.w3.org/TR/wai-aria-1.2/#namecalculation)をご覧ください。

### WIP: Add CSS-AAM editor’s draft

https://github.com/w3c/aria/pull/2673

TPAC での議論を基に、[止まっていた CSS-AAM](https://github.com/w3c/css-aam/)を復活させる PR が作成されました。

OpenUI や CSS 周りの新しい機能において、アクセシビリティレビューが手薄になってしまったりタイミングが遅くなってしまったりしている状態を脱却するために、2021 年まで CSS Accessibility Task Force で管理されていた CSS-AAM を実験的に復活させ、プロセスの見直しが行われるようです。
CSS-AAM は、CSS 機能の Accessibility API への Mapping を定義するもので、例えば HTML には既に[HTML-AAM](https://www.w3.org/TR/html-aam-1.0/)が運用されています。

参考

- [Need a process for new CSS features and accessibility mappings / review · Issue #2496 · w3c/aria](https://github.com/w3c/aria/issues/2496)
- [TPAC 2025 - ARIA WG Day 2 – 11 November 2025](https://www.w3.org/2025/11/11-aria-minutes.html#585f)

## JavaScript

### ECMA262

#### Ship: Temporal in ECMA262

https://groups.google.com/a/chromium.org/g/blink-dev/c/s58GKzoQZFg

Chromium ブラウザに実装されていた Temporal 実装がフラグなしでロールアウトされます。
Chrome では バージョン 144 から利用可能とのことです。WebKit でもすでに一部の機能が Technology Preview で実装されており、クロスブラウザサポートが期待されます。

## Baseline

### 📃 November 2025 release notes

https://web-platform-dx.github.io/web-features-explorer/release-notes/november-2025/

## Misc

### How Cybozu eliminated browser compatibility overhead with Baseline

https://web.dev/case-studies/cybozu-baseline?hl=ja

弊社の saku さんが、サイボウズのプロダクトにおける Baseline 活用についての記事を web.dev にて執筆しました。

### 111th meeting of Ecma TC39 が東京で開催

https://github.com/tc39/agendas/blob/main/2025/11.md

TC39 の 111 回目のミーティングが 11/18 ～ 20 に東京で開催されました。
次に日本で行われるのは来年の 9 月のようです（[参考](https://github.com/tc39/agendas#future-meeting-locations-and-hosts)）。

### W3C Advisory Committee Elects Technical Architecture Group

https://www.w3.org/news/2025/w3c-advisory-committee-elects-technical-architecture-group/

W3C の Technical Architecture Group (TAG) の、2026 年 2 月から任期が開始するメンバーが選出されました。
空議席数 4 席に対してノミネートされたのが 4 名だったので、選挙無しに 4 名全員当選となりました。

### Microsoft Edge introduces passkey saving and syncing with Microsoft Password Manager - Microsoft Edge Blog

https://blogs.windows.com/msedgedev/2025/11/03/microsoft-edge-introduces-passkey-saving-and-syncing-with-microsoft-password-manager/

デスクトップ版 Edge on Windows でパスキーの同期が可能になりました。

### ADC Projects Update

https://docs.google.com/document/d/1ree75ImLZjf60lTZ3BhCaLHygxgywr7SBXp-q0xPs8A/edit?tab=t.kwy3jemh57eo#heading=h.otakpzvvzent

ブラウザではなく、支援技術の相互互換性を測る指標づくりの一環として、Accessibility Compat Data (ACD) の作成が進んでいます。Mozilla が資金調達に合意したとのことです。
