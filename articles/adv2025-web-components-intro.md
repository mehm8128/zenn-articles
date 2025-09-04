---
title: "イントロダクション - Web Components a11y 1人 Advent Calendar"
emoji: "🎄"
type: "idea"
topics: ["frontend", "webcomponents", "a11y"]
published: false
---

:::message
この記事は [Web Components a11y 1 人 Advent Calendar - Qiita Advent Calendar 2025](https://qiita.com/advent-calendar/2025/web-components-a11y) の 1 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。

去年に引き続き、25 日間 1 人アドベントカレンダーとして Web Components a11y というテーマで記事を投稿していきます。
今回は概要や具体的なトピック、書くこと書かないことについて簡単に紹介します。
なお、去年の 1 人アドベントカレンダー「React Aria の実装読むぞ」は、以下のリンクからご覧ください。

https://qiita.com/advent-calendar/2024/react-aria

会社の先輩方の 1 人アドベントカレンダーはこちらです。

https://blog.sakupi01.com/dev/articles/2025-css-advent-0

## 概要

昨年のアドベントカレンダーでは React Aria を a11y の観点で見ていきました。Web サービスや Web サイトにおいて a11y を確保する上で、このようなライブラリや、これに UI などの要素が加わったデザインシステムが重要な役割を果たします。日本だと freee や SmartHR などの企業だったり、デジタル庁もデザインシステムを公開しています。しかし、多くのデザインシステムのコンポーネントは React などライブラリ・フレームワーク固有のものになってしまっていて、それを別のライブラリ・フレームワークを用いたプロジェクトで使おうとすると手間がかかってしまいます。

そこで、Web Components という Web が標準で提供する機能を使うことで、ライブラリ・フレームワークに依存しないコンポーネントを作成することができるようになります。これにより、ライブラリ・フレームワークを用いることによるパフォーマンス的な懸念や相互運用性の問題を排除することができます。
実際、[Salesforce Lightning Design System](https://www.lightningdesignsystem.com/2e1ef8501/p/85bd85-lightning-design-system-2) や [Spectrum Web Components](https://opensource.adobe.com/spectrum-web-components/) などのデザインシステムでは Web Components が利用されていたり、[OpenUI Design System](https://github.com/openui/design-system) でも Web Components を用いたデザインシステムの構築が検討されています。

ただ、現状 Web Components は、一般的な Web 開発者が React などのライブラリ・フレームワークと同じような開発体験で触ることができるかというとそんなことは無く、まだ様々な辛さを抱えています。
そこで、今回のアドベントカレンダーでは Web Components の基本を確認した後、主に a11y の観点でこの辛さを見ていき、今までいくつかの辛さがどのように解決されてきたのか・現在存在する辛さがどのように解決されようとしているのかを紹介します。

## 具体的なトピック

毎日のトピックは以下のように進んでいく予定です。順番など前後する可能性があるため、最新の予定は [Qiita のカレンダー](https://qiita.com/advent-calendar/2025/web-components-a11y) をご覧ください。

## 書くことと書かないこと

### 書くこと

- Web Components の基本
- Vue や React など Web フレームワーク・ライブラリとの連携
- Lit など周辺ライブラリの紹介
- Web Components を用いたデザインシステムの紹介（OpenUI Design System も含む）
- W3C などで出されている proposal や issue
  - 主に a11y に関係のあるもの
  - Reference Target や Focus Management、ElementInternals、ElementInternals.type など

### 書かないこと

- CSS 関連の話
  - 必要最低限に留めます
  - 代わりに、[🎨 CSS Advent Calendar: Day 16 / Hard Core Scoping of Standard | @sakupi01.com](https://blog.sakupi01.com/dev/articles/2025-css-advent-16/) をご覧ください
- DOM Parts や Template Instantiation、HTML Modules など
  - 少し毛色が違うものっぽいので、今回は扱いません

## まとめ

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、「Custom Element その１」です。お楽しみにー
