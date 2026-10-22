---
title: "Web Sustainability Guidelinesを読む会 #9"
emoji: "🥝"
type: "idea"
topics: ["W3C", "sustainability", "ガイドライン"]
published: true
publication_name: "wsgj"
---

これは[Web Sustainability Guidelines (WSG)](https://www.w3.org/TR/web-sustainability-guidelines/)を読む会の第9回のレポートです。Web Sustainability Guidelinesの概要については、[Web Sustainability Guidelinesとは](https://zenn.dev/wsgj/articles/1f79582f108e02)で説明しています。

## #9で読んだ範囲

今回も引き続き、Filtersの「AI」で絞り込んだガイドラインを読みました。

- **5.23 Repair, reuse, refurbish, recycle or reduce e-waste**
  - [Success Criterion: E-waste management](https://www.w3.org/TR/web-sustainability-guidelines/#e-waste-management)
  - ハードウェアや電子廃棄物の修理・再利用・再整備・責任あるリサイクルを、ユーザーによる修理の支援や認証されたサービスを通じて優先すること
- **5.26 Create a business continuity and disaster recovery plan**
  - [Success Criterion: Plan of action](https://www.w3.org/TR/web-sustainability-guidelines/#plan-of-action)
  - テスト済みのインシデント対応計画を維持し、リスクや復旧のプロセスについてユーザーに透明性高く伝えること

## 修理する権利とハードウェアの設計

5.23のリソースには、[米国の多くの州で「修理する権利（Right to Repair）」が法制化されているという記事](http://404media.co/all-50-states-have-now-introduced-right-to-repair-legislation/)がありました。

- 関連してDellの製品について、[USB-Cポートがはんだ付けではなくネジ止めで、壊れたら交換できるようになっているという投稿](https://www.theverge.com/2025/1/6/24335562/dells-new-usb-c-ports-use-screws-instead-of-solder-so-you-can-fix-them-yourself)が共有されました。一方でAppleは、ユーザー自身で直せない印象が強い
- 修理する権利は「ユーザーが自分で直す権利」として語られることが多いが、法律になる背景には、独占によってサードパーティの修理業者のビジネスが成り立たなくなるという、経済的な権利の問題があるのではないかという話もありました
- 修理することが前提になっていない製品が増えると、金銭面だけでなく、修理を通して学ぶ機会や経験も失われていく
  - [太陽を創った少年　―僕はガレージの物理学者―](https://www.hayakawa-online.co.jp/shop/g/g0000612466/)という書籍が紹介されました

また、5.23のリソースは3R（Reduce、Reuse、Recycle）に関するものが中心で、電子機器メーカー自身の取り組み事例は意外と少ないという話も出ました。

## 災害復旧計画とIBMのテンプレート

5.26のリソースにあった[IBMのディザスタリカバリ計画のドキュメント](https://www.ibm.com/docs/en/i/7.6.0?topic=system-example-disaster-recovery-plan)は、とても具体的で驚きました。「計画を立てましょう」という理念だけではなく、次のような項目を埋める形式のチェックリストのテンプレートが用意されています。

- 通信用モデムの送付先
- 現地に向かう担当者の旅費を前払いで支給する手続き
- 本拠地が被災したときに業務を引き継ぐ予備の拠点（バックアップ用の施設）で、何キロワットの電力が必要かを書く欄

リソースには[1.5.3 Best practices](https://www.w3.org/TR/web-sustainability-guidelines/#x1-5-3-best-practices)にあるようなAWSやAzureの記事が多い中で、IBMのリソースもよく出てくるのが印象的でした。他には[AWSのディザスタリカバリに関するドキュメント](https://docs.aws.amazon.com/ja_jp/whitepapers/latest/disaster-recovery-workloads-on-aws/what-is-a-disaster.html)もあり、RTO、RPOといった用語や責任共有モデルが説明されていました。

## クラウドの冗長化と地政学リスク

AWSのドキュメントにあった「ディザスタリカバリを計画するときは災害の性質と地理的な影響の両方を考える」という話から、インフラが物理的に依存している「地理」へと話が広がりました。

- 海底ケーブルは、日本に陸揚げされるものが多いなど、国や地域によって事情が異なる。デジタルシルクロード構想のような動きもあり、データセンターや通信の要衝となる地域の地政学的なリスクも、災害リスクの一つと言えそう
- 先端半導体の製造は10ステップほどあり、それを全て一つの国でまかなえる国はない。オランダの露光装置メーカーASMLや、日本の素材メーカーなど、世界中の企業が絡み合っている。この相互依存は戦争が起こりにくくなるという見方がある一方、どこかが止まると続けられなくなる脆さでもあり、意見が分かれました
  - [Why NO Country Can Build AI Chips Alone: TSMC, Japan, and the Dutch - YouTube](https://www.youtube.com/watch?v=jPiI6SSfn30)

## 「AI」タグはなぜ付いているのか

WSGはAPIが公開されており、各達成基準に付いているタグを確認できます。今回読んだ5.23と5.26では、一部の達成基準だけに「AI」タグが付いていました。

- 5.23の他の達成基準は物理的なプロダクトの話で、AIタグが付いていないのに、特定の一つだけが対象になっている理由がわからない
- 5.26は"Plan of action"だけにAIタグがついており、"Audience awareness"には付いていない
- AIによるサイバー攻撃などを、自然災害と同じく予測しにくい「災害」と捉える考え方はあるが、ガイドラインはそれより前から作られているので、それが理由とは限らない

結局理由は分からないため、今後issueを立てるなどして質問・問題提起してみることにしました。

## 書き起こし作成に使用した電力

前回に引き続き、書き起こしに使った電力も（maihaさんが）[CodeCarbon](https://codecarbon.io/)で計測しました。Apple M4搭載のMac上で、ローカルのWhisper（`mlx-community/whisper-large-v3-turbo`）を使っています。

| 対象トラック | 音声の長さ      | 処理時間   | CPU（実測） | GPU（実測） | メモリ（推定） | 合計        |
| ------------ | --------------- | ---------- | ----------- | ----------- | -------------- | ----------- |
| 発言者1      | 約1時間29分     | 13.6分     | 0.14 Wh     | 0.33 Wh     | 0.45 Wh        | 0.92 Wh     |
| 発言者2      | 約1時間29分     | 16.8分     | 0.20 Wh     | 0.43 Wh     | 0.54 Wh        | 1.17 Wh     |
| **計**       | **約2時間59分** | **30.4分** | **0.34 Wh** | **0.76 Wh** | **0.99 Wh**    | **2.09 Wh** |

**チップ（CPU＋GPU）の実測値は 1.10 Wh** でした。10WのLED電球を7分ほど点けたのと同じくらいです。メモリ分を含めた合計は約2.1 Whで、CO2換算では約1.0 gです（日本の電源構成の排出係数 約485 g/kWhで換算）。

## 次回予告

AIに関連するガイドライン・達成基準は全て読み終えました。次回はこれまでの振り返りができるよう、スプレッドシートなどで項目を一覧にしたものを用意して、全体を眺める予定です。

このコミュニティは誰でも自由に参加できます。興味のある方は以下のリンクからご参加ください。

https://discord.gg/QXemS58mwV
