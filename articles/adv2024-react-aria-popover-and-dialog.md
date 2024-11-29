---
title: "PopoverとDialogについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は Popover と Dialog について書いていきます。

https://react-spectrum.adobe.com/react-aria/usePopover.html
https://react-spectrum.adobe.com/react-aria/useDialog.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx

```

## 主な a11y 考慮事項

https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/

- `dialog`role
- フォーカス制御
- モーダル化

## いくつかピックアップ

### フォーカス制御

ダイアログを開いたときと閉じたときのフォーカス制御について、APG に書いてあることを説明します。

#### 開いたときに最初にフォーカスする要素

基本的にダイアログ内の最初のフォーカス可能要素にフォーカスされますが、以下のような場合には例外となります。

- リスト、表、複数の段落など複雑なものの場合、先頭に`tabindex="-1"`な要素を追加してそこに最初にフォーカスする
- 最初のインタラクティブ要素にフォーカスするとダイアログがスクロールされてしまう場合は、`tabindex="-1"`な要素を追加してそこに最初にフォーカスする
- ダイアログ内に破壊的なアクションボタンが含まれている場合、最も破壊的でないボタンにフォーカスを設定する
- 情報を提供したり処理を続行したりするインタラクションボタンに限られている場合、「OK」など最もよく使われるボタンにフォーカスする

#### 閉じたときにフォーカスする要素

基本的にはダイアログを開くトリガーとなったボタンにフォーカスを戻しますが、以下の場合にはワークフロー上の別の要素にフォーカスするとよいです。

- トリガーボタンが消えたとき
- トリガーボタンをすぐに再度押す可能性がほとんどないときや、ダイアログ内のタスクを完了すると次のステップに進む場合

例えば grid で行を追加するボタンを押し、ダイアログで追加する行数を入力するような場合には、行を追加するボタンにフォーカスを戻すのではなく、追加された行の最初のセルにフォーカスするようにするとよいです。

### モーダル化

React Aria のポップオーバーで特徴的なのはこれです。
ダイアログがモーダルになっているのは自然なのですが、ポップオーバーでもモーダルなのはあまり見たことがありませんでした。ポップオーバーを開くとポップオーバー外の要素へのインタラクションができなくなり、背景のスクロールもできなくなります。
これは意図せずポップオーバーが閉じてしまったりポップオーバー外の要素にアクセスしてしまったりするのを防いでいます。

モーダルにするときは`aria-modal="true"`にするとよいのですが、Safari でフォーカス制御が上手くいかないことがあることから、React Aria では代わりに外側の要素に`aria-hidden="true"`をつけています。

https://github.com/adobe/react-spectrum/blob/10a43de887ffc28913c770a33573aebf3df786fc/packages/%40react-aria/dialog/src/useDialog.ts#L66-L70

### todo

overlays 以下を見て面白そうなコメントあれば

https://github.com/adobe/react-spectrum/issues/4130
https://github.com/adobe/react-spectrum/issues/4922
原因同じらしい

`usePreventScroll`の`overscroll-behavior should prevent scroll chaining, but currently does not`
同じく`fixes a firefox issue that starts text selection`

useOverlayTrigger の`Aria 1.1 supports multiple values for aria-haspopup other than just menus.`

## まとめ

明日は の話です。お楽しみにー
