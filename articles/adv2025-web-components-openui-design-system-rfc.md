---
title: "OpenUI Design System の RFC - Web Components a11y 1人 Advent Calendar"
emoji: "🔍"
type: "tech"
topics: ["frontend", "webcomponents", "a11y", "designsystem", "openui"]
published: false
---

:::message
この記事は [Web Components a11y 1 人 Advent Calendar - Qiita Advent Calendar 2025](https://qiita.com/advent-calendar/2025/web-components-a11y) の n 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。

前回は OpenUI Design System について概要を見てきました。今日は、実際に現在 OpenUI Design System に出ている 3 つのコンポーネントの RFC を見ていきます。

## RFC に書かれていること

## Badge コンポーネント

軽く自分で実装してみると良さそう

https://github.com/openui/design-system/pull/9

- a11y 的な考慮点として、「色だけで伝えるな」を含めておいて欲しさがある
  - 丸くて文字が書いていないバッジで、色だけで何かを種類分けしていると色覚特性によっては判別できないので
    https://waic.jp/translations/WCAG22/#use-of-color
- `<oui-badge.ribbon>`って書き方できるの？
- a11y の考慮事項、もう少し包括的に書く必要がありそう？「スクリーンリーダー」の項目が具体的すぎる気もする
- `aria-label`って props 作るのって問題ないの？型付け的に`ariaLabel`にしたさ
- プロパティ多い？

  - プロパティを CSS 変数にしたいの、分かる
  - https://github.com/openui/design-system/pull/9#discussion_r2011062136

  ## Progress コンポーネント

https://github.com/openui/design-system/pull/10

- aria-valuetext は？

## Switch コンポーネント

https://github.com/openui/design-system/pull/11

- a11y の観点として、2 値を区別できるような見た目にすることを書く必要ある？もう 1 回読んでみる

## まとめ

明日は Form-Associated Custom Element について紹介します。
