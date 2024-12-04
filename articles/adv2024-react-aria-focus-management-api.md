---
title: "【番外編】Focus Management APIについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は Focus Management API について書いていきます。初めての番外編です。枠稼ぎではないです。

## Focus Management API とは

Focus Management API とは、こちらの React の RFC で提案されている API です。提案者が React Spectrum のメンテナーということと、後述する理由により、今回紹介することにしました。

https://github.com/reactjs/rfcs/pull/109

RFC 自体はここから見ることができます。

https://github.com/devongovett/rfcs-1/blob/patch-1/text/2019-focus-management.md

簡単に言うと、`FocusScope`コンポーネントと`FocusManager`という API を `react-dom` にビルトインで導入したいという提案です。
[React の `createPortal`](https://ja.react.dev/reference/react-dom/createPortal)が抱えている問題の改善や、その他フォーカス制御をいい感じにしたいというのが主な目的です。

まだ`react-dom`には入っていないのですが、実は`FocusScope`も`FocusManager`も React Aria には導入済みです。

https://react-spectrum.adobe.com/react-aria/FocusScope.html

## 現状の辛さ

`Challenges`のセクションに、現状フォーカス制御を実装することの辛さが語られています。1 つずつ見ていきます。
基本的には要約していくので、補足説明的なところには分かりやすいように「mehm8128: 」というマークを振っておきます。

### Refs everywhere

宣言的な React で、命令的な処理を書くことになる ref を使うのはエスケープハッチとされています。特にフォーカス制御などで ref を使わざるを得ないときがありますが、ref は React っぽくない（un-reacty）ので使いたくないとのことです。

### Global focus state

一度に 1 つの要素にしかフォーカスできないので、ある要素にフォーカスしたら元々フォーカスされていた要素からはフォーカスが外れてしまいます。また現状、前にどの要素がフォーカスを持っていたかを記憶しておく方法には手動で管理しておくしかありません。

### Arrow key navigation

リストとかグリッドといった UI パターン（mehm8128: 数日後の記事で出てきます）では、一度 Tab キーでフォーカスしたらその後の その UI の中でのフォーカス移動は矢印キーで移動したいです。 なぜなら、見たいわけではないグリッドにフォーカスしたときに、そこから抜け出して次のコンテンツに進むために何回も Tab キーを押さなければならないからです。
そのために [roving tab index パターン](https://www.w3.org/WAI/ARIA/apg/patterns/radio/examples/radio/)（mehm8128: リンク切れてたので参考になりそうなものに変えました。TODO: ラジオボタンの記事貼って説明）が上記の動作を実現する 1 つの手段ではあるけど、一度フォーカス外れてまた戻ってきたときに、前にあった箇所にフォーカスするにはそれを記憶しておかないといけません。

### Focus containment

Focus containment とは、ダイアログやその他ポップアップでフォーカスが外に出てしまわないようにするためのものです。ダイアログで最後の要素にフォーカスしている状態で Tab キーを押したらダイアログ内の一番上にフォーカスが戻るようにしたりします。これは現状、手動で命令的にフォーカスを制御しなければ実装できません。

### Restoring focus

先ほどのフォーカス位置の記憶に関連して、ダイアログが閉じたときに、ダイアログを開く前にフォーカスしていた要素（mehm8128: つまり、ほとんどの場合ダイアログをトリガーしたボタン）に再度フォーカスしたいです。

### Portals

React portals は実際の DOM の順番が、React tree（mehm8128: 書いたソースコードの DOM 構造みたいな？）と異なるのでフォーカス順についても開発者の意図しない動作をしてしまうことがあります。
RFC に書かれているソースコードの例をそのまま取ってきます。

```tsx
function App() {
  return (
    <div>
      <input placeholder="input 1" />
      <Portal>
        <input placeholder="input 2" />
      </Portal>
      <input placeholder="input 3" />
    </div>
  );
}
```

この場合、フォーカス順が`input 1`->`input2`->`input3`ではなくて、`input 1`->`input3`->`input2`となります（mehm8128: RFC に書かれている`Users`とはおそらく、Web サイトのユーザーではなくて React のユーザーです）。これは分かりづらいなので、React tree の順にフォーカスされるようにしてほしいとのことです。実はイベントバブリングは React tree の通りに行われるらしいです（mehm8128: `input 2`で発生したイベントは先ほどのソースコードの`div`タグにバブリングされる、ということです）。

TODO: In addition, portals make implementing focus containment in user space difficult の段落が分からん

## 解決へ

上記の辛さを踏まえて、`Detailed design` のセクションでは今回の提案でどのように問題を解決していくかが述べられています。

### Definitions

focusable: tabindex がマイナスのものを含む
tabbable: tab で到達できるものなので、tabindex>=0 のみ

### The focus tree

ルートに暗黙的な`FocusScope`を用意しておく
各 focus scope 内で最後にフォーカスされた要素を記憶する
ダイアログ閉じたときにトリガーボタンに戻れるのは、ルートの`FocusScope`のスコープ内にどちらも入っているから

### FocusManager

特定の要素にフォーカスするのは、引き続き ref を使えば OK

React に実装する必要性が薄そう
portal のやつも単体でできそうだし

## その他

残りの部分は例とかが書いてありますみたいな

## React Aria の`FocusScope`

ほぼ同じだねとか色んなコンポーネントで使われてるとか

## まとめ

明日は の話です。お楽しみにー
