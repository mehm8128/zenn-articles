---
title: "WCAG2.2のISO/IEC 40500:2025 規格化など: Cybozu Frontend Weekly (2025-09-30号)"
emoji: "🍣"
type: "idea"
topics: ["frontend", "cybozufrontendweek"]
published: false
publication_name: "cybozu_frontend"
---

こんにちは！ サイボウズ株式会社 フロントエンドエンジニアの [mehm8128 (@mehm8128)](https://x.com/mehm8128) です。

## はじめに

サイボウズ社内では毎週火曜日に Frontend Weekly と題し「一週間の間にあったフロントエンドニュースを共有する会」を開催しています。

今回は、2025/09/30 の Frontend Weekly で取り上げた記事や話題を紹介します。

## 取り上げた記事・話題

### State of JavaScript 2025

https://survey.devographics.com/en-US/survey/state-of-js/2025

State of JavaScript のサーベイが開始しています。
回答期間は 10 月末までとのことです。

### ISO/IEC 40500:2025 - Information technology — W3C Web Content Accessibility Guidelines (WCAG) 2.2

https://www.iso.org/standard/91029.html

WCAG2.0 がそのまま国際規格となっていた『ISO/IEC 40500:2012』が、WCAG2.2 を採用する形で『ISO/IEC 40500:2025』に改定されました。

これに合わせて、日本の規格である『JIS X 8341-3』の改定も動き出すことが考えられます。

### Temporal API の現在地（2025 年 9 月時点）

https://zenn.dev/fabon/articles/4dbe40896fac73

JavaScript における日時操作の新しい仕様である Temporal API の現状が解説されています。
Chromium で [Intent to Ship](https://groups.google.com/a/chromium.org/g/blink-dev/c/s58GKzoQZFg) がアナウンスされており、Chrome v144 で ship 予定とのことです。

### AI エージェント用の Chrome DevTools（MCP）

https://developer.chrome.com/blog/chrome-devtools-mcp?hl=ja

Chrome DevTools で公式から MCP サーバーが公開されました。
これにより、ネットワークやコンソールのエラー、DOM や CSS などを AI エージェントから直接見れるようになったり、ユーザーの行動のシミュレートやパフォーマンスの問題調査ができるようになります。

### Zenn 記事をより直感的に書ける！〜 WYSIWYG エディタのすゝめ 〜

https://zenn.dev/karintou/articles/zenn-cli-wysiwyg

Zenn CLI で WYSIWYG エディタを使って Zenn の記事を執筆できるようになるエディタを開発した紹介記事です。
Zenn CLI で起動したサーバーで、UI 上で WYSIWYG を使って編集したものが、ローカルファイルに反映されるようなものになっていて、画像のドラッグアンドドロップなどもできるようです。

### Our plan for a more secure npm supply chain

https://github.blog/security/supply-chain-security/our-plan-for-a-more-secure-npm-supply-chain/

サプライチェーン攻撃を防ぐために、GitHub がセキュリティ強化のロードマップを公開しました。
主に以下の 3 つが紹介されています。

- パッケージを publish する際の、2FA の必須化
- expiration が 1 週間だけの、きめ細かいアクセストークンの利用
- Trusted Publishing

その他 classic token の廃止なども挙げられています。

### Intent to Ship: Interest Invokers (the interestfor attribute)

https://groups.google.com/a/chromium.org/g/blink-dev/c/bX1G_yDt6W4/m/EHtiOi_IAAAJ

Interest Invokers が Chromium で Intent to Ship になりました。

Interest Invokers はユーザーが「興味」を示したときにターゲット要素でアクションがトリガーされるような機能です。
詳しくは以下の記事で紹介しています。

https://zenn.dev/cybozu_frontend/articles/web_standards_monthly_202509#intent-to-ship%3A-interest-invokers-(the-interestfor-attribute)

### feat: implement npmMinimumReleaseAge and npmMinimumReleaseAgeExclude config options

https://github.com/yarnpkg/berry/pull/6901

pnpm v10.16.0 からサプライチェーン攻撃を緩和するための minimumReleaseAge オプションが導入されたことから、yarn にも同様のオプションが導入されました。
v4.10.0 でリリースされたとのことです。

### Quiet UI - My Creative Outlet

https://www.abeautifulsite.net/posts/quietui-my-creative-outlet/

[Web Awesome](https://github.com/shoelace-style/webawesome) の作者が開発した、Web Components ベースの UI コンポーネントライブラリです。
Web Awesome と違い、ElementInternals を用いたフォーム検証や Popover API など、まだ Newly Available な最新の機能を色々使っているものです。
Comparison、Joystick、Slide Activator など、他ではあまり見ないようなコンポーネントがあるようです。
また、現在 Beta 版の Web Awesome は 10 月中に正式リリース予定とのことです。

### Codex vs Claude Code: which is the better AI coding agent?

https://www.builder.io/blog/codex-vs-claude-code

Codex と Claude Code の比較が行われています。
エージェントやモデルでは大きな違いはない一方で、最も大きな違いとして GitHub 上での自動レビューの点で、筆者は Codex を好むと主張されています。

### Help Us Raise $200k to Free JavaScript from Oracle

https://deno.com/blog/javascript-tm-gofundme

Deno が Oracle に JS の商標の取消申請を行っている件で、証拠開示段階に突入しているようです。
その上で、訴訟に費用がかかるため、20 万ドルの寄付を募るとのことです。

### Storybook is going ESM-only

https://storybook.js.org/blog/storybook-is-going-esm-only/

Storybook が ESM only に向かっている理由や現状が語られています。
Storybook v10 では、v9 と比べてバンドルサイズが 15% 削減され、完全に ESM のみになるとのことです。

### あとがき

サプライチェーン攻撃への対策が話題になっている一方で、Temporal や Interest Invokers の Intent to Ship など、標準側も着実に進んでいます。
片方に寄らずに、どちらもバランスよくキャッチアップしていきたいです。
