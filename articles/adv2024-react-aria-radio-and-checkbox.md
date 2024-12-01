---
title: "RadioとCheckboxについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は Radio と Checkbox について書いていきます。

https://react-spectrum.adobe.com/react-aria/useRadioGroup.html
https://react-spectrum.adobe.com/react-aria/useCheckbox.html
https://react-spectrum.adobe.com/react-aria/useCheckboxGroup.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx

```

## 主な a11y 考慮事項

https://www.w3.org/WAI/ARIA/apg/patterns/radio/
https://www.w3.org/WAI/ARIA/apg/patterns/checkbox/

- `radiogroup`, `checkbox`roles
- styling
- フォーカス制御

## いくつかピックアップ

### styling

スタイリングしやすいように、visually hidden で`input`要素を隠します。
[VisuallyHidden](https://react-spectrum.adobe.com/react-aria/VisuallyHidden.html) コンポーネントがあるので、これで`input`要素を wrap するだけで OK です。

### フォーカス制御

#### Tab フォーカス

ラジオグループの場合、Tab フォーカスはグループの中で選択されているラジオボタンか、選択されているラジオボタンがなければ最後にフォーカスされたラジオボタンにあたり、それ以外は Tab ではなくて矢印キーで移動します。

ただ、APG には選択されているラジオボタンがなければ、グループ内の最初のラジオボタンにフォーカスされることが多いと書いてありました。

> If none of the radio buttons are checked, focus is set on the first radio button in the group.

https://github.com/adobe/react-spectrum/blob/10a43de887ffc28913c770a33573aebf3df786fc/packages/%40react-aria/radio/src/useRadio.ts#L83-L93

#### 2 種類のフォーカス移動

APG の例では 2 種類の方法でグループ内のラジオボタンのフォーカスを移動する方法が紹介されています。

1 つは`tabindex`を変化させる方法です。これは React Aria で用いられている方法です。選択されている要素を`tabindex="0"`にし、選択されていない要素を`tabindex="-1"`にします。矢印キーが押されるたびにこれを変化させていくことで、選択されている要素にフォーカスを当てていくことができます。

https://www.w3.org/WAI/ARIA/apg/patterns/radio/examples/radio/

もう 1 つの方法は、`aria-activedescendant`を用いる方法です。フォーカスは常に`group`role（ラジオなら`radiogroup`role）を持つ要素に当てておき、そのグループ内でアクティブな要素（選択されているラジオボタン）の id を`aria-activedescendant`に渡すことで、アクティブな要素をスクリーンリーダーが読み上げてくれます。

https://www.w3.org/WAI/ARIA/apg/patterns/radio/examples/radio-activedescendant/

#### TreeWalker

矢印キーが押されたときに次にフォーカスするべき要素を特定するために、`getFocusableTreeWalker`関数が用いられています。

https://github.com/adobe/react-spectrum/blob/10a43de887ffc28913c770a33573aebf3df786fc/packages/%40react-aria/radio/src/useRadioGroup.ts#L104-L124

https://github.com/adobe/react-spectrum/blob/10a43de887ffc28913c770a33573aebf3df786fc/packages/%40react-aria/focus/src/FocusScope.tsx#L744-L774

`getFocusableTreeWalker` について簡単に説明していきます。
この関数では、HTML のノードを探索するために利用できる TreeWalker という API が使われています。

https://developer.mozilla.org/ja/docs/Web/API/TreeWalker

`Document.createTreeWalker`関数で`walker`を作成します。第一引数にルートの要素、第二引数にどのような種類のノードを探索するか（`whatToShow`）というフラグを組み合わせたビットマスクを指定します。ビットマスクなので、例えば`Element`ノードと`Comment`ノードをどちらも探索したい場合は`NodeFilter.SHOW_ELEMENT + NodeFilter.SHOW_COMMENT`を指定すればよいです。
そして第三引数には、第二引数で指定したノードを探索していく中でさらにどういう条件を満たすノードを含み、どういう条件を満たすノードを含まないのかを指定する `acceptNode`という callback 関数を指定します。各ノードに対して`NodeFilter.FILTER_ACCEPT`を return するとこのノードを含み、`NodeFilter.FILTER_REJECT`を返すとこのノードとそのサブツリーの全てのノードを含まず、`NodeFilter.FILTER_SKIP`を返すとこのノードのみを含まないでサブツリーは探索を続けます。

今回の場合、`onKeyDown`が発火したタイミングで次にフォーカス可能なラジオボタンを探すのか、前のフォーカス可能なラジオボタンを探すのかを判定し、`walker.nextNode()`や`walker.previousNode()`、もしくは`walker.firstChild()`や`walker.lastChild()`などを呼んでいます。このタイミングで先ほどの`acceptNode`関数を発火し、探索していきます。今回はフォーカス可能なノードを探すので`selector`を以下のように定義し、`(node as Element).matches(selector)`でフォーカス可能かどうかを判定しています。

```ts
let selector = opts?.tabbable
  ? TABBABLE_ELEMENT_SELECTOR
  : FOCUSABLE_ELEMENT_SELECTOR;
```

ちなみに`focusable`は`tabindex="0"`などのノードはもちろん、`tabindex="-1"`で programmatically にフォーカス可能なノードも含み、`tabbable`は`tabindex="-1"`は含まず、Tab キーによってフォーカス可能なノードのみを表します。

これを用いて「一番下のラジオボタンにフォーカスされているときに下矢印キーが押されたら一番上のラジオボタンにフォーカスする」などといった動作が実現されています。

### グループ化

group 化を a11y 本と照らし合わせて確認

## まとめ

明日は の話です。お楽しみにー
