---
title: "Breadcrumbsについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は Breadcrumbs について書いていきます。

https://react-spectrum.adobe.com/react-aria/useBreadcrumbs.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx
function Breadcrumbs(props) {
  let { navProps } = useBreadcrumbs(props);
  let childCount = React.Children.count(props.children);

  return (
    <nav {...navProps}>
      <ol style={{ display: "flex", listStyle: "none", margin: 0, padding: 0 }}>
        {React.Children.map(props.children, (child, i) =>
          React.cloneElement(child, { isCurrent: i === childCount - 1 })
        )}
      </ol>
    </nav>
  );
}

function BreadcrumbItem(props) {
  let ref = React.useRef(null);
  let { itemProps } = useBreadcrumbItem({ ...props, elementType: "span" }, ref);
  return (
    <li>
      <span
        {...itemProps}
        ref={ref}
        style={{
          color: props.isDisabled ? "var(--gray)" : "var(--blue)",
          textDecoration:
            props.isCurrent || props.isDisabled ? "none" : "underline",
          fontWeight: props.isCurrent ? "bold" : null,
          cursor: props.isCurrent || props.isDisabled ? "default" : "pointer",
        }}
      >
        {props.children}
      </span>
      {!props.isCurrent && (
        <span aria-hidden="true" style={{ padding: "0 5px" }}>
          {"›"}
        </span>
      )}
    </li>
  );
}
```

## 主な a11y 事項

https://www.w3.org/WAI/ARIA/apg/patterns/breadcrumb/

- `navigation` landmark role
- `aria-current`

## いくつかピックアップ

### ランドマーク

パンくずリストは`nav`要素で囲うことでランドマークにします。
また、`aria-label`などで accessible name をつけることが必要になります。明示的に与えない場合は React Aria 側で i18n 対応をしながら自動で付与します。例えば日本語だと「パンくずリスト」という accessible name がついてくれます。

https://github.com/adobe/react-spectrum/blob/5ed06068ee2742f32e066ffa8eb55fd93a083123/packages/%40react-aria/breadcrumbs/src/useBreadcrumbs.ts#L35

### `aria-current`

現在のページを表しているアイテムには`aria-current="page"`をつけます。
ただし、現在のページを表すアイテムのみ`a`タグではなくて`span`で実装している、などといった場合には`aria-current`は必須ではありません。

https://www.w3.org/WAI/ARIA/apg/patterns/breadcrumb/

> If the element representing the current page is not a link, aria-current is optional.

## 疑問点

### 読み上げについて

Example で最初にフォーカスしたときに「3 項目」と読み上げられるのに 3 項目目（今いるページの項目）がフォーカス不可能なのは問題ないのか気になりました。APG の実装例では今いるページの項目もフォーカス可能になっているようです。

## まとめ

明日は Text Field の話です。お楽しみにー
