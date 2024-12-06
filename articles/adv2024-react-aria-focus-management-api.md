---
title: "【番外編】Focus Management APIについて - React Ariaの実装読むぞ"
emoji: "🔍"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

:::message
この記事は [React Aria の実装読むぞ - Qiita Advent Calendar 2024](https://qiita.com/advent-calendar/2024/react-aria) の 11 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は Focus Management API について書いていきます。初めての番外編です。枠稼ぎではないです。

## Focus Management API とは

Focus Management API とは、こちらの React の RFC で提案されている API です。提案者の方が React Spectrum のメンテナーということと、React Aria で同様の API が実装されていることから、今回紹介することにしました。

https://github.com/reactjs/rfcs/pull/109

RFC 自体はここから見ることができます（リンク先飛ぶときれいに表示されてる状態で見れます）。

https://github.com/devongovett/rfcs-1/blob/patch-1/text/2019-focus-management.md

簡単に言うと、`FocusScope`コンポーネントと`FocusManager`という API を `react-dom` にビルトインで導入したいという提案です。
[React の `createPortal`](https://ja.react.dev/reference/react-dom/createPortal)が抱えている問題の改善や、その他フォーカス制御をいい感じにしたいというのが主な目的です。

まだ`react-dom`には入っていないのですが、前述のように`FocusScope`も`FocusManager`も React Aria には導入済みです。

https://react-spectrum.adobe.com/react-aria/FocusScope.html

## 現状の辛さ

`Challenges`のセクションに、現状フォーカス制御をすることの辛さが語られています。1 つずつ見ていきます。
基本的には要約していくので、補足説明的なところには分かりやすいように「mehm8128: 」というマークを振っておきます。

### Refs everywhere

宣言的な React で、命令的な処理を書くことになる ref を使うのはエスケープハッチとされています。特にフォーカス制御などで ref を使わざるを得ないときがありますが、ref は React っぽくないのであまり使いたくないとのことです。

### Global focus state

一度に 1 つの要素にしかフォーカスできないので、ある要素にフォーカスしたら元々フォーカスされていた要素からはフォーカスが外れてしまいます。また現状、前にどの要素がフォーカスを持っていたかを記憶しておくには手動で管理しておくしかありません。

### Arrow key navigation

リストとかグリッドといった UI パターンでは、一度 Tab キーでフォーカスしたらその後の、その UI の中でのフォーカス移動は矢印キーで移動したいです。なぜなら、見たいわけではないグリッドにフォーカスしたときに、そこから抜け出して次のコンテンツに進むために何回も Tab キーを押さなければならないからです。
そのために [Roving tab index パターン](https://www.w3.org/WAI/ARIA/apg/practices/keyboard-interface/#kbd_roving_tabindex)（mehm8128: リンク切れてたので参考になりそうなものに変えました。TODO: このページ全部ちゃんと読む）が上記の動作を実現する 1 つの手段ではあるけど、一度フォーカス外れてまた戻ってきたときに、前にあった箇所にフォーカスするにはそれを記憶しておかないといけません。

### Focus containment

Focus containment とは、ダイアログやその他ポップアップでフォーカスが外に出てしまわないようにするためのものです。ダイアログで最後の要素にフォーカスしている状態で Tab キーを押したらダイアログ内の一番上にフォーカスが戻るようにしたりします。これは現状、手動で命令的にフォーカスを制御しなければ実装できません。

### Restoring focus

先ほどのフォーカス位置の記憶に関連して、ダイアログが閉じたときに、ダイアログを開く前にフォーカスしていた要素（mehm8128: ダイアログのトリガーボタンなど）に再度フォーカスしたいです。

### Portals

[React portals](https://ja.react.dev/reference/react-dom/createPortal) は実際の DOM の順番が React tree（mehm8128: ソースコード上の DOM 及びコンポーネントのツリー）と異なるので、フォーカス順についても開発者の意図しない動作をしてしまうことがあります。
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

この場合、フォーカス順が`input 1`->`input2`->`input3`ではなくて、`input 1`->`input3`->`input2`となります（mehm8128: RFC に書かれている`Users`とはおそらく、Web サービスのユーザーではなくて React のユーザーです）。これは分かりづらいので、React tree の順にフォーカスされるようにしてほしいとのことです。実はイベントバブリングは React tree の通りに行われるらしいです（mehm8128: `input 2`で発生したイベントはその親要素である`div`タグにバブリングされる、ということです）。

TODO: In addition, portals make implementing focus containment in user space difficult の段落が分からなかった

## 解決方法

上記の辛さを踏まえて、`Detailed design` のセクションでは今回の提案でどのように問題を解決していくかが述べられています。

### Definitions

用語の定義がされています。[Radio と Checkbox について - React Aria の実装読むぞ](https://zenn.dev/mehm8128/articles/adv2024-react-aria-radio-and-checkbox)でも出てきましたが一応確認します。

focusable: デフォルトでフォーカス可能な`input`や`button`要素に加えて、`tabindex`属性がついている要素
tabbable: デフォルトでフォーカス可能な`input`や`button`要素に加えて、値が 0 以上の`tabindex`属性がついている要素（mehm8128: つまり、Tab キーでフォーカスできないマイナスの`tabindex`を持つ要素は含まない）

### The focus tree

React ルートに暗黙の`FocusScope`を用意しておき、`FocusScope`はその中にあるフォーカス可能要素で順序付きリストを作成します。Tab キーを押したときに、この順序通りにフォーカスが移動していきます。
各`FocusScope`はその中で最後にフォーカスされた要素を記憶しておき、他の`FocusScope`からフォーカスが移動してきたときにその位置にフォーカスを戻すことができます。また、現在フォーカスされている要素を持っている`FocusScope`がアンマウントした場合、最後にフォーカスを持っていた要素にフォーカスが移動します。
TODO: When a FocusScope which contains the currently focused element unmounts 辺りの話だけど、よく分かってない。全部の FocusScope の中で最後にフォーカスを持っていた要素にフォーカスできる？

また、Focus containment にも用いることができます。`contain`prop を渡した場合、`FocusScope`内のフォーカス可能要素の中でフォーカスがループします。そして、`autoFocus`prop を渡すと`FocusScope`内で最初のフォーカス可能要素に自動でフォーカスします。

### FocusManager

矢印キーを押したときのフォーカス移動など、Programmatically にフォーカスを移動するための API で、next, previous, first, last の focusable 及び tabbable な要素への移動をサポートしています。特定の要素へのフォーカスは引き続き React の`ref`を使うのがよさそうとのことです。

## その他

残りのセクションでは、具体的な実装方針や使用例がソースコードとともに紹介されています。

## まとめ

React Aria に実装されている`Focus Scope`の実装を読もうと思ったのですが、長くなりそうなので別の回で書きます。

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、 Toast についての記事です。お楽しみにー
