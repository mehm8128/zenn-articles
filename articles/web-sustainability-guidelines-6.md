---
title: "Web Sustainability Guidelinesを読む会 #6"
emoji: "🍓"
type: "idea"
topics: ["W3C", "sustainability", "ガイドライン"]
published: true
publication_name: "wsgj"
---

これは [Web Sustainability Guidelines (WSG)](https://www.w3.org/TR/web-sustainability-guidelines/) を読む会の第6回のレポートです。Web Sustainability Guidelinesの概要については、[Web Sustainability Guidelinesとは](https://zenn.dev/wsgj/articles/1f79582f108e02) で説明しています。

前回・前々回に引き続き、今回もFiltersの「AI」でガイドラインを絞り込みながら読み進めました。

## 読んだ範囲

- 4.5 Avoid maintaining unnecessary virtualized environments or containers
  - Success Criterion: Unused environment
- 4.6 Use automation wisely
  - Success Criterion: Task automation
  - Success Criterion: Necessary tasks
  - Success Criterion: Suspicious activity management
- 4.9 Assess the impact and requirements of data processing
  - Success Criterion: Efficient location
- 5.1 Have an ethical and sustainable product strategy

## Amazon EC2 Spot Instance

[4.5 Avoid maintaining unnecessary virtualized environments or containers](https://www.w3.org/TR/web-sustainability-guidelines/#x4-5-avoid-maintaining-unnecessary-virtualized-environments-or-containers) は、使われていない・重複した仮想／物理環境（コンテナやVM）を無効化・削除して、アクティブな環境の数を減らそうというガイドラインです。

> **Success Criterion: Unused environment**  
> Reduce the number of active environments by deactivating, offlining, or removing unused or redundant virtual and physical environments (e.g., containers and virtual machines) wherever this can be done without reducing required security, isolation, or compliance guarantees. Optimize active environments by tailoring resource allocations to actual workload demand, sharing common services, and eliminating duplicate tasks while maintaining necessary isolation.

Resourcesとして紹介されているAWSの「[Optimize your container workloads for sustainability](https://aws.amazon.com/jp/blogs/containers/optimize-your-container-workloads-for-sustainability/)」に [Amazon EC2 Spot Instance](https://aws.amazon.com/jp/ec2/spot/) を使おうという話があり、「Spot Instanceって結局何？」という話でしばらく盛り上がりました。

- オンデマンドで確保されていない、余っている計算リソースを安く使わせてもらう仕組みで、他の優先度の高いタスクが来ると処理を止められることがある
- 止まっても困らないバッチ処理などを投げると、AWS全体で見たときの無駄（つけっぱなしの空きリソース）を減らせる

個人的にこの話は、電気を夜間や早朝などに使うことで電気代を節約したり電気の有効活用ができるという「ピークシフト」と関連がありそうと感じました。
最近ちょうど東京ガスで、ピークシフトのキャンペーンが行われているようです。
[IGNITURE スマートアクション（節電／ピークシフトプログラム）｜東京ガス](https://home.tokyo-gas.co.jp/gas_power/plan/power/smartaction/index.html)

関連して、ResourcesにAWSの情報が多いように感じるので、Cloudflareなど他のクラウドサービスの情報も見てみたい・載せてほしいという話もありました。

## クロール最適化とAIクローラー問題

[4.6 Use automation wisely](https://www.w3.org/TR/web-sustainability-guidelines/#x4-6-use-automation-wisely) には、Task automation、Necessary tasks、Suspicious activity managementの3つのSuccess Criterionがあります。

### Task automation / Necessary tasks

タスクとインフラを効率的に自動化・スケールし、必要なときだけプロセスを実行するという内容です。リソースには「クロール最適化でサイトのサステナビリティが上がる」という記事があり、robots.txtで不要なクロールを避けることがトラフィック削減＝コスト削減＝サステナビリティに繋がるという話が紹介されていました。
参加メンバーからは「Vercelでページネーションのクエリパラメータ違いのURLを大量にクロールされて課金が跳ねた」といった実体験も共有され、日常のインフラコスト削減の工夫がそのままサステナビリティに直結するという再確認が今回もありました。

### Suspicious activity management

今回一番盛り上がったのがこのSuccess Criterionです。望ましくない第三者クローラー・ボット・スクレーパーを制限しつつ、正当な検索エンジンや有用なクローラーはアクセスできる状態を保つ、という内容で、その中に次のような一文があります。

> Consider that some scrapers may be used for beneficial purposes or to inform or train Large Language Models (LLMs).

「beneficial purposes **or** to inform or train LLMs」という書き方になっており、「LLMの学習はbeneficial purposesに含まれるのか、それとも並列した別枠（＝あまり歓迎されていない扱い）なのか」という原文の解釈で議論になりました。わざわざ後半を書き添えているということは、後者寄りなのでは、という解釈で一致しました。

今までAIに直接関連しているといえる達成基準が少なかったのですが、この達成基準のリソースには、AI関連の話題がついにたくさん登場しました。

- [AI crawlers cause Wikimedia Commons bandwidth demands to surge 50% | TechCrunch](https://techcrunch.com/2025/04/02/ai-crawlers-cause-wikimedia-commons-bandwidth-demands-to-surge-50/)
  - もっとも高コストなトラフィックの65%がbotによるもので、しかも人気のないページ・アーカイブにも無差別にアクセスしてくるため、CDNのエッジキャッシュ的な最適化が効きづらいという記事
- [Go To Hellman: AI bots are destroying Open Access](https://go-to-hellman.blogspot.com/2025/03/ai-bots-are-destroying-open-access.html)
  - AIボット対策のためにブロックしたら善良なクローラーやWayback Machineまで巻き添えを食ったという話
- [I use Zip Bombs to Protect my Server](https://idiallo.com/blog/zipbomb-protection)
  - AIクローラーからのアクセスに対して、展開すると巨大になるZip爆弾を返すという過激な対策を紹介した記事

これまでAIカテゴリーの達成基準を読んでも「AIに限らずどの技術にも当てはまる話では？」という感想ばかりだったのですが、ここへ来てようやく「AIクローラーによる過剰な負荷」という、AI特有かつ実務に直結する具体的な問題意識に出会えました。一方で「AIが正しい方法でクロールするのであれば問題ない」といった、AI推進そのものを否定するのではなく共存を模索する意見も出ました。

## ダークデータと「消していいか誰も判断できないデータ」

[4.9 Assess the impact and requirements of data processing](https://www.w3.org/TR/web-sustainability-guidelines/#x4-9-assess-the-impact-and-requirements-of-data-processing) は、パフォーマンス・セキュリティ・制約のバランスを取りながら、カーボンアウェアなスケジューリングや効率的なプロトコル、シンプルな設計で不要な処理を減らそうというガイドラインです。Carbon shiftingは [第2回](https://zenn.dev/wsgj/articles/web-sustainability-guidelines-2#4.9-assess-the-impact-and-requirements-of-data-processing) に読んでいたため、今回はEfficient locationのみ読みました。

> **Success Criterion: Efficient location**  
> Perform data transformations, transfers, and processing between the layers of an application as close to the source as possible. This reduces unnecessary serialization overhead and avoids wasting resources.

Resourcesの中でIBMの「[ダークデータ](https://www.ibm.com/jp-ja/think/topics/dark-data)（組織が蓄積しているものの、ほとんど分析や意思決定に利用されないデータ）」という概念が紹介され、「消していいか誰も判断できず溜め込まれ続けるデータ」の話に共感が集まりました。ストレージを圧迫するだけでなく、不要なデータ処理によるエネルギー浪費にも繋がるという整理です。

このあたりで「4章はずっと『不要なものを使うな・必要なときだけ動かせ』という同じ話の繰り返しだ」という指摘があり、次回以降は4章を通しで読み返す会はやらず、5章に軸足を移していく方針を確認しました。

## アクセシビリティステートメントとサステナビリティ声明の共通点

[5.1 Have an ethical and sustainable product strategy](https://www.w3.org/TR/web-sustainability-guidelines/#x5-1-have-an-ethical-and-sustainable-product-strategy) は、倫理規範・製品ガイドライン・アクセシビリティ及び持続可能性に関する声明などの方針を策定・公開・維持し、バージョン管理を通じて透明性を確保することを求める達成基準です。

Resourcesには「アクセシビリティステートメントの書き方」に関する記事があり、「どのガイドラインに準拠しているか」「どんな支援技術・環境でテストしたか」「問題を報告する連絡先」などが書かれていない声明は形骸化しているという指摘が紹介されていました。

他の適当なサイトからコピペしないで、という記事も紹介されていました。
[Don’t just borrow your accessibility statement for European Accessibility Act from some random site – Bogdan on Digital Accessibility (A11y)](https://cerovac.com/a11y/2025/02/dont-just-borrow-your-accessibility-statement-for-european-accessibility-act-from-some-random-site/)

## まとめと次回のお知らせ

AIカテゴリーの達成基準は残り10個ほど（5.13、5.14、5.18、5.19、5.23、5.26など）となりました。次回もその続きを読み進めつつ、[CO2.js](https://www.thegreenwebfoundation.org/co2-js/)や [EcoGrader](https://ecograder.com/)など、前回や今回Resourceとして登場したツールを実際に動かしてみる回にできたらという話も出ています。

WSGを読む会に興味がある方は是非「読む会」のDiscordにご参加ください。

https://discord.com/invite/TEAYemhAc
