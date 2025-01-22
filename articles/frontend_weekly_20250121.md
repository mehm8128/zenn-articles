---
title: "React の ViewTransition コンポーネントなど: Cybozu Frontend Weekly (2025-01-21号)"
emoji: "⚡"
type: "tech"
topics: ["frontend", "cybozufrontendweek"]
published: true
publication_name: "cybozu_frontend"
---

こんにちは！ サイボウズ株式会社 フロントエンドエンジニア（内定者バイト）の [mehm8128 (@mehm8128)](https://x.com/mehm8128) です。

## はじめに

サイボウズ社内では毎週火曜日に Frontend Weekly と題し「一週間の間にあったフロントエンドニュースを共有する会」を開催しています。

今回は、2025/01/21 の Frontend Weekly で取り上げた記事や話題を紹介します。

## 取り上げた記事・話題

## Oracle has informed us they won’t voluntarily withdraw their trademark on "JavaScript"

https://x.com/deno_land/status/1876728474666217739

Oracle は Deno が署名活動を行い "JavaScript" の商標を取り下げを申請していた件に関して、JavaScript の商標を手放すつもりはないと通告しました。

以前 Deno は米国特許商標庁に Oracle が所有する「JavaScript」の商標登録の取り消しを申請していましたが、これに対する返答が今回の通告になります。

https://deno.com/blog/deno-v-oracle

## Cursor Directory

https://cursor.directory/

[Cursor](https://www.cursor.com/)という AI を搭載したエディタで使える、`.cursorrules`のまとめサイトです。
`.cursorrules`（プロンプト）を書いておくと、内容に応じて AI の補完を効率化させることができます。

## Upgrading: Single-Page Apps | Next.js

https://nextjs.org/docs/app/building-your-application/upgrading/single-page-applications

Next.js の公式ドキュメントに、SPA の構築の項目が追加されました。
SPA の開発に Next.js を使用するメリットや React v19 から使える `use` を用いた方法、SWR を利用することでサーバー機能を徐々に採用することができることなどが説明されています。

## Storybook v9 に向けた RFC

https://x.com/storybookjs/status/1878939055582519432

Storybook v9 に向けて以下の 2 つの RFC が出されているようです。

- [[RFC]: Typesafe CSF factories](https://github.com/storybookjs/storybook/discussions/30112)
  - typesafe に CSF を書けるようにするような提案です
- [[RFC]: Add the .test method to CSF factories](https://github.com/storybookjs/storybook/discussions/30119)
  - 従来の play 関数のテストを、Jest などのように`test`メソッドを用いて書くことができるようにする提案です

## Vite が Rolldown ベースになることで更なる高速化が見込まれる

https://x.com/brandontroberts/status/1878879880781480442

現在 v1.0 beta になっている Rolldown が Vite に入ることでビルド速度が改善しそうという解説動画です。
Storybook など、Vite に依存するサービスやライブラリもこのメリットを享受できる可能性があり、今後ビルド時間が改善しそうです。

## The success of Interop 2024!

https://webkit.org/blog/16413/the-success-of-interop-2024/

Interop2024 の成果発表ブログです。
2024 年は 95%と、過去最高の達成率を記録したと述べられています。
記事では、Webkit が特に注力したポイントが取り上げられています。

## Storybook と Playwright ではじめるインタラクションテスト

https://techblog.enechain.com/entry/frontend-interaction-testing

enechain のテスト戦略についての記事です。
Storybook Test runner は Jest などの`test`関数を用いたテストの記述ができないので、代わりに Playwright を用いて Storybook を直接見てテストする手法が説明されています。
将来的には Portable stories + Playwright CT を使った方法への移行を検討しているとのことです。

## WebDriverIO v9.5.0 のリリース

https://x.com/webdriverio/status/1873806664228626857

WebDriverIO v9.5.0 がリリースされました。
Web と Android、iOS に対応した swipe、tap コマンドが追加されました。

## Testing | react-spectrum.adobe.com

https://react-spectrum.adobe.com/react-aria/testing.html

React Aria に `@react-aria/test-utils` というパッケージが追加されました。

Combobox や Table など、いくつかのコンポーネントパターンに対してそれぞれテスト用のヘルパーメソッドが用意されました。
例えば Combobox なら`open`、`findOption`、`selectOption`などのメソッドが提供されています。

## Vitest 3.0 is out! | Vitest

https://vitest.dev/blog/vitest-3

Vitest の v3.0 がリリースされました。主な変更点は以下です。

- テスト実行中のコンソールのちらつきが減少して安定した出力に
- Inline Workspace 機能が追加され、[Workspace](https://vitest.dev/guide/workspace)定義用のファイルを新しく作らなくて良いように
- 複数のブラウザでテストするための新しい仕組み[browser.instances](https://vitest.dev/guide/browser/multiple-setups)オプションが利用可能に
- テスト実行時、ファイル名に行番号を指定してテストを絞り込めるように

## The Browser Company teases Dia, its new AI browser | TechCrunch

https://techcrunch.com/2024/12/02/the-browser-company-teases-dia-its-new-ai-browser/

Arc ブラウザを開発している The Browser Company が、 Dia という新しいブラウザを 2025 年初頭に公開予定です。

## no-misused-spread | typescript-eslint

https://typescript-eslint.io/rules/no-misused-spread/

typescript-eslint v8.20 で追加された `no-misused-spread` ルールは`[...'hoge']`を`'hoge'.split('')`に強制します。
JavaScript では `length` や `split` を CodeUnit(16bit) 単位で計算しますが、Iterator 経由で反復する場合は CodePoint で計算するという挙動の違いがあり、この挙動差による意図しないミスを防ぐのが目的のようです。

## 令和 7 年版 あなたが使ってよいフロントエンド機能とは

https://speakerdeck.com/mugi_uno/baseline-ha-iizo

弊社フロントエンドエキスパートの mugi さんによる、ブラウザごとの機能サポートの従来の考え方と、それに対しての一つの解決策である Baseline について解説している発表資料です。
Baseline に含まれる 3 つのステータスに加え、今後新たに検討されるかもしれないステータス（Upcoming など）についても触れられています。

## Revealed: React's experimental animations API

https://motion.dev/blog/reacts-experimental-view-transition-api
https://azukiazusa.dev/blog/declarative-page-transition-animation-with-react-viewtransition-component/

View Transition API を React で使用する手段として、`unstable_ViewTransition`が実験的に利用できるようになりました。
`unstable_ViewTransition`と`startTransition()`を利用することで、これまで State 管理との関係で React との併用が難しかった View Transition API が利用可能になりそうです。

## 2024 year in review | Astro

https://astro.build/blog/year-in-review-2024/

Astro の 2024 年のまとめと 2025 年の展望が書かれた記事です。

前半には Content Layer や Astro Actions など、 2024 年に追加された機能の紹介がされています。
後半には 2025 年にセッション系の API を出したいという話や、レスポンシブな画像を扱うときに Image コンポーネントのオプションを Astro 側で自動生成できるようにしたいという展望が語られています。

また、日本語の原文があります。
https://zenn.dev/morinokami/articles/astro-2024-2025

## あとがき

Storybook の Story やテストの書き方が大きく変わりそうという話と、React に`<ViewTransition>`コンポーネントが追加された話が興味深かったです。
