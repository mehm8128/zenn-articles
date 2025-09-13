---
title: "Reference Target イントロダクション - Web Components a11y 1人 Advent Calendar"
emoji: "🎯"
type: "tech"
topics: ["frontend", "webcomponents", "a11y", "waiaria"]
published: false
---

:::message
この記事は [Web Components a11y 1 人 Advent Calendar - Qiita Advent Calendar 2025](https://qiita.com/advent-calendar/2025/web-components-a11y) の n 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。

今日から 5 日間は、Reference Target について紹介します。

## 問題点

https://alice.pages.igalia.com/blog/how-shadow-dom-and-accessibility-are-in-conflict/

autocomplete を例にして、aria-activedescendant が shadowDOM 内の要素を参照しないことを説明している
同じ shadowroot 内であれば、slot 化されているオプションでも`slot.assignNodes()[1]`みたいにして取れるようになったけど、オプション自体がさらに shadowDOM に入ってしまっていたら外からは取れない
ShadowDOM は実装をカプセル化し、隠蔽することでユーザーにとっても扱いやすくなるし、I/F だけ変わっていなければ内部を自由に変えられるという意味でコンポーネントの開発者にとっても大きなメリットがある。しかし、その内部の要素との関係性を示したいけどコードで表現できないという、ページのセマンティクス・アクセシビリティと矛盾する状況が発生しうる
実装は隠蔽したいけど、ページのユーザーは要素を認識できるので、そこで矛盾が発生している？

proposal の問題点のところからも取ってくる
Background の図がほしい

## 解決策

1 reflection
内部から外部を参照はできるようになった
今年から使えるようになったブラウザが多そう
https://caniuse.com/?search=ariaActiveDescendantElement%20

宣言的には使えない

2 reflection を open な shadowroot にも使えるようにする
問題点
a. カプセル化が失われてしまう可能性
b. closed な shadowroot ではどうする？
c. 宣言的に使えない

残りは別の日に

どの解決案でも共通で必須の条件は行か

- シリアル化可能
  - declarative に使えるようにするため
- closed と open の両方の shadowroot で機能する
- shadow dom のカプセル化を保持する

https://nolanlawson.com/2022/11/28/shadow-dom-and-accessibility-the-trouble-with-aria/

## デザインシステムでの実装

Spectrum
気合で aria-lebel の設定やフォーカス移動をやっていた
https://github.com/adobe/spectrum-web-components/blob/main/packages/field-label/src/FieldLabel.ts#L81
https://github.com/adobe/spectrum-web-components/blob/main/packages/field-label/src/FieldLabel.ts#L97
Material Web
label は input に上手く当たっていなくて、読み上げ失敗してる
フォーカスは、formAssociated: true にしつつ delegateFocus: true にすれば当たってくれるっぽい
https://material-web.dev/components/checkbox/#label
https://github.com/material-components/material-web/blob/main/checkbox/internal/checkbox.ts

## まとめ

明日はについて紹介します。
