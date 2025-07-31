---
title: "State of HTML 2025 の開始など: Cybozu Frontend Weekly (2025-07-29号)"
emoji: "🎆"
type: "idea"
topics: ["frontend", "cybozufrontendweek"]
published: true
publication_name: "cybozu_frontend"
---

こんにちは！ サイボウズ株式会社 フロントエンドエンジニアの [mehm8128 (@mehm8128)](https://x.com/mehm8128) です。

## はじめに

サイボウズ社内では毎週火曜日に Frontend Weekly と題し「一週間の間にあったフロントエンドニュースを共有する会」を開催しています。

今回は、2025/07/29 の Frontend Weekly で取り上げた記事や話題を紹介します。

## 取り上げた記事・話題

### Stylus が npm レジストリから削除された件

https://github.com/stylus/stylus/issues/2938

Stylus が npm レジストリがから削除された件について、一時的な対応策などが Issue に記載されていましたが、再びアクセス可能になったとのことです。

詳細な経緯がこちらの記事に書かれています。
https://www.bleepingcomputer.com/news/security/npm-accidentally-removes-stylus-package-breaks-builds-and-pipelines/

Stylus のメンテナーの 1 人が（悪意のあるコードだとされている）バグを含んだコードを公開した結果アカウントが BAN され、そのアカウントに紐づく（Stylus を含む）パッケージが全て削除されてしまったのが原因とのことです。
よって、Stylus 自体には悪意のあるコードは含まれていないと説明されています。

> "Panya, who is one of the maintainers of the stylus package, published them, and because of that, his account was banned, and all the packages that were connected to him were yanked, including the Stylus one. So that's the story here. A big false alarm by NPM," states Abai.

### ユーザー投稿のコードをブラウザ上で他人が動かせるサービスを作るときのセキュリティについて

https://zenn.dev/catnose99/scraps/b2b3420a06eb53

ユーザーがコード投稿し、それを他のユーザーがブラウザ上で動かすことができるサービスを作るときのセキュリティについて、まとめられたスクラップです。

いくつかの問題点が取り上げられ、HttpOnly や iframe、CSP などを用いた対策方法が説明されています。

### OSS 貢献を「依頼」から「協力」に変える、Issue とプルリクエストの書き方

https://findy-code.io/media/articles/codesidechat-sapphi_red_ja

Vite のコアチームメンバーである翠さんが、OSS のメンテナー視点で「すぐ対応したくなる」Issue や PR の特徴を解説しています。

Issue の作成については Minimal Reproducible Example（最小の再現コード）や XY 問題について説明されていて、PR の作成については変更内容についてレビュワーに納得してもらうための説明方法や、「変更が安全である」ことを伝えるための方法が紹介されています。

### コントラストについて改めて考える

https://lydesign.jp/n/ndc6de5db7178

Tech-Verse 2025 での発表「文字コントラストを改めて考える」の内容を再構成した記事です。

WCAG 1.0（コントラスト差と色差）、WCAG 2.0（コントラスト比）、WCAG 3.0（APCA）でのそれぞれのコントラストの基準について説明し、LINE ヤフーのブランドカラーにおける課題や今後の対応として考えられることを紹介しています。

### Take the State of HTML survey today

https://web.dev/blog/state-of-html-2025

State of HTML 2025 のサーベイが始まりました。
タイトルは「State of HTML」ですが、Web に関する機能が幅広く対象となっており、この結果は Interop プロジェクトに影響してくるとのことです。

### React Server Components: How We Got Here

https://www.epicreact.dev/react-server-components-how-we-got-here-zcuxn

Web 開発のアークテクチャについて、これまでの MPA → SPA → PESPA (Progressive Enhancement SPAs) という変遷を振り返り、最新のアーキテクチャである React Server Components のメリットについて解説している記事です。
React Server Components の最も注目すべき点を、ローディングやエラーなどの状態、データフェッチなどのネットワーク管理が不要になることだと説明しています。

### Support for React Server Components · Issue #1209 · testing-library/react-testing-library

https://github.com/testing-library/react-testing-library/issues/1209#issuecomment-3122021060

このコメントで紹介されている vitest-plugin-rsc というものを用いて、Testing Library で React Server Components のテストが書けるようになるとのことです。

以下のようなサンプルコードが紹介されています。

```tsx
import { renderServer } from "vitest-plugin-rsc/testing-library";

await renderServer(<Component />);
```

### Vanilla JavaScript support for Tailwind Plus

https://tailwindcss.com/blog/vanilla-js-support-for-tailwind-plus

Tailwind Plus に、カスタム要素を用いて HTML のみで利用できるインタラクティブなコンポーネントが登場しました。
ドロップダウンやダイアログ、Disclosure などのコンポーネントが提供されています。

記事では Svelte での使用例が紹介されています。

### あとがき

State of HTML 2025 に早速回答してみました。知っているものも多かったのですが、System Capabilities のページは知らないものが多かったです...。
是非みなさんも回答してみてください！
