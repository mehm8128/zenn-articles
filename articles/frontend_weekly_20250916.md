---
title: "WebKit Features in Safari 26.0など: Cybozu Frontend Weekly (2025-09-15号)"
emoji: "🎨"
type: "idea"
topics: ["frontend", "cybozufrontendweek"]
published: true
publication_name: "cybozu_frontend"
---

こんにちは！ サイボウズ株式会社 フロントエンドエンジニアの [mehm8128 (@mehm8128)](https://x.com/mehm8128) です。

## はじめに

サイボウズ社内では毎週火曜日に Frontend Weekly と題し「一週間の間にあったフロントエンドニュースを共有する会」を開催しています。

今回は、2025/09/15 の Frontend Weekly で取り上げた記事や話題を紹介します。

## 取り上げた記事・話題

### A deep dive into Cloudflare’s September 12, 2025 dashboard and API outage

https://blog.cloudflare.com/deep-dive-into-cloudflares-sept-12-dashboard-and-api-outage/

Cloudflare のダッシュボードのフロントエンドで、useEffect の第二引数に問題のあるオブジェクトを誤って渡してしまって障害が発生してしまったバグレポートです。

タイムラインや対応方法について解説されています。

### Google Chrome at 17 - A history of our browser

https://addyosmani.com/blog/chrome-17th/

Chrome の 17 周年を記念して、Google の addy が Chrome の歴史について書いた記事です。

Chrome の起源から始まり、パフォーマンスやセキュリティ、UI、Baseline や Core Web Vitals などの話題に移り、最近の AI の状況についても触れられています。

### WebKit Features in Safari 26.0

https://webkit.org/blog/17333/webkit-features-in-safari-26-0/

Safari 26.0 の新機能についての記事です。

CSS の話題が多く、Anchor Positioning や Scroll-driven animations、Contrast Color、Progress 関数などが紹介されています。
その他、HDR や Digital Credentials API についての情報もありました。

### A refresh of Learn CSS with nine new modules

https://web.dev/blog/learn-css-refresh

web.dev の Learn CSS コースに、コンテンツが追加されました。
Baseline に到達したばかり・到達が見込まれる機能を中心に拡充されています。

### pnpm 10.16 - New setting for delayed dependency updates

https://pnpm.io/blog/releases/10.16

pnpm 10.16 から、`minimumReleaseAge` オプションが導入されました。
公開から指定した期間が経過したパッケージのみをインストールすることができるようになるもので、サプライチェーン攻撃を軽減することができます。

このバージョンでは他に、依存関係を検索するための関数を指定できる finder 機能も追加されています。

### Prisma 6.16.0

https://github.com/prisma/prisma/releases/tag/6.16.0

Prisma 6.16.0 がリリースされました。

DB アダプタ部分の、 Rust から TS への移行が完了したとのことです。
その結果、バンドルサイズを 90% 削減できたりパフォーマンスの向上が確認できたりしたとのことです。

### 2025: 0 of the Global Top 200 Websites Use Valid HTML

https://meiert.com/blog/html-conformance-2025/

Wikipedia や Youtube など、世界で訪問数の多いサイトを Nu HTML Checker で調査した結果のまとめ記事です。
スプレッドシートにまとめられていて、エラーが無かったサイトが 0 件だったとのことです。

### クリックしやすいターゲットエリアを実装する

https://yuheiy.com/2025-09-12-click-friendly-target-areas

padding や疑似要素などを使って、視覚的なサイズは変えずにターゲットエリア（クリックやタップに反応する領域）だけ大きくするテクニックが紹介されています。
既に取り入れられている例として、iOS のキーボードで、入力した文字列から次の文字を予測して、その文字のキーだけターゲットエリアを少し大きくしてるというものが挙げられています。

### あとがき

最近、新しい CSS の話題が多いように感じます。先週の記事でも interop 2026 の紹介がありましたが、全ブラウザで対応されるのが楽しみです。
