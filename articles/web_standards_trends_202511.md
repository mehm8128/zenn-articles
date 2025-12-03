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

## CSS

### \[css-ui\] interactivity: focusable

https://github.com/w3c/csswg-drafts/issues/13040

CSS カルーセルなどで使われることを想定して最近追加された`interactivity: inert`の反対の挙動をする値として、`interactivity: focusable`が提案されています。
しかし、ユースケースが明確でないことや、CSS だけの問題に留まらず HTML やアクセシビリティなどが深く関係していることから、APA WG で今後議論される流れになっています。

### Blink: Intent to Ship: Enable monochrome emoji rendering in forced colors mode.

https://groups.google.com/a/chromium.org/g/blink-dev/c/A57SyMHayKM/m/-eWIfZqBCAAJ

強制カラーモード時に、絵文字がモノクロでレンダリングされるようになります。

## ARIA・WCAG

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
