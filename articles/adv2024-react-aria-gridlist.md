---
title: "GridListについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

:::message
この記事は [React Aria の実装読むぞ - Qiita Advent Calendar 2024](https://qiita.com/advent-calendar/2024/react-aria) の 10 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は GridList について書いていきます。

https://react-spectrum.adobe.com/react-aria/useGridList.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx
function List(props) {
  let state = useListState(props);
  let ref = useRef<HTMLUListElement | null>(null);
  let { gridProps } = useGridList(props, state, ref);

  return (
    <ul {...gridProps} ref={ref} className="list">
      {[...state.collection].map((item) => (
        <ListItem key={item.key} item={item} state={state} />
      ))}
    </ul>
  );
}

function ListItem({ item, state }) {
  let ref = React.useRef(null);
  let { rowProps, gridCellProps, isPressed } = useGridListItem(
    { node: item },
    state,
    ref
  );

  let { isFocusVisible, focusProps } = useFocusRing();
  let showCheckbox =
    state.selectionManager.selectionMode !== "none" &&
    state.selectionManager.selectionBehavior === "toggle";

  return (
    <li
      {...mergeProps(rowProps, focusProps)}
      ref={ref}
      className={`${isPressed ? "pressed" : ""} ${
        isFocusVisible ? "focus-visible" : ""
      }`}
    >
      <div {...gridCellProps}>
        {showCheckbox && <ListCheckbox item={item} state={state} />}
        {item.rendered}
      </div>
    </li>
  );
}
```

## 本題

https://www.w3.org/WAI/ARIA/apg/patterns/grid/

### GridList は 2 種類ある

APG によると、Grid パターンは大きく 2 種類に分かれます。

https://www.w3.org/WAI/ARIA/apg/patterns/grid/

それぞれ説明していきます。

#### データグリッド

表形式でデータを表示するためのパターンです。
インタラクティブで、フォーカス可能な要素を含みます。完全に静的な場合はテーブルパターンや ListBox パターンを使うとよさそうです。
また、セルの選択や拡張などもサポートする場合があります。

僕が面白いと思ったのはこれです。

> Only one of the focusable elements contained by the grid is included in the page tab sequence.

データグリッドパターンの場合、1 つの表に複数フォーカス可能な要素があることになりますが、データグリッド外から Tab キーを順番に押していったときに、データグリッド内の要素でフォーカスされるのは 1 つだけになります。これはページ全体を Tab キーで移動していてデータグリッド内をそんなに重点的に見たいというわけではないときに、素早くページを閲覧できるようにするためです。

Example ページがあるのでこちらを触ってみると分かりやすいと思います。

https://www.w3.org/WAI/ARIA/apg/patterns/grid/examples/data-grids/

Tab キーを押していくと、「Open In CodePen」の次に「01-Jan-16」のセルにフォーカスされ、次には「Deposit」のセルではなくて、Example 2 の「Open In CodePen」にフォーカスが飛ぶようになっています。

なので、データグリッド内では矢印キーでセルの移動ができるようになっています。

ただ、フォーカスが端にいったときに折り返すようにしてしまうと今どこにいるかが分かりづらくなってしまうので、折り返しはしないらしいです。

> If focus is on the right-most cell in the row, focus does not move.

React Aria の`useGridList`は主にこちらをサポートしていそうというのと、今回のアドベントカレンダーでは見送ったのですが`useTable`もこちらになります。

https://react-spectrum.adobe.com/react-aria/useTable.html

#### レイアウトグリッド

インタラクティブ要素をグループ化するために用いられるものです。
こちらも Tab キーを押していったときの挙動がデータグリッドと同様で、矢印キーでレイアウトグリッド内を移動できるようになっています。

https://www.w3.org/WAI/ARIA/apg/patterns/grid/examples/layout-grids/

こちらは React Aria だと`useTagGroup`などが該当します。

https://react-spectrum.adobe.com/react-aria/useTagGroup.html

### キーボード操作

`useGridList`で使っている`useSelectableList`で使っている`useSelectableCollection`の`onKeyDown`をちゃんと読む。treewalker とか

### 読み上げ

useGridSelectionAnnouncement
選択された・解除されたときに読み上げるらしい

## まとめ

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、 GridList についての記事です。お楽しみにー
