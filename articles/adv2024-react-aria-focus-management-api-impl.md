---
title: "【番外編】Focus Management APIについて（実装編） - React Ariaの実装読むぞ"
emoji: "🎮"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

:::message
この記事は [React Aria の実装読むぞ - Qiita Advent Calendar 2024](https://qiita.com/advent-calendar/2024/react-aria) の 18 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は Focus Management API の実装について書いていきます。

### `FocusScope`

`FocusScope`コンポーネント内で色んな hooks を実行したり、`useFocusManager`を実行できるようにするための Provider を提供したりしています。

`scopeRef`に`FocusScope`内の要素を配列として保持しているようで、以下の`useLayoutEffect`内で取得しています。コメントにある`sentinels`というのは`startRef`と`endRef`をつけている`span`要素のことで、これを目印にしてここからここまで、というのを決めているようです。

https://github.com/adobe/react-spectrum/blob/326f48154e301edab425c8198c5c3af72422462b/packages/%40react-aria/focus/src/FocusScope.tsx#L117-L136

https://github.com/adobe/react-spectrum/blob/326f48154e301edab425c8198c5c3af72422462b/packages/%40react-aria/focus/src/FocusScope.tsx#L187-L193

ここで取得した`scopeRef`は次から見ていく`useFocusContainment`などの hooks にも渡されています。
それでは、`FocusScope`に渡すことができる props である`contain`, `restoreFocus`, `autoFocus`に関係する hooks を見ていきます。

#### `useFocusContainment`

focus containment を実現する hook です。
`onKeyDown`関数で Tab キーによるフォーカス移動を`e.preventDefault()`した上で、 TreeWalker API（参考: [Radio と Checkbox について - React Aria の実装読むぞ](https://zenn.dev/mehm8128/articles/adv2024-react-aria-radio-and-checkbox#treewalker-api)）などを用いて、最後の tabbable な要素から最初の tabbable な要素にフォーカスを移動する処理などが実装されています。

https://github.com/adobe/react-spectrum/blob/326f48154e301edab425c8198c5c3af72422462b/packages/%40react-aria/focus/src/FocusScope.tsx#L330-L357

#### `useRestoreFocus`

フォーカスの復元を実現する hook です。
mount 時に`nodeToRestoreRef`に[`document.activeElement`](https://developer.mozilla.org/ja/docs/Web/API/Document/activeElement)で取得した現在フォーカスされている（この`FocusScope`外で最後にフォーカスされた）要素を入れておき、記憶しておきます。
つまり、RFC に書かれていたように「`FocusScope`内で最後にフォーカスを持っていた要素をその`FocusScope`が記憶しておく」のではなく、「現在アクティブな（内側にフォーカスされている要素を持っている）`FocusScope`が、その外で最後にフォーカスを持っていた要素を記憶しておく」ような実装になっているのだと理解しました。
例えばダイアログとそのトリガーボタンだと、トリガーボタンが押されてフォーカスがダイアログ内に移動したときに、トリガーボタンに最後にフォーカスがあったということをダイアログとトリガーボタンを囲っている`FocusScope`が記憶しているのではなく、ダイアログだけを囲っている`FocusScope`が新しく記憶し、その`FocusScope`が unmount されたタイミングでその記憶している要素にフォーカスを戻すようになっているということです。
こっそり追記しておいたのですが、[Toast について - React Aria の実装読むぞ](https://zenn.dev/mehm8128/articles/adv2024-react-aria-toast#%E3%83%95%E3%82%A9%E3%83%BC%E3%82%AB%E3%82%B9%E6%93%8D%E4%BD%9C)の記事で言及していた疑問もこれで解消されました。

フォーカスの復元処理はここらへんで`restoreFocusToElement`関数で行っているようです。

https://github.com/adobe/react-spectrum/blob/326f48154e301edab425c8198c5c3af72422462b/packages/%40react-aria/focus/src/FocusScope.tsx#L690-L726

また、`FocusScope`コンポーネント内で、アクティブな`FocusScope`の変更も行っています。

https://github.com/adobe/react-spectrum/blob/326f48154e301edab425c8198c5c3af72422462b/packages/%40react-aria/focus/src/FocusScope.tsx#L171-L176

#### `useAutoFocus`

auto focus を実現する hook です。

mount 時に`getFirstInScope`関数を用いて、`FocusScope`内の最初の tabbable な要素にフォーカスします。なお、tabbable な要素が見つからなかったら最初の focusable な要素にフォーカスします。

https://github.com/adobe/react-spectrum/blob/326f48154e301edab425c8198c5c3af72422462b/packages/%40react-aria/focus/src/FocusScope.tsx#L507-L516

https://github.com/adobe/react-spectrum/blob/326f48154e301edab425c8198c5c3af72422462b/packages/%40react-aria/focus/src/FocusScope.tsx#L483-L499

### `useFocusManager`

`useFocusManager`は親の`FocusScope`から context を受け取って色んなメソッドを実行できるようになっています。ここらへんで TreeWalker API を使って実装されています。

https://github.com/adobe/react-spectrum/blob/326f48154e301edab425c8198c5c3af72422462b/packages/%40react-aria/focus/src/FocusScope.tsx#L205-L268

## まとめ

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、 ProgressBar についての記事です。お楽しみにー
