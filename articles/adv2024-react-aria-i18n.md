---
title: "i18nについて - React Ariaの実装読むぞ"
emoji: "🌏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: true
---

:::message
この記事は [React Aria の実装読むぞ - Qiita Advent Calendar 2024](https://qiita.com/advent-calendar/2024/react-aria) の 21 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は i18n について書いていきます。

## React Aria の i18n

React Aria の i18n についてはこのページにまとめられています。
https://react-spectrum.adobe.com/react-aria/internationalization.html

React Aria では主に、文字列の翻訳、日付と数値のフォーマット、RTL がサポートされています。
そしてこれらはブラウザに組み込まれている `Intl` オブジェクトの各 API を利用して実装されています。
Intl については saji さんの 1 人アドベントカレンダーでまとめられているので、適宜参照しながら解説していきます。

https://adventar.org/calendars/10555

Intl の全体像についてはこちらの記事をご覧ください。
https://zenn.dev/sajikix/articles/intl-advent-calendar-24-01

### 文字列の翻訳

`useLocalizedStringFormatter`を用いて現在の locale に基づいて翻訳されたテキストを取得できます。ここでは Intl は使われていません多分。
例えば[`useNumberField`](https://zenn.dev/mehm8128/articles/adv2024-react-aria-number-field)だと、+/-ボタンに振られているラベルが翻訳されています。

https://github.com/adobe/react-spectrum/blob/50c7ada5d1880a174b6b6d3f43e8d90ee9bd4ad8/packages/%40react-aria/numberfield/src/useNumberField.ts#L93
https://github.com/adobe/react-spectrum/blob/50c7ada5d1880a174b6b6d3f43e8d90ee9bd4ad8/packages/%40react-aria/numberfield/src/useNumberField.ts#L285

`en-US.json`と`ja-JP.json`を見てみます。`Increase`と`Decrease`をそれぞれ「拡大」と「縮小」と訳すのはかなり微妙な気もするのですが、こんな感じに翻訳されています。
https://github.com/adobe/react-spectrum/blob/50c7ada5d1880a174b6b6d3f43e8d90ee9bd4ad8/packages/%40react-aria/numberfield/intl/en-US.json#L1-L6
https://github.com/adobe/react-spectrum/blob/50c7ada5d1880a174b6b6d3f43e8d90ee9bd4ad8/packages/%40react-aria/numberfield/intl/ja-JP.json#L1-L5

`useLocalizedStringFormatter`関数内では`useLocale`で現在の locale を取得し、その locale のテキストに翻訳するための`format`メソッドを含む`LocalizedStringFormatter`オブジェクトを返しています。
https://github.com/adobe/react-spectrum/blob/50c7ada5d1880a174b6b6d3f43e8d90ee9bd4ad8/packages/%40react-aria/i18n/src/useLocalizedStringFormatter.ts#L40-L44

`useColorPicker`でも、同じく翻訳用のパッケージが利用されていました。
https://zenn.dev/mehm8128/articles/adv2024-react-aria-color-picker

### 数値のフォーマット

再び`useNumberField`の話です。`useNumberField`では様々な数値のフィーマットに対応していて、ブログ記事にまとめられています。

https://react-spectrum.adobe.com/blog/how-we-internationalized-our-numberfield.html

`Intl.NumberFormat`を用いて小数点や％や通貨、単位などのフォーマットをいい感じにしています。

`Intl.NumberFormat`についてはここらへんの記事を読みましょう。
https://zenn.dev/sajikix/articles/intl-advent-calendar-24-13
https://zenn.dev/sajikix/articles/intl-advent-calendar-24-14
https://zenn.dev/sajikix/articles/intl-advent-calendar-24-15

### RTL

アラビア語など、右から左に文字を書く言語圏の locale の場合、画面内の要素もミラーリングする必要があります。
ただし、React Aria はスタイリングにまでは責任を持っていないので、React Aria は主にキーボード操作などのインタラクション部分で RTL サポートをしています。

`useLocale`から`direction`という`ltr | rtl`の union 型の値を取得し、その値に応じてインタラクションの方向を変化させます。
https://github.com/adobe/react-spectrum/blob/50c7ada5d1880a174b6b6d3f43e8d90ee9bd4ad8/packages/%40react-aria/i18n/src/context.tsx#L54-L58

例えば[`useTabList`](https://zenn.dev/mehm8128/articles/adv2024-react-aria-tab)の場合、`direction`を`TabsKeyboardDelegate`というオブジェクトのコンストラクタに渡しています。

https://github.com/adobe/react-spectrum/blob/50c7ada5d1880a174b6b6d3f43e8d90ee9bd4ad8/packages/%40react-aria/tabs/src/useTabList.ts#L44-L49

`TabsKeyboardDelegate`はこれを受け取り、`flipDirection`を定義するのに使います。`flipDirection`は`true`のときにキーボード操作を反転させます。
https://github.com/adobe/react-spectrum/blob/50c7ada5d1880a174b6b6d3f43e8d90ee9bd4ad8/packages/%40react-aria/tabs/src/TabsKeyboardDelegate.ts#L21-L26

`getKeyLeftOf`という関数では引数の`key`の 1 つ左の`key`を取得しますが、通常時（`direction = ltr`のとき）は 1 つ左というのは 1 つ前の`key`ですが、`flipDirection`時には 1 つ左は 1 つ次の`key`になるので（見た目がミラーリングされても ← キーを押したときには左に移動する、みたいになっています）、`getNextKey`関数を使用しています。
https://github.com/adobe/react-spectrum/blob/50c7ada5d1880a174b6b6d3f43e8d90ee9bd4ad8/packages/%40react-aria/tabs/src/TabsKeyboardDelegate.ts#L28-L40

### Typeahead

[`useListBox`](https://zenn.dev/mehm8128/articles/adv2024-react-aria-listbox)や[`useGridList`](https://zenn.dev/mehm8128/articles/adv2024-react-aria-gridlist)、[`useMenu`](https://zenn.dev/mehm8128/articles/adv2024-react-aria-menu)では Typeahead が実装されています。
[`useListBox`](https://zenn.dev/mehm8128/articles/adv2024-react-aria-listbox#typeahead)の記事で軽く言及していました。が、Typeahead についての説明が抜けていたので説明すると、リストにフォーカスしているときに頭文字を入力するとその文字から始まるリストアイテムにフォーカスを移動できるというものです。

これは`useCollator`を用いて実装されていて、内部で`Intl.Collator`を使っています。

https://github.com/adobe/react-spectrum/blob/50c7ada5d1880a174b6b6d3f43e8d90ee9bd4ad8/packages/%40react-aria/i18n/src/useCollator.ts#L22-L33

`Intl.Collator`についてはこちらの記事をご覧ください。
https://zenn.dev/sajikix/articles/intl-advent-calendar-24-19

上記の 3 つの hooks で共通で使われている`useSelectableList`では、ここで`useCollator`を用いて`collator`を取得し、`ListKeyboardDelegate`オブジェクトのコンストラクタに渡しています。
https://github.com/adobe/react-spectrum/blob/50c7ada5d1880a174b6b6d3f43e8d90ee9bd4ad8/packages/%40react-aria/selection/src/useSelectableList.ts#L62-L73

`ListKeyboardDelegate`はこれを受け取り、`getKeyForSearch`でリストアイテムの検索に用いてます。
https://github.com/adobe/react-spectrum/blob/50c7ada5d1880a174b6b6d3f43e8d90ee9bd4ad8/packages/%40react-aria/selection/src/ListKeyboardDelegate.ts#L269-L290

### 日付のフォーマット

日付も地域によって様々なフォーマットがあるので、フォーマットされています。詳しくは明日の記事で書いたり書かなかったりするのですが、`useDateFormatter`という hook が提供されていて、その中で`Intl.DateTimeFormat`を利用しています。

`useDateSegment`ではここらへんで使われています。
https://github.com/adobe/react-spectrum/blob/50c7ada5d1880a174b6b6d3f43e8d90ee9bd4ad8/packages/%40react-aria/datepicker/src/useDateSegment.ts#L42-L54

日付についての公式ブログ記事はこちらです。
https://react-spectrum.adobe.com/blog/date-and-time-pickers-for-all.html

`Intl.DateTimeFormat`についてはここらへんの記事を読むといいです。
https://zenn.dev/sajikix/articles/intl-advent-calendar-24-07
https://zenn.dev/sajikix/articles/intl-advent-calendar-24-08
https://zenn.dev/sajikix/articles/intl-advent-calendar-24-09

## まとめ

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、DateField についての記事です。お楽しみにー
