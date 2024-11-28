---
title: "Tailwind CSS v4.0 Beta のリリースなど: Cybozu Frontend Weekly (2024-11-26号)"
emoji: "❄️"
type: "tech"
topics: ["frontend", "cybozufrontendweek"]
published: true
publication_name: "cybozu_frontend"
---

こんにちは！ サイボウズ株式会社 フロントエンドエンジニア（内定者バイト）の [mehm8128 (@mehm8128)](https://x.com/mehm8128) です。

## はじめに

サイボウズ社内では毎週火曜日に Frontend Weekly と題し「一週間の間にあったフロントエンドニュースを共有する会」を開催しています。

今回は、2024/11/26 の Frontend Weekly で取り上げた記事や話題を紹介します。

## 取り上げた記事・話題

### Using React Server Components in Expo Router apps

https://docs.expo.dev/guides/server-components/

Expo Router で React Server Components が使えるようになりました。
Experimental なので、まだ本番環境での使用には対応していないようです。

### Ubie が 2024 年に React Native を選ぶ理由

https://zenn.dev/ubie_dev/articles/46cf443d5dd25b

Ubie にとって React Native がフィットした理由が書かれた記事です。
TypeScript に統一することの良さや開発体験、パフォーマンスの改善、エコシステム、Flutter など他の技術との比較、などの観点でまとめられています。
また、上で挙げた React Server Components についても述べられています。

### パフォーマンスの AI アシスタンス | Chrome DevTools | Chrome for Developers

https://developer.chrome.com/docs/devtools/ai-assistance/performance?hl=ja

Chrome Devtools 上の Performance パネル で、 AI と対話してパフォーマンスを改善できるようになりました。
なおこの機能は、Chrome Canary 132 以降でのみ利用できます。

### ts-blank-space

https://bloomberg.github.io/ts-blank-space/

ts-blank-space は TypeScript と JavaScript に変換するトランスパイラです。
型チェックや複雑なコード変換をするわけではなく、ただ型情報をそれと同じ文字数のスペースに置き換えるだけのものです。

JavaScript に変換したときにコードの位置がほとんどずれないのでソースマップを生成する必要がなくなり、エラースタックの座標からそのままエラー箇所を特定することができます。

また、ビルドパフォーマンスの向上にも繋がることが示されています。

### The State of ES5 on the Web

https://philipwalton.com/articles/the-state-of-es5-on-the-web/

現在の ES5 を target としたトランスパイルについて調査・解説した記事です。
Babel はまだデフォルトで ES5 を target にしていますが、ES5 を target にしたトランスパイルはサイズが大きくなりがちで、CrUX を用いた調査結果を踏まえても ES5 を target にする必要性は現在ではほとんどないと説明されています。
そこで、BrowsersList や Baseline を利用して target を設定することが推奨されています。

### CSS Nesting Module/CSS の入れ子指定 ［CSS Modern Features no.4］ | gihyo.jp

https://gihyo.jp/article/2024/11/ride-modern-frontend-04

弊社のフロントエンドエキスパート [mugi](https://zenn.dev/mugi)さんの、CSS Nesting Module についての記事です。
コンテナクエリなど At-rules との組み合わせについても紹介されています。

### State of WordPress Security In 2024 – Patchstack

https://patchstack.com/whitepaper/state-of-wordpress-security-in-2024/

少し昔に出されたものですが、WordPress のセキュリティのまとめ記事です。

2023 年にあった WordPress 周りのセキュリティ事項のまとめと、2024 年にどのようなことが期待されているかが書かれています。

（WordPress プラグインやテーマの作者などを含む）OSS 開発者にソフトウェアの安全性を確保するための対策を実施することを義務付けるような法案が出されていたので、それによって 2024 年以降より安全になることが期待されていました。

そしてその法案は今年 10 月に正式に採択され、11 月 20 日に正式発行されました。2027 年 12 月 11 日以降、上記の内容及びその他様々なセキュリティ強化が義務付けられるようになります。

https://prtimes.jp/main/html/rd/p/000000196.000017062.html

### Tailwind CSS v4.0 Beta - Tailwind CSS

https://tailwindcss.com/docs/v4-beta

Tailwind CSS v4.0 Beta がリリースされました。

- [ビルドパフォーマンスの改善](https://tailwindcss.com/docs/v4-beta#new-high-performance-engine)
  - フレームワークを書き直し、フルビルドで 3.78 倍、インクリメンタルビルドで 8.8 倍高速化されました
- [CSS first な設定](https://tailwindcss.com/docs/v4-beta#css-first-configuration)
  - `tailwind.config.js`の代わりに、全ての設定を CSS ファイルに直接書けるようになりました
- [Lightning CSS を利用するように](https://tailwindcss.com/docs/v4-beta#built-in-css-transpilation)
  - これによって`autoprefixer`と`postcss-preset-env`のインストールが不要になりました
- [モダンな CSS のサポート](https://tailwindcss.com/docs/v4-beta#native-css-cascade-layers)
  - コンテナクエリやカスケードレイヤー、ポップオーバー API などがサポートされました

その他多くの新機能が含まれているので、リリースノートをご覧ください。

### React Spectrum November release

https://react-spectrum.adobe.com/releases/2024-11-20.html

React Spectrum の November release がリリースされました。

主な変更点は以下です。

- Accordion と Disclose が generally available に
  - 閉じられているコンテンツをページ内検索時で発見時に開く`hidden="until-found"`が追加されたりしました
- ToggleButtonGroup の追加や Menu の section-level selection group のサポート
- React 19 対応
- TypeScript の strict mode 準拠

### Playwright 1.49 Aria snapshots

https://playwright.dev/docs/release-notes#version-149

アクセシビリティツリーを yaml に変換したものを用いてスナップショットテストを実行することができる機能が追加されました。
role や accessible name、ARIA attributes などをスナップショットに乗せることができ、部分マッチングや正規表現などを用いてスナップショットが一致しているかどうかをテストすることができます。

### Introducing the Leader’s Guide to Accessibility – Accessibility in government

https://accessibility.blog.gov.uk/2024/11/21/introducing-the-leaders-guide-to-accessibility/

イギリスの Home Office（内務省）と DWP（労働・年金省）がそれぞれのデザインシステムのページにおいて、アクセシビリティの重要性を説明するガイドを作成しました。
これらはチームリーダーやプロダクトの責任者などを対象としており、チームメンバーが果たす役割や、アクセシビリティのツール・トレーニング方法などを把握するのに役立ちます。

https://accessibility-manual.dwp.gov.uk/tools-and-resources/govuk-resources

https://design.homeoffice.gov.uk/accessibility/leaders-guide

## あとがき

僕はアクセシビリティに興味があるので、最後の 3 つは特に面白かったです。
アクセシビリティに関する様々なガイドやライブラリ・ツールが整備され、よりアクセシビリティを改善していく活動が広まっていくことを期待していますし、自分自身も貢献していければと思います。
