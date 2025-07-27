---
title: "命令的な ARIA Live region：ARIA Notifyの紹介"
emoji: ""
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "a11y", "aria", "waiaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今回は、Edge 136 から Origin Trial で導入されており、Chrome 140 でも導入予定も ARIA Notify について紹介します。

## ARIA Notify とは

https://blogs.windows.com/msedgedev/2025/05/05/creating-a-more-accessible-web-with-aria-notify/

explainer: https://github.com/MicrosoftEdge/MSEdgeExplainers/blob/main/Accessibility/AriaNotify/explainer.md
aria の PR: https://github.com/w3c/aria/pull/2577
aria の PR2？: https://github.com/w3c/aria/pull/2211
aria の issue: https://github.com/w3c/aria/issues?q=is%3Aissue%20state%3Aopen%20%20%22ariaNotify%22%20in%3Atitle
Chrome Intent to Ship: https://groups.google.com/a/chromium.org/g/blink-dev/c/QCtWzIPgcCY/m/RSuXobocDAAJ
design-reviews: https://github.com/w3ctag/design-reviews/issues/1075
discussion: https://github.com/w3c/aria/discussions/1958
Design Doc: https://docs.google.com/document/d/1tFT-4_sDvgnZoS8AYEcQquXzqAYaoB53DBH0C2T5rMk/edit?tab=t.0#heading=h.8a9jnxl3wfhe
priority 動かん: https://www.oidaisdes.org/aria-notify-first-look.en/
W3C のスライド: https://docs.google.com/presentation/u/0/d/1aoleJpv04kdQ3kMYlkInU9W-bdnrdf_9-aHyMgBEXW0/htmlpresent (https://www.w3.org/2025/03/26-aria-minutes.html)

## 背景

- スクリーンリーダーやブラウザによって、挙動やアウトプットが大きく異なり、一貫性がなかった
  - DOM が複雑だと特に、通知するテキストの計算に一貫性がなかった
- assertive と polite の挙動が明確に定義されておらず、スクリーンリーダーによって動作が異なっていた
- Live region や「視覚的な変化を通知する」という前提があるので、DOM に結びついていなければならないが、[React Aria の LiveAnnouncer](https://github.com/adobe/react-spectrum/blob/main/packages/%40react-aria/live-announcer/src/LiveAnnouncer.tsx)のようなハック的な実装が行われてしまうことがある
  - https://zenn.dev/mehm8128/articles/adv2024-react-aria-button#ispending%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6
  - これは視覚的な役割を果たさないため、機能実装の修正時に追従が忘れられたりいつの間にか壊れたりしがち

## 用例

- RTE で文字をハイライトしたり太くしたりするみたいなショートカットキーを押したとき
- メールの送信に失敗したとき

## 解決策

- 命令的なので DOM から計算する必要がなく、通知内容に一貫性が出る

## `priority`

全通知キュー：高優先度キュー＋低優先度キュー

- `high`: 高優先度通知キューの末尾に追加
- `normal`: 全通知キュー（低優先度キュー）の末尾に追加

fallback として`aria-live`が使われる

## iframe

後でちゃんと見る

## 未解決問題

- AT ユーザーのみ弾くために aria notify を（たくさんアナウンスさせるなどして）悪用する可能性がある
  - その対策として、[UserActivation API](https://developer.mozilla.org/ja/docs/Web/API/Navigator/userActivation) を利用して、ユーザーがアクティブにしたウィンドウでのみに制限することができる？
  - これは開発者が行うもの？それとも Aria Notify 側で行うもの？
  - `actions`って書いてあるけど、UserActivation API はアクション単位ではなくてウィンドウ単位でしか検知できない

## 今後

### 点字と音声マークアップ

- `braille`オプションで点字用のテキストも指定できるようにする（`aria-braillelabel`みたいな）
- `SSML`オプションで、読み上げ方をより細かく指定できるようにする

### interrupt

後から ariaNotify されたテキストの割り込み方

大体以下のようになるが、priority に応じて微妙に挙動が変わる
正確な動作は explainer の step1 ～ 3 を参照
例えば`priority: high`を読み上げ中に`priority: normal`を`interrupt: all`で`ariaNotify`しても、`priority: high`が一通り読み上げ終わるまで読み上げられないはず

- none: デフォルト。priority に応じて、保留中のキューに追加される
- all: 読み上げ中テキストを無音にして、保留中キューも全て削除し、自分のテキストを読み上げさせる
- pending: 読み上げ中テキストはそのまま読み上げさせるが、保留中キューは全て削除し、読み上げ中テキストが読み終わり次第、自分のテキストを読み上げさせる

### type

- `type`を自分で定義して、コンテキストを提供できる。例えば「このアナウンスはタスクの進行状況に関するものである」とか「このアナウンスはアクションの完了ステータスを表すものである」とか
- 汎用的な`type`を事前に定義しておくべきかはまだ議論中
- `type`でフィルターしたり、この`type`は`priority: high`のもののみ通知する、のような設定をスクリーンリーダーでできるらしい？
- 最初に書かれてる点字とか音とかは何の話？

next: PR 読んでみる
