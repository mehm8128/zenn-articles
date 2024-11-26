---
title: "Accessible Name and Description Computation 1.2 を読む"
emoji: "💻"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "a11y", "w3c"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
前に Zenn Scrap で調査した、Accessible Name and Description Computation 1.2 の内容を改めて記事としてまとめます。

Scrap はこちら（この記事でまとめ直しているので Scrap は読む必要ありません）。

https://zenn.dev/mehm8128/scraps/a66a7f7fb6a6aa

## Accessible Name and Description Computation とは

Accessible Name and Description Computation とは、user agent が accessible name や accessible description をどのように計算するかを定義しているドキュメントです。
とある文脈で、それらの計算方法が知りたくなったので調べていたらこのドキュメントに辿りついたので読んでいました。

内容としては主に以下の 4 つです。

- 用語の定義
- accessible name の計算方法
- accessible description の計算方法
- text alternative の計算方法

なお、text alternative とは accessible name と accessible description の両方の計算で使用される、各 HTML 要素に対して計算される文字列のことです。

現在は 2018 年に publish された 1.1 が Recommendation ですが、1.2 が Working Draft として出ていて 2024 年現在も更新中なので、1.1 と 1.2 の間で主に何が更新されているのかを見ていきます。

## 1.2 での主な変更点

変更点はここにまとめられています。

[6.1.1 Substantive changes since the last public working draft](https://www.w3.org/TR/accname-1.2/#substantive-changes-since-the-last-public-working-draft)

僕が気になったものを 3 つ紹介します。

### Support aria-description

[Support aria-description by aleventhal · Pull Request #69 · w3c/accname](https://github.com/w3c/accname/pull/69)

1.1 では [4.2 Description Computation (1.1)](https://www.w3.org/TR/accname-1.1/#mapping_additional_nd_description) で`aria-describedby`が参照する要素の text alternative のみが accessible description になるような説明になっていました。
しかし、

- WAI-ARIA 1.3 で`aria-description`が追加された
- `title`属性、テーブルの`caption`要素、`input`要素の`value`なども考慮される

など、`aria-describedby`以外のものも考慮されるので、それらを踏まえてどのような優先度で accessible description が計算されるのかが表形式でまとめられています。

[4.2 Description Computation (1.2)](https://www.w3.org/TR/accname-1.2/#mapping_additional_nd_description)

ここで、1.1 と 1.2 での accessible name と accessible description の計算方法及びそれらと text alternative の関係についてまとめます。

|     | name                        | description                                                       |
| --- | --------------------------- | ----------------------------------------------------------------- |
| 1.1 | その要素の text alternative | `aria-describedby`が参照する要素の text alternative               |
| 1.2 | その要素の text alternative | 表を上から順に計算。適用できなかったら 1 つ下の行の計算をしていく |

ついでに 1.2 で追加された accessible description の計算方法の表も翻訳＆簡略化して載せておきます（下 2 つはおそらく HTML に限らない説明になっていましたが、分かりやすく HTML の例で書きます）。

| 属性                                                             | 計算方法                                                    |
| ---------------------------------------------------------------- | ----------------------------------------------------------- |
| `aria-describedby`                                               | 参照している要素の accessible name を計算し、スペース区切り |
| `aria-description`                                               | 指定した文字列をそのまま採用                                |
| HTML 要素やその属性（`input`の`value`、テーブルの`caption`など） | text alternative か、指定した文字列をそのまま採用           |
| `title`属性                                                      | 指定した文字列をそのまま採用                                |

### suggested simplification

[suggested simplification by MelSumner · Pull Request #122 · w3c/accname](https://github.com/w3c/accname/pull/122)

[4.3 Accessible Name and Description Computation (1.1)](https://www.w3.org/TR/accname/#mapping_additional_nd_te) で説明されていた text alternative の計算ステップの順番が変更されたことにより、説明が簡素化＆バグも修正されました（[4.3.2 Computation steps (1.2)](https://www.w3.org/TR/accname-1.2/#computation-steps)）。

この計算ステップについては後ほど説明します。

### add name from prohibited

[add name from prohibited by billybonks · Pull Request #71 · w3c/accname](https://github.com/w3c/accname/pull/71)

WAI-ARIA role にはそれぞれ `nameFrom`というプロパティがあり、accessible name がどこから計算されることができるかが決まっています。

[5.2.8 Accessible Name Calculation - WAI-ARIA 1.2](https://www.w3.org/TR/wai-aria-1.2/#namecalculation)

`nameFrom`の種類は以下の 3 つです。

| 種類         | 説明                                                          |
| ------------ | ------------------------------------------------------------- |
| `author`     | `aria-label`や`aria-labelledby`などから計算されることができる |
| `contents`   | 子要素などから計算されることができる                          |
| `prohibited` | accessible name をつけることができない                        |

1.1 では text alternative の計算方法に`prohibited`についての記述がなかったのですが、1.2 で追加されていたので`prohibited`についての説明も追加されました。

例えば `button` role だと [Accessible Rich Internet Applications (WAI-ARIA) 1.2](https://www.w3.org/TR/wai-aria-1.2/#button) を見てみると、表の`Name From`の行に`contents`, `author`とあるので、`aria-label`などを用いて accessible name を指定するか、子要素から計算されることができます。ただし、`author`の方が優先されるので`aria-label`などが与えられていない場合のみに`contents`が採用されます。

## 計算ステップの詳細

text alternative の計算ステップについてまとめます。が、難しそうなので重要そうなところを雰囲気でまとめてます。

## おまけ

React Aria のソースコード上でも参照されていました。

https://github.com/adobe/react-spectrum/blob/main/packages/react-aria-components/src/Button.tsx#L137-L138

## まとめ
