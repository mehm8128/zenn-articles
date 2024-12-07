---
title: "ListBoxについて - React Ariaの実装読むぞ"
emoji: "📜"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

:::message
この記事は [React Aria の実装読むぞ - Qiita Advent Calendar 2024](https://qiita.com/advent-calendar/2024/react-aria) の 9 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は ListBox について書いていきます。

https://react-spectrum.adobe.com/react-aria/useListBox.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx
function ListBox<T extends object>(props: AriaListBoxProps<T>) {
  let state = useListState(props);

  let ref = React.useRef(null);
  let { listBoxProps, labelProps } = useListBox(props, state, ref);

  return (
    <>
      <div {...labelProps}>{props.label}</div>
      <ul {...listBoxProps} ref={ref}>
        {[...state.collection].map((item) =>
          item.type === "section" ? (
            <ListBoxSection key={item.key} section={item} state={state} />
          ) : (
            <Option key={item.key} item={item} state={state} />
          )
        )}
      </ul>
    </>
  );
}

function Option({ item, state }) {
  let ref = React.useRef(null);
  let { optionProps } = useOption({ key: item.key }, state, ref);

  let { isFocusVisible, focusProps } = useFocusRing();

  return (
    <li
      {...mergeProps(optionProps, focusProps)}
      ref={ref}
      data-focus-visible={isFocusVisible}
    >
      {item.rendered}
    </li>
  );
}
```

## 本題

APG はこちらです。
https://www.w3.org/WAI/ARIA/apg/patterns/listbox/

### オプションのグルーピング

https://react-spectrum.adobe.com/react-aria/useListBox.html#sections

にあるように、`useListBoxSection`でグループ化ができます。
実装的には`group`role でグループ化して、`presentation`role にした heading で`group`role の要素に accessible name を与えています。

https://github.com/adobe/react-spectrum/blob/5ed06068ee2742f32e066ffa8eb55fd93a083123/packages/%40react-aria/listbox/src/useListBoxSection.ts#L45-L59

`Techincally, listbox cannot contain headings according to ARIA.`については、[WAI-ARIA の `listbox`role の項目](https://w3c.github.io/aria/#listbox)の`Allowed Accessibility Child Roles`を見てください。単純なオプションとなる`option`role か、オプションをグルーピングするための`group`role しか許可されていないので、グルーピングしたセクションの見出しに`heading`role を用いることができないという意味です。

グルーピングすることで、`Static items`の例だと以下のように読み上げられます。

```
Choose sandwich contents  リスト
Veggies  グループ
Lettuce  9の1
Tomato  選択なし  9の2
Onion  選択なし  9の3
Protein  グループ
Ham  選択なし  9の4
Tuna  選択なし  9の5
Tofu  選択なし  9の6
Condiments  グループ
Mayonaise  選択なし  9の7
Mustard  選択なし  9の8
Ranch  選択なし  9の9

```

グループに入ったタイミングで一度だけグループ名が読み上げられます。

### `aria-setsize`と`aria-posinset`

Virtual Scroll する場合に利用します。`aria-setsize`が ListBox 全体のオプションの数、`aria-posinset`がそのオプションが全体の何番目のオプションなのかを表すものです。

https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-setsize

https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-posinset

実装はこのあたりです。

https://github.com/adobe/react-spectrum/blob/main/packages/%40react-aria/listbox/src/useOption.ts#L121-L125

### オプションのラベル

APG の最初の方に、ListBox の各オプションのラベルについて言及がありました。
https://www.w3.org/WAI/ARIA/apg/patterns/listbox/

> Avoiding very long option names facilitates understandability and perceivability for screen reader users.

長いラベル名はやめましょう。

> Sets of options where each option name starts with the same word or phrase can also significantly degrade usability for keyboard and screen reader users.

各オプションのラベルの最初が同じだと、毎回同じものが読み上げられて探しにくい。

みたいな感じのことが書かれています。

後者は例えば「日本 東京都」という選択肢と「日本 大阪府」という選択肢があると、「日本」までは同じなのでこれが毎回読み上げられると目当てのものを探すのが大変、という話ですね。こういう場合は国名と都市名で別で ListBox を用意するのがよい、とのことです。

### Typeahead

`useListBox`の中で使われている`useSelectableList`の中で使われている`useSelectableCollection`の中で使われている`useTypeSelect`で、Typeahead が実装されています。

https://github.com/adobe/react-spectrum/blob/5ed06068ee2742f32e066ffa8eb55fd93a083123/packages/%40react-aria/selection/src/useTypeSelect.ts#L44-L47

またこのために、`useSelectableList`内で`useCollator`を用いて i18n 対応もされています。これについては i18n の回で説明します。

https://github.com/adobe/react-spectrum/blob/5ed06068ee2742f32e066ffa8eb55fd93a083123/packages/%40react-aria/selection/src/useSelectableList.ts#L62

### `shouldSelectOnPressUp`

props として`allowsDifferentPressOrigin`と`shouldSelectOnPressUp`を`true`で渡すと、「メニューのトリガーボタン上で pointer down し、そのままメニュー内のボタンにカーソルを移動して pointer up する」というような、一回のクリックでメニューを開いてそのままメニュー内のボタンを発火させる操作ができるようになっています。以下のコードだと、273 行目の`onSelect`が発火します。
[2 日目の記事で説明した Pointer Events API](https://zenn.dev/mehm8128/articles/adv2024-react-aria-button#usepress%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6)が役に立っています。

https://github.com/adobe/react-spectrum/blob/8228e4efd9be99973058a1f90fc7f7377e673f78/packages/%40react-aria/selection/src/useSelectableItem.ts#L237-L298

## まとめ

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、 GridList についての記事です。お楽しみにー
