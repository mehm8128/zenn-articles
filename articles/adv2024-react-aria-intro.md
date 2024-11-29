---
title: "イントロダクション - React Ariaの実装読むぞ"
emoji: "🎄"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。

これから 25 日間、1 人アドベントカレンダーとして React Aria を読んでいきます。

来年正社員として新卒入社するまで暇だからなんかやることあるかなって話をしていたら、先輩社員の方に勧められて 1 人アドベントカレンダーなるものを始めてしまったのですが、知り合い含めて意外とやる人が多くてびっくりしました。特に社内の先輩方だと、React Aria に関係ありそうな i18n の話や Open UI の話を書く方がいるのでそちらの記事も楽しみにしています。

Qiita の方でカレンダーを作っていて（完走賞ほしい）、購読者が少しずつ増えていってちゃんといい記事を書けるか不安ではあるのですが、あまり気にせずに自分の書きたいことを書いていこうと思います。

https://qiita.com/advent-calendar/2024/react-aria

また、Adventar の方にアクセシビリティのカレンダーがあったので、そちらにも参加させていただいています。
[Accessible Name and Description Computation 1.2](https://www.w3.org/TR/accname-1.2/)についてまとめようと思っているので、お楽しみに。

https://adventar.org/calendars/9957

今回は初回なので、まずは React Aria について自分が理解している範囲で軽く説明していこうと思います。

## React Aria とは

Adobe が OSS として公開しているデザインシステムの React Spectrum と共に公開されている、headless な React components と hooks ライブラリです。
a11y や i18n などに重点を置いて実装されており、React Spectrum 内でも利用されています。
また、React Stately というパッケージと組み合わせて利用することでコンポーネントに必要な状態管理も簡単に行うことができるようになっています。

その他の機能はまっつーさんのスライドや記事をご覧ください。

https://speakerdeck.com/ryo_manba/react-aria-deshi-xian-suruci-shi-dai-noakusesibiritei

https://zenn.dev/ryo_manba/articles/ccd0fddcb5e02c

https://zenn.dev/cybozu_frontend/articles/react-aria-component-design

## 書くことと書かないこと

### 書くこと

実装を読むというタイトルにはしましたが、

- [React Aria のドキュメント](https://react-spectrum.adobe.com/react-aria/getting-started.html)
- [ARIA Authoring Practices Guide (APG)](https://www.w3.org/WAI/ARIA/apg/)
- [Accessible Rich Internet Applications (WAI-ARIA) 1.2](https://www.w3.org/TR/wai-aria-1.2/)

なども読みつつ、ソースコードを読んで実際にどのような a11y 対応がサポートされているかを見ていき、深掘りたいところは深掘ったり、深掘りたくないところはスルーしたりしながら進めていきます。
解説というよりは自分の勉強メモという感じになると思いますがよろしくお願いします。

### 書かないこと

デザインシステムである React Spectrum や、 React Aria Components の実装・インターフェースなどはあまり書きません。今回は hooks に焦点を当てていこうと思います。

## まとめ

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、 Button についての記事です。お楽しみにー
