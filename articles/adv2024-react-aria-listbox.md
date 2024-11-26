---
title: "ListBoxについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

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

## 主な a11y 考慮事項

https://www.w3.org/WAI/ARIA/apg/patterns/listbox/

- `listbox`role
- 各オプションが`option`role
- グルーピングされている場合、`group`role の利用
- 複数選択可能な場合、`aria-multiselectable`を付与
- 選択されているオプションは`aria-selected`などを付与
- 必要に応じて`aria-setsize`と`aria-posinset`を設定
- オプションが水平に配置されている場合、`aria-orientation`を`horizontal`に設定
- キーボード操作

## いくつかピックアップ

### オプションのグルーピング

https://react-spectrum.adobe.com/react-aria/useListBox.html#sections

にあるように、`useListBoxSection`でグループ化ができます。
実装的には`group`role でグループ化して、`presentation`role にした header で`group`role の要素に accessible name を与えています。

https://github.com/adobe/react-spectrum/blob/5ed06068ee2742f32e066ffa8eb55fd93a083123/packages/%40react-aria/listbox/src/useListBoxSection.ts#L45-L59

そうすることで、`Static items`の例だと以下のように読み上げられます。

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

### `aria-multiselectable`

複数選択な ListBox の場合に`true`にするのですが、NVDA だと特に「複数選択」みたいに読み上げられたりしてなくて、どう判別されるのかが気になりました。キーボード操作に影響するので認識できないとまずい気がします。

### `aria-setsize`と`aria-posinset`

Virtual Scroll する場合に利用します。`aria-setsize`が ListBox 全体のオプションの数、`aria-posinset`がそのオプションが全体の何番目のオプションなのかを表すものです。

https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-setsize

https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-posinset

実装はこのあたりです。

https://github.com/adobe/react-spectrum/blob/main/packages/%40react-aria/listbox/src/useOption.ts#L121-L125

### キーボード操作

Tab でフォーカスした後、矢印キーで移動するのが主な操作です。単一選択の場合は、フォーカスの移動とともに自動で選択されることがあります。これを"selection follows focus"と呼ぶらしいです（[APG](https://www.w3.org/WAI/ARIA/apg/patterns/listbox/#:~:text=%22selection%20follows%20focus%22.)を参照）。
その他のキーボード操作も APG に詳細が記載されています。

## その他

### オプションのラベル

APG の最初の方に、ListBox の各オプションのラベルについて言及がありました。
https://www.w3.org/WAI/ARIA/apg/patterns/listbox/

> Avoiding very long option names facilitates understandability and perceivability for screen reader users.

長いラベル名はやめましょう。

> Sets of options where each option name starts with the same word or phrase can also significantly degrade usability for keyboard and screen reader users.

各オプションのラベルの最初が同じだと、毎回同じものが読み上げられて探しにくい。

みたいな感じのことが書かれています。

後者は例えば「日本 東京都」という選択肢と「日本 大阪府」という選択肢があると、「日本」までは同じなのでこれが毎回読み上げられると目当てのものを探すのが大変、という話ですね。こういう場合は国名と都市名で別で ListBox を用意するのがよい、とのことです。

### `label`要素について

ソースコードに以下のようなコメントがありました。
https://github.com/adobe/react-spectrum/blob/694fc853ea6cbecb1a72d0a95ef460aaede65171/packages/%40react-aria/listbox/src/useListBox.ts#L115-L116

知らなかったのですが、確かに[HTML Standard の category-label の項目](https://html.spec.whatwg.org/multipage/forms.html#category-label:~:text=Some%20elements%2C%20not%20all%20of%20them%20form%2Dassociated%2C%20are%20categorized%20as%20labelable%20elements.%20These%20are%20elements%20that%20can%20be%20associated%20with%20a%20label%20element.)にあるように、基本的には role 関係なくタグ名だけで`label`要素でラベル付け可能かどうかが決まるようです。

### Typeahead

`useListBox`の中で使われている`useSelectableList`の中で使われている`useSelectableCollection`の中で使われている`useTypeSelect`で、Typeahead が実装されています。

https://github.com/adobe/react-spectrum/blob/5ed06068ee2742f32e066ffa8eb55fd93a083123/packages/%40react-aria/selection/src/useTypeSelect.ts#L47

またこのために、`useSelectableList`内で`useCollator`を用いて i18n 対応がされています。`useCollator`内では[Intl.Collator - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/Collator)が用いられているようです。

https://github.com/adobe/react-spectrum/blob/5ed06068ee2742f32e066ffa8eb55fd93a083123/packages/%40react-aria/selection/src/useSelectableList.ts#L62

## まとめ

明日は Number Field の話です。お楽しみにー
