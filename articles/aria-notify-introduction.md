---
title: "命令的な ARIA ライブリージョン：ARIA Notifyの紹介"
emoji: "📢"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "a11y", "aria", "waiaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今回は、Edge では 136、Chrome では 140 から導入されている ARIA Notify について紹介します。

https://blogs.windows.com/msedgedev/2025/05/05/creating-a-more-accessible-web-with-aria-notify/

## ARIA Notify とは

ARIA Notify とは、既存の ARIA ライブリージョンにおける問題点を基に検討されている、新しい API です。`document.ariaNotify()`のように命令的に呼び出すことで、スクリーンリーダーや点字ディスプレイなどの支援技術に情報を伝えることができます。
ただし、既存のライブリージョンを完全に置き換えるものではありません。本来の目的で利用されているライブリージョンはそのままで良く、意図しない用いられ方をしてしまっている部分で、より正確に支援技術に情報を通知するための API となっています。
現在は仕様の議論段階で、v1 が Edge では 136 、Chrome では 140 から導入されています。

以下が explainer で、本記事ではこれを上から順に追っていきます。

https://github.com/MicrosoftEdge/MSEdgeExplainers/blob/main/Accessibility/AriaNotify/explainer.md

W3C のスライド: https://docs.google.com/presentation/u/0/d/1aoleJpv04kdQ3kMYlkInU9W-bdnrdf_9-aHyMgBEXW0/htmlpresent (https://www.w3.org/2025/03/26-aria-minutes.html)
aria の PR: https://github.com/w3c/aria/pull/2577
aria の PR2？: https://github.com/w3c/aria/pull/2211
aria の issue: https://github.com/w3c/aria/issues?q=is%3Aissue%20state%3Aopen%20%20%22ariaNotify%22%20in%3Atitle
design-reviews: https://github.com/w3ctag/design-reviews/issues/1075
discussion: https://github.com/w3c/aria/discussions/1958
Design Doc: https://docs.google.com/document/d/1tFT-4_sDvgnZoS8AYEcQquXzqAYaoB53DBH0C2T5rMk/edit?tab=t.0#heading=h.8a9jnxl3wfhe
Chrome Intent to Ship: https://groups.google.com/a/chromium.org/g/blink-dev/c/QCtWzIPgcCY/m/RSuXobocDAAJ

## 背景

背景として、ライブリージョンの問題点が挙げられています。

- スクリーンリーダーやブラウザによって、挙動やアウトプットが大きく異なり、一貫性がなかった
  - そもそも DOM が更新されたときに、それがちゃんとブラウザからスクリーンリーダーに伝われるかどうかなど
  - DOM が複雑だと、通知するテキストの計算方法に一貫性がなかったり
- `aria-live` の `assertive` と `polite` の挙動が明確に定義されておらず、スクリーンリーダーによって動作が異なっていた
- ライブリージョンには「視覚的な変化を通知する」という前提があるので DOM に結びついていなければならないが、DOM と結びつかない命令的な通知をしたい場合に、ハック的な実装が行われてしまうことがある
  - これは視覚的な役割を果たさないため、機能実装の修正時に追従が忘れられたり、いつの間にか壊れたりしがちとのこと
  - 例えば、[Button について - React Aria の実装読むぞ](https://zenn.dev/mehm8128/articles/adv2024-react-aria-button#ispending%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6) で紹介したような、[React Aria の LiveAnnouncer](https://github.com/adobe/react-spectrum/blob/main/packages/%40react-aria/live-announcer/src/LiveAnnouncer.tsx) のような実装のことを表しているのだと思います

## 用例

- リッチテキストエディターで文字をハイライトしたり太くしたりするようなショートカットキーを押したときに、「選択した文字をハイライトしました」や「選択した文字を太くしました」のような通知を流すとき
- メールの送信に少し時間がかかった後、送信に失敗したことを通知するとき
- スプレッドシートで、セルの編集時に副作用として別のセルの値が変更されたとき

## 解決策

- 命令的なので DOM から計算する必要がなく、通知内容に一貫性が出る

## `priority`

全通知キュー：高優先度キュー＋低優先度キュー

- `high`: 高優先度通知キューの末尾に追加
- `normal`: 全通知キュー（低優先度キュー）の末尾に追加

fallback として`aria-live`が使われる

## iframe

iframe を使う親コンテンツは、iframe 内の document に対して`ariaNotify`することはできない
しかし、iframe 内の document が自分自身のコンテンツに対して`ariaNotify`することができる
それを親コンテンツは iframe の`allow`属性や`Permissions-Policy`などで禁止することができる（例えば iframe の`ariaNotify`がうるさい場合など？）
どういう用途で使うのか、Permissions-Policy は誰が何のためにつかえるのかなど、もう少し調べる余地がある

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
