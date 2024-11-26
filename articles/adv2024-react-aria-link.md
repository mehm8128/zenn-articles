---
title: "Linkについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は Link について書いていきます。

https://react-spectrum.adobe.com/react-aria/useLink.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx
function Link(props) {
  let ref = React.useRef(null);
  let { linkProps } = useLink(props, ref);

  return (
    <a {...linkProps} ref={ref} style={{ color: "var(--blue)" }}>
      {props.children}
    </a>
  );
}
```

## 主な a11y 考慮事項

https://www.w3.org/WAI/ARIA/apg/patterns/link/

- `link`role

## その他

### client side navigation

`useLink`を用いると、例えば Next.js の`router.push()`などの client side navigation が `a` タグをクリックしたときに実行されるようになります。つまり、Next.js の`Link`コンポーネントのような動きをすることになります。

こちらのページに詳細が書かれています。
https://react-spectrum.adobe.com/react-aria/routing.html

実現方法としては上記のページに書かれているような方法で`RouterProvider`の`navigate`props に`router.push`などのナビゲーション関数を登録すると、`useLink`内部で以下のようにして`RouterProvider`の context から`router`を取得しています（この`useRouter`は React Aria 独自のものです）。

```ts
let router = useRouter();
```

そして、`useLink`の `linkProps` を渡した要素の`onClick`で`e.preventDefault()`して、`router.open()`（`navigate`関数の発火などが含まれているメソッド）が実行され、無事 client side navigation が実現されています。

https://github.com/adobe/react-spectrum/blob/12920fc91afa90d54ae769db45a1cff4b823e1bb/packages/%40react-aria/link/src/useLink.ts#L92-L93

https://github.com/adobe/react-spectrum/blob/12920fc91afa90d54ae769db45a1cff4b823e1bb/packages/%40react-aria/utils/src/openLink.tsx#L45-L53

### リンクを disabled にする方法

`useLink`内部で、`useButton`に出てきた`usePress`を利用しているのですが、`useLink`に`isDisabled`を渡すと`usePress`内で`e.preventDefault()`してくれて、ナビゲーションが発火しません。
これによって disabled が実現されています。

https://github.com/adobe/react-spectrum/blob/12920fc91afa90d54ae769db45a1cff4b823e1bb/packages/%40react-aria/interactions/src/usePress.ts#L334-L336

### `useLink`で足りないもの

色々と面倒を見てくれる`useLink`ですが、どうしても実現できないものもあります。
以下のリンクの「note」を見てください。

https://www.w3.org/WAI/ARIA/apg/patterns/link/

> Authors are strongly encouraged to use a native host language link element, such as an HTML <A> element with an href attribute. As with other WAI-ARIA widget roles, applying the link role to an element will not cause browsers to enhance the element with standard link behaviors, such as navigation to the link target or context menu actions. When using the link role, providing these features of the element is the author's responsibility.

`a` タグのリンクを使うとき、ブラウザはいくつか便利機能を提供してくれています。
例えば Chrome の場合、リンクにホバーしたとき、左下に小さく URL が表示されます。また、リンクの上で右クリックしたときのコンテキストメニューに「新しいタブで開く」などの項目があったり、中クリックしても新しいタブでリンク先のページを開くことができたりします。
しかし、`span`タグでリンクを実装して`useLink`を利用した場合、`a`タグではないため、ブラウザはこれらの機能を提供してくれません。

以下のページの Example では、リンクが`a`タグ以外のタグで実装されています。上で挙げた機能が提供されていないことを確認してみてください。

https://www.w3.org/WAI/ARIA/apg/patterns/link/examples/link/#ex_label

よって、リンクはできるだけ`a`タグで実装するのが好ましいです。

## 疑問点

disabled にしたとき、focusable かどうかが HTML 要素の種類によって決まっていて、`a`タグを使っている場合は focusable だけど`span`タグの場合は focusable でない、みたいになっていて一貫していないのは問題ないのか気になりました。

https://github.com/adobe/react-spectrum/blob/12920fc91afa90d54ae769db45a1cff4b823e1bb/packages/%40react-aria/link/src/useLink.ts#L55-L60

## まとめ

明日は Text Field の話です。お楽しみにー
