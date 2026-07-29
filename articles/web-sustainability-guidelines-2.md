---
title: "Web Sustainability Guidelinesを読む会 #2"
emoji: "🍍"
type: "idea"
topics: ["W3C", "sustainability", "ガイドライン"]
published: false
publication_name: "wsgj"
---

これは第二回 [Web Sustainability Guidelines](https://www.w3.org/TR/web-sustainability-guidelines/)（参考: [Google翻訳版](https://www-w3-org.translate.goog/TR/web-sustainability-guidelines/?_x_tr_sl=auto&_x_tr_tl=ja&_x_tr_hl=ja)）を読む会のレポートです。Web Sustainability Guidelinesの概要については、[Web Sustainability Guidelinesとは](https://zenn.dev/wsgj/articles/1f79582f108e02)で説明しています。

Web Sustainability Guidelinesを読む会の第二回目は、7月下旬某日の夜にオンラインで集まり、事前に読んできた内容について意見や感想、疑問をカジュアルに共有する形で実施しました。参加者としてウェブフロントエンドとアクセシビリティのエキスパート、計4名が集まりました。

## 読んだ範囲

前回はIntroductionなどガイドライン冒頭部分を読み進めましたが、今回はガイドラインに付随しているFiltersを触りながら、気になった箇所について意見交換しました。

FiltersにはPeople/Planet/Prosperityの3つのPに加え、前回のIntroductionに登場したTimeframeやStandards、その他Considerations、Categoriesがあります。

![FiltersのPeopleからStandardsまでのスクショ](/images/wsgj/filters1.png =200x)

![FiltersのConsiderationsとCategoriesのスクショ](/images/wsgj/filters2.png =200x)

[Considerationsの章](https://www.w3.org/TR/web-sustainability-guidelines/#considerations)は前回触れなかったのですが、W3Cのmissionに含まれている[Accessibility](https://www.w3.org/mission/accessibility/)、[Privacy](https://www.w3.org/mission/privacy/)、[Security](https://www.w3.org/mission/security/)という3つを基にして記載されているようです。

## Filtersで気になったところ

### 5.15 Share economic benefits

PeopleへのImpactがHighのガイドラインは、1つを除いてアクセシビリティのカテゴリーに属しているという発見がありました。
そしてその1つの例外が、[5.15 Share economic benefits](https://www.w3.org/TR/web-sustainability-guidelines/#x5-15-share-economic-benefits)です。
GovernanceとSocial Equityというカテゴリーに属しており、労働に対する賃金やインセンティブ、福利厚生が必要だというガイドラインとなっています。

日本人も翻訳に参加した[Heydon/principles-of-web-accessibility](https://github.com/Heydon/principles-of-web-accessibility/blob/main/README_ja.md)の"Get paid"（日本語: 報酬を得よう）のセクションを思い出しました。

### AIの話

[5.19 Establish responsible practices around AI and emerging or disruptive technologies](https://www.w3.org/TR/web-sustainability-guidelines/#x5-19-establish-responsible-practices-around-ai-and-emerging-or-disruptive-technologies)という、AIに特化したガイドラインがあるという発見もありました。
環境負荷（managing environmental impacts）やユーザー・エージェントの制御の尊重（respecting user-agent controls）、耐量子暗号（using post-quantum encryption）など、AIに関して幅広いカテゴリーに触れているガイドラインとなっており、着目しました。

### Prosperityの解釈

Sustainabilityの文脈ではPeople/Planet/**Profit**が3つのPとしてよく使われ、WSGでは最後のProfitを**Prosperity**に置き換えて使っています。しかし、実際に達成基準を眺めてみると「PeopleやPlanetと重複する」ような項目が多く、Prosperity固有の内容が見えづらいという議論がありました。

- ProsperityへのImpactがHighとして分類されているデータ処理の話（4.9）はPlanetに近く、賃金や経済的便益の話（5.15）はPeopleに近い、など
- 「利益と環境・人権のトレードオフ」（多少の犠牲を払ってでも技術発展・繁栄を優先する、といった判断）を正当化するような記述があるのではと予想したが、実際にはそういった項目は見当たらなかった

この議論の流れでProsperityについてもう少し理解を深めたいという話になり、後半はProsperityへのImpactがHighなものをフィルタリングし、順番に見ていくことにしました。

## [4.9 Assess the impact and requirements of data processing](https://www.w3.org/TR/web-sustainability-guidelines/#x4-9-assess-the-impact-and-requirements-of-data-processing)

Prosperity: Highは4.9、5.15、5.23の3つのガイドラインが該当しますが、今回は4.9のみを読みました。

「4.9 Assess the impact and requirements of data processing」は5つの達成基準で構成されています。
5つのうち、1つ目の「Success Criterion: Carbon shifting」だけ他の4つと比べて毛色が異なっており、理解に時間を費やしました。

Carbon Shiftingという言葉に聞き馴染みがなかったのですが、途中で登場している「Grid Carbon Intensity（グリッド炭素強度）」というキーワードを調べることで理解が進みました。電力網（グリッド）から供給される電気1kWh（キロワット時）あたりに排出される二酸化炭素の量（gCO2eq/kWh）のことで、発電に使われるエネルギー源の比率（再生可能エネルギーの割合など）によって時間帯や地域ごとに変動します。Carbon Shiftingは、このグリッド炭素強度データに基づいて、炭素強度が低いタイミングにタスクをスケジューリング・バッチ実行したり、炭素強度の低いリージョンにワークロードをずらしたりする取り組みとのことです。

これに関連して、AWSやGoogle Cloudなどのクラウドサービスで、炭素強度が低い時間帯を確認できるような機能・APIはないのかという疑問が挙がりました。
調べてみると、ユーザーがAWSの使用によって排出している二酸化炭素量の監視ができる機能があるようです。詳細は以下のページや[What is AWS Sustainability? - AWS Sustainability](https://docs.aws.amazon.com/sustainability/latest/userguide/what-is-sustainability.html)をご覧ください。

https://aws.amazon.com/jp/blogs/aws-cloud-financial-management/updated-carbon-methodology-for-the-aws-customer-carbon-footprint-tool/

残り4つは「HTTP/2など高パフォーマンスのプロトコルを使おう」「不要なデータ処理を避けてください」など、Sustainabilityの意識がなくとも、パフォーマンスやコストなどの観点から一般的なエンジニアが考慮するであろう達成基準となっていました。

アクセシビリティに関連するものだと[3.6 Ensure code follows good semantic practices](https://www.w3.org/TR/web-sustainability-guidelines/#ensure-code-follows-good-semantic-practices)なども、特別にSustainabilityへの意識がなくても考慮するような内容だという気づきもありました。

## 次回予告

次回以降は、Prosperity: Highの残りのガイドラインや、Medium/Lowの達成基準を読み進めていく予定です。
