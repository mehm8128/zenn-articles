---
title: "イントロダクション - React Ariaの実装読むぞ"
emoji: "🎄"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

:::message
この記事は [React Aria の実装読むぞ - Qiita Advent Calendar 2024](https://qiita.com/advent-calendar/2024/react-aria) の 1 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。

これから 25 日間、1 人アドベントカレンダーとして React Aria を読んでいきます。
今日は具体的な話はしないので idea の方に投稿させてもらっています。

僕は大学を半年早期卒業して今は就職先の会社で内定者アルバイトをさせてもらっています。そこで、来年正社員として新卒入社するまで暇だからなんかやることあるかなって話をしていたら、先輩社員の方に勧められて 1 人アドベントカレンダーなるものを始めてしまいました...。ですが知り合い含めて意外とやる人が多くてびっくりしました。特に社内の先輩方だと、React Aria に関係ありそうな [i18n の話](https://adventar.org/calendars/10555) や [Open UI の話](https://adventar.org/calendars/10293) を書く方がいるのでそちらの記事も楽しみにしています。

Qiita の方でカレンダーを作っていて（完走賞ほしい）、購読者が少しずつ増えていってちゃんといい記事を書けるか不安ではあるのですが、あまり気にせずに自分の書きたいことを書いていこうと思います。あと、カレンダーに入ってるタイトルはあくまでも予定なので結構変わったりずれたりします（公開したタイミングで直します）。

https://qiita.com/advent-calendar/2024/react-aria

また、Adventar の方にアクセシビリティのカレンダーがあったので、そちらにも参加させていただいています。
[Accessible Name and Description Computation 1.2](https://www.w3.org/TR/accname-1.2/) についてまとめて、既に記事は完成しているので、お楽しみに。

https://adventar.org/calendars/9957

今回は初回なので、まずは React Aria について自分が理解している範囲で軽く説明していこうと思います。

## React Aria とは

Adobe が OSS として公開しているデザインシステムの React Spectrum と共に公開されている、headless な React components と hooks ライブラリです。
a11y や i18n などに重点を置いて実装されており、React Spectrum 内でも利用されています。
また、React Stately というパッケージと組み合わせて利用することでコンポーネントに必要な状態管理も簡単に行うことができるようになっています。

また、React Spectrum は[v2](https://s2.spectrum.adobe.com/)が準備されている途中です。知らなくて開発環境の Storybook で謎の`s2`というのが気になっていたので一応。

その他の機能はまっつーさんのスライドや記事をご覧ください。

https://speakerdeck.com/ryo_manba/react-aria-deshi-xian-suruci-shi-dai-noakusesibiritei

https://zenn.dev/ryo_manba/articles/ccd0fddcb5e02c

https://zenn.dev/cybozu_frontend/articles/react-aria-component-design

## 書くことと書かないこと

### 書くこと

実装を読むというタイトルにはしましたが、

- [React Aria の issue](https://github.com/adobe/react-spectrum/issues?q=sort%3Aupdated-desc+is%3Aissue+is%3Aopen)
- [React Aria のドキュメント](https://react-spectrum.adobe.com/react-aria/getting-started.html)
- [ARIA Authoring Practices Guide (APG)](https://www.w3.org/WAI/ARIA/apg/)
- [Accessible Rich Internet Applications (WAI-ARIA) 1.2](https://www.w3.org/TR/wai-aria-1.2/)

なども読みつつ、ソースコードを読んで実際にどのような a11y 対応がサポートされているかを見ていき、深掘りたいところは深掘ったり、深掘りたくないところはスルーしたりしながら進めていきます。
解説というよりは自分の勉強メモという感じになると思いますがよろしくお願いします。

また、~~枠稼ぎ~~ 番外編として React Aria でのテストの話や i18n の話なども挟んでいく予定です。

### 書かないこと

デザインシステムである React Spectrum や、 React Aria Components の実装・インターフェースなどはあまり書きません。今回は hooks に焦点を当てていこうと思います。

## まとめ

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、 Button についての記事です。お楽しみにー
