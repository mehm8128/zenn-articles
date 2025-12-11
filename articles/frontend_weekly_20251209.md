---
title: "OxlintやOxfmtのAlpha版など: Cybozu Frontend Weekly (2025-12-09号)"
emoji: "🎄"
type: "idea"
topics: ["frontend", "cybozufrontendweek"]
published: true
publication_name: "cybozu_frontend"
---

こんにちは！ サイボウズ株式会社 フロントエンドエンジニアの [mehm8128 (@mehm8128)](https://x.com/mehm8128) です。

## はじめに

サイボウズ社内では毎週火曜日に Frontend Weekly と題し「一週間の間にあったフロントエンドニュースを共有する会」を開催しています。

今回は、2025/12/09 の Frontend Weekly で取り上げた記事や話題を紹介します。

## 取り上げた記事・話題

### never が好き

https://susisu.hatenablog.com/entry/2025/12/04/130842

TypeScript の`never`型の紹介記事です。

条件分岐の網羅性チェックや「絶対に特定のパターンにならない型」「型レベルプログラミングにおけるエラー」など、実用的な使い方が解説されています。

### 2025 年公正取引委員会チョイススクリーン特設サイト | 公正取引委員会

https://www.jftc.go.jp/choice_screen/index.html

12/18 より施行される、スマホソフトウェア競争促進法に則り、スマホのデフォルトブラウザ・検索エンジンを選択することができる「チョイススクリーン」の紹介サイトです。

以下の記事も併せてご覧ください。
https://www.mitsue.co.jp/knowledge/column/20251209.html

### Announcing Oxfmt Alpha

https://voidzero.dev/posts/announcing-oxfmt-alpha

VoidZero が開発中の Rust 製フォーマッターである Oxfmt の Alpha リリースがアナウンスされました。

Prettier と 95%の互換性をもちつつ、30 倍以上高速化されているとのことです。

併せて Oxc 側でのアナウンスもご覧ください。
https://oxc.rs/blog/2025-12-01-oxfmt-alpha.html

また、Oxlint の Type-Aware Linting も Alpha 版がアナウンスされました。以下の記事をご覧ください。

https://oxc.rs/blog/2025-12-08-type-aware-alpha.html

### tsgolint はいかにして typescript-go の非公開 API を呼び出しているのか

https://speakerdeck.com/syumai/how-tsgolint-exposes-typescript-gos-private-apis

layerx.go で行われた、tsgolint の内部実装についての発表のスライドです。

git submodule や`go:linkname`など複数の方法を用いて typescript-go の非公開 API を呼び出していることが解説されています。

### ソースマップはどのように元コードへの参照を保持するか

https://www.docswell.com/s/4136989/574PP1-fec_kansai_long

フロントエンドカンファレンス関西で行われた、ソースマップの仕組みについての発表のスライドです。

「ソースマップとは」の解説から始まり、Base64 をデコードした JSON に埋め込まれている`mappings`やそこで使われている Base64 VLQ という形式について解説されています。

### Vite 8 Beta: The Rolldown-powered Vite

https://vite.dev/blog/announcing-vite8-beta

Vite 8 の Beta 版がリリースされました。

バンドラーに esbuild と Rollup を用いていたのに代わり、Rust 製バンドラーである Rolldown が採用されました。実際にいくつかの製品で試しに使ってみた、高速化事例も紹介されています。

### Critical Security Vulnerability in React Server Components (CVE-2025-55182)

https://react.dev/blog/2025/12/03/critical-security-vulnerability-in-react-server-components

https://nextjs.org/blog/CVE-2025-66478

React Server Components においてリモートコード実行が可能になる脆弱性が報告されました。
[CVE-2025-55182](https://www.cve.org/CVERecord?id=CVE-2025-55182)として公開され、CVSS 10.0（深刻度 max）と評価されているようです。

該当するバージョンで React Server Components を利用している方はアップデートしましょう。

### CSS wrapped 2025

https://chrome.dev/css-wrapped-2025/

Chrome から 2025 年の CSS まとめが公開されました。

Invoker Commands や Customizable Select、scroll-state、`if()`など 2025 年に Chrome に登場した様々な CSS が紹介されています。

### Announcing Baseline in action

https://web.dev/blog/announcing-baseline-in-action

最近 Baseline になった機能の活用事例がデモとともに紹介される記事の連載です。

既にダイアログとポップオーバー、画像、コンテナクエリーの 3 つのトピックの記事が出ていますが、今後も公開されるようです。

### How We're Protecting Our Newsroom from npm Supply Chain Attacks

https://pnpm.io/blog/2025/12/05/newsroom-npm-supply-chain-security

Seattle Times が pnpm を用いてどのようにして npm サプライチェーン攻撃を防いだかという解説記事です。
主に以下の 3 つの手法を組み合わせて対応したとのことです。

1. pnpm がデフォルトでライフサイクルスクリプトを実行しないこと。`strictDepBuilds: true`オプション
2. `minimumReleaseAge`を用いたクールダウン期間
3. Trust Policy

## あとがき

VoidZero のツールチェイン周りのアップデートが目立っていました。stable 版のリリースや、Vite+の続報が楽しみです。
