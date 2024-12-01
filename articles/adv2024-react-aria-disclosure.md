---
title: "Disclosureについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は Disclosure について書いていきます。

alpha 版なので、残念ながらまだ本番環境のドキュメントにはありません。
手元で https://github.com/adobe/react-spectrum を clone して、`npm run start:docs`して見てください。

http://localhost:1234/react-aria/useDisclosure.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx
function Disclosure(props) {
  let state = useDisclosureState(props);
  let panelRef = React.useRef<HTMLDivElement | null>(null);
  let triggerRef = React.useRef<HTMLButtonElement | null>(null);
  let { buttonProps: triggerProps, panelProps } = useDisclosure(
    props,
    state,
    panelRef
  );
  let { buttonProps } = useButton(triggerProps, triggerRef);
  let { isFocusVisible, focusProps } = useFocusRing();

  return (
    <div className="disclosure">
      <h3>
        <button
          className="trigger"
          ref={triggerRef}
          {...mergeProps(buttonProps, focusProps)}
          style={{ outline: isFocusVisible ? "2px solid dodgerblue" : "none" }}
        >
          <svg viewBox="0 0 24 24">
            <path d="m8.25 4.5 7.5 7.5-7.5 7.5" />
          </svg>
          {props.title}
        </button>
      </h3>
      <div className="panel" ref={panelRef} {...panelProps}>
        <p>{props.children}</p>
      </div>
    </div>
  );
}
```

## 主な a11y 考慮事項

https://www.w3.org/WAI/ARIA/apg/patterns/disclosure/

- `group`role
- `aria-`属性
- `hidden="until-found"`

## いくつかピックアップ

### `group`role と`aria-`属性

ボタンとパネルを結びつけたり、現在 disclosure が開いているかどうかを表したりするために、いくつかの`aria-`属性が用いられています。

https://github.com/adobe/react-spectrum/blob/3f44370de69e48ee56cbf2bbd8664cee8294e9fe/packages/%40react-aria/disclosure/src/useDisclosure.ts#L95-L98

https://github.com/adobe/react-spectrum/blob/3f44370de69e48ee56cbf2bbd8664cee8294e9fe/packages/%40react-aria/disclosure/src/useDisclosure.ts#L111-L114

`aria-expanded`の boolean で現在開いているかどうかの状態を表し、`aria-controls`でパネル（コンテンツ）と結びつけています。

また、非表示のときは`aria-hidden`や`hidden`属性がついています。

https://github.com/adobe/react-spectrum/blob/3f44370de69e48ee56cbf2bbd8664cee8294e9fe/packages/%40react-aria/disclosure/src/useDisclosure.ts#L116-L117

`group`role が用いられているのは、`detail`要素の暗黙の ARIA role が`group`role だからです。

https://w3c.github.io/html-aria/#el-details

### `hidden="until-found"`

`hidden="until-found"`がつけられています。

https://github.com/adobe/react-spectrum/blob/3f44370de69e48ee56cbf2bbd8664cee8294e9fe/packages/%40react-aria/disclosure/src/useDisclosure.ts#L71-L84

詳しい説明は MDN に任せるのですが、disclosure が閉じている状態でもページ内検索などでは disclosure 内のコンテンツがヒットするようにし、その結果コンテンツまでスクロールされたら`hidden`属性を外してコンテンツを表示するようにする、というものです。

https://developer.mozilla.org/ja/docs/Web/HTML/Global_attributes/hidden#hidden_until_found_%E7%8A%B6%E6%85%8B

TODO: beforematch 確認

React 側がまだ対応していないみたいな話も
https://github.com/facebook/react/pull/24741

## まとめ

明日は の話です。お楽しみにー
