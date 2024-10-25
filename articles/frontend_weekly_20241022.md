---
title: "Next.js 15 のリリースなど: Cybozu Frontend Weekly (2024-10-22号)"
emoji: "🍮"
type: "tech"
topics: ["frontend", "cybozufrontendweek"]
published: true
publication_name: "cybozu_frontend"
---

はじめまして！ サイボウズ株式会社 フロントエンドエンジニア（内定者バイト）の [mehm8128 (@mehm8128)](https://x.com/mehm8128) です。

## はじめに

サイボウズ社内では毎週火曜日に Frontend Weekly と題し「一週間の間にあったフロントエンドニュースを共有する会」を開催しています。

今回は、2024/10/22 の Frontend Weekly で取り上げた記事や話題を紹介します。

## 取り上げた記事・話題

### Announcing Deno 2

https://deno.com/blog/v2.0

Deno v2.0 がリリースされました。
v2.0 には次の内容が含まれます。

- Node.js および npm との後方互換性
  - `package.json`と`node_modules`をネイティブでサポート
  - Next.js、Astro、Remix、SvelteKit など多くのフレームワークをサポート
- `deno fmt`で HTML、CSS、YAML をフォーマットできるように
- ワークスペースとモノレポのサポート
- プライベートな npm レジストリのサポート
- ロゴの変更
  - 種類が多く統一したかったことと、小さいサイズで雨の背景が見づらくなるなどといった理由でリニューアルされたようです

| 旧ロゴ                                                  | 新ロゴ                                                  |
| ------------------------------------------------------- | ------------------------------------------------------- |
| ![](/images/frontend_weekly_20241024/Deno_logo_old.png) | ![](/images/frontend_weekly_20241024/Deno_logo_new.png) |

### Announcing TypeScript 5.7 Beta

https://devblogs.microsoft.com/typescript/announcing-typescript-5-7-beta/

TypeScript v5.7 Beta が利用可能になりました。
主な変更点は以下の通りです。

- [Checks for Never-Initialized Variables](https://devblogs.microsoft.com/typescript/announcing-typescript-5-7-beta/#checks-for-never-initialized-variables)
  - 今までは、初期化されていない変数が別の関数内で使用される場合に型エラーになっていませんでしたが、今回改善されました
  - 具体的には、条件分岐などで変数が初期化される可能性がある場合には今まで通り型エラーにならないのですが、変数の初期化が全く行われない状況では型エラーになるようになりました
- [Path Rewriting for Relative Paths](https://devblogs.microsoft.com/typescript/announcing-typescript-5-7-beta/#path-rewriting-for-relative-paths)
  - 今までは相対パスで ts ファイルを import するときには拡張子を`.js`にしなければならなかったのが、`--rewriteRelativeImportExtensions`オプションを使うことで`.ts`で書けるようになります
  - ただし、uhyo さんの[TS 5.7 の --rewriteRelativeImportExtensions オプションを使う前に読む記事](https://zenn.dev/uhyo/articles/rewrite-relative-import-extensions-read-before-use)で利用上の注意事項が解説されています
- [Support for `--target es2024` and `--lib es2024`](https://devblogs.microsoft.com/typescript/announcing-typescript-5-7-beta/#support-for---target-es2024-and---lib-es2024)
  - ECMAScript 2024 をサポートするようになりました
  - `Object.groupBy`や`Promise.withResolvers`など

### RFC 9651 on Structured Field Values for HTTP

https://lists.w3.org/Archives/Public/ietf-http-wg/2024JulSep/0316.html

HTTP ヘッダの値を構造化する RFC が更新されました。
現在 HTTP ヘッダの各値のフォーマットはフィールドによってバラバラですが、それを標準化する RFC です。
[RFC 8941 Structured Field Values for HTTP](https://www.rfc-editor.org/rfc/rfc8941.html) で Structured Field Values (SFV) について既に定義されていたのですが、新たに Date 型と UTF8 型が追加されたようです。

SFV についての詳細は少し前の記事ですが、jxck さんの記事が参考になります。

https://blog.jxck.io/entries/2021-01-31/structured-field-values.html

### React Compiler Beta Release

https://react.dev/learn/react-compiler#using-react-compiler-with-react-17-or-18

React Compiler が React v17 と v18 でも利用できるようになりました。
利用するには追加で`react-compiler-runtime@beta`をインストールし、compiler の設定を変える必要があります。

### Google's New CrUX Vis Tool: Explore Core Web Vitals Data

https://www.debugbear.com/blog/google-crux-vis

Google から新しく、 CrUX Vis という Core Web Vitals ツールがリリースされました。
Google がユーザーから収集している CrUX (Chrome UX report) のデータから、指定したサイトの CWV などのメトリクスを時系列で閲覧することができます。

関連して、Chrome 129 から DevTools の Performance パネルで同様に CrUX が閲覧できるようになっています。ローカル環境でのパフォーマンスを実際のユーザー体験と比較でき、より詳細にチューニングするのに役立てることができます。

https://developer.chrome.com/blog/devtools-realtime-cwv#field-data

### Zustand v5 release

https://github.com/pmndrs/zustand/releases/tag/v5.0.0

Zustand v5 がリリースされました。
新機能の追加は特にないようですが、古い機能が削除されてバンドルサイズが削減されました。具体的には、minified のサイズが v4.5.5 では 3.1kB だったのが、v5.0.0 では 1.2kB まで減りました。

また、一部の動作が変更されて無限ループが発生するようになる可能性があるので、アップデート時にはマイグレーションガイドを参照してください。

https://zustand.docs.pmnd.rs/migrations/migrating-to-v5#changes-in-v5

### Next.js 15 release

https://nextjs.org/blog/next-15

Next.js 15 がリリースされました。一部アップデート内容を抜粋します。

- Async Request APIs
  - `cookies`や`headers`などの API が非同期関数に変更されました
- Caching Semantics
  - `fetch`リクエストや`GET`ルートハンドラーなどがデフォルトでキャッシュされなくなりました
- React 19 のサポート
- `instrumentation.js` (Stable)
  - OpenTelemetry などを用いたエラー監視の初期化に使うことができる`instrumentation.js`が stable になりました
- `<Form>` Component
  - HTML の`<form>`タグを拡張した`<Form>`コンポーネントが導入されました
  - フォームの送信時に client navigation ができたりと、パフォーマンスなどの向上が見込めます
- Support for `next.config.ts`
  - `NextConfig`という型を用いて nextConfig に型をつけることができるようになりました
- Improvements for self-hosting
  - `Cache-Control`ヘッダの細かい制御が可能になりました

## あとがき

個人的に Next.js 15 がリリースされたのが嬉しかったです。
また、Structured Field Values for HTTP については噂を聞いたことあるくらいだったので、詳しく知ることができてよかったです。
