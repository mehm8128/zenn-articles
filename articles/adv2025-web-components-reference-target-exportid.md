---
title: "Reference Target exportid - Web Components a11y 1人 Advent Calendar"
emoji: "🎯"
type: "tech"
topics: ["frontend", "webcomponents", "a11y", "waiaria"]
published: false
---

:::message
この記事は [Web Components a11y 1 人 Advent Calendar - Qiita Advent Calendar 2025](https://qiita.com/advent-calendar/2025/web-components-a11y) の n 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。

https://github.com/WICG/aom/blob/gh-pages/exportid-explainer.md

`exportid`という bool 値で export し、`for="hostid::id(childid)`で参照できる
ネストされてる場合は`forwardids="id"`で forward する。rename もできる

shadow tree 内から外への参照もできる。refection でできるけど、declarative な方法がなかった
カスタム要素で`useids="inner-id: outer-idref"`して、内部で`aria-describedby=":host::id(inner-id)`みたいにする

兄弟ツリー（複合パターン）でもいける

感じた問題点：内部実装（どの id があるか）を知る必要がある。既存の属性の仕様を変更する必要がある（`inner-id: outer-idref`みたいなのをサポートする必要があるので）

getElement するときのカプセル化の維持についても説明されてる

useid がカスタム要素を使うときに毎回指定するので、簡潔にしたいらしい

ref だけで、aria-label とかを渡せるようにしたい問題の解決にはなっていない

reject された理由
複雑＆Reference Target が出てきたのでいらなくなった
https://groups.google.com/a/chromium.org/g/blink-dev/c/CEdbbQXPIRk?utm_source=chatgpt.com

## まとめ

明日はについて紹介します。
