---
title: "TagGroupについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は TagGroup について書いていきます。

https://react-spectrum.adobe.com/react-aria/useTagGroup.html

## `useTagGroup` が提供しているもの

## 使用例

ドキュメントからそのまま取ってきています。

```tsx
function TagGroup<T extends object>(props: AriaTagGroupProps<T>) {
  let { label, description, errorMessage } = props;
  let ref = React.useRef(null);

  let state = useListState(props);
  let { gridProps, labelProps, descriptionProps, errorMessageProps } =
    useTagGroup(props, state, ref);

  return (
    <div className="tag-group">
      <div {...labelProps}>{label}</div>
      <div {...gridProps} ref={ref}>
        {[...state.collection].map((item) => (
          <Tag key={item.key} item={item} state={state} />
        ))}
      </div>
      {description && (
        <div {...descriptionProps} className="description">
          {description}
        </div>
      )}
      {errorMessage && (
        <div {...errorMessageProps} className="error-message">
          {errorMessage}
        </div>
      )}
    </div>
  );
}

function Tag<T>(props: TagProps<T>) {
  let { item, state } = props;
  let ref = React.useRef(null);
  let { focusProps, isFocusVisible } = useFocusRing({ within: true });
  let { rowProps, gridCellProps, removeButtonProps, allowsRemoving } = useTag(
    props,
    state,
    ref
  );

  return (
    <div
      ref={ref}
      {...rowProps}
      {...focusProps}
      data-focus-visible={isFocusVisible}
    >
      <div {...gridCellProps}>
        {item.rendered}
        {allowsRemoving && <Button {...removeButtonProps}>❎</Button>}
      </div>
    </div>
  );
}
```

## 主な a11y 考慮事項

https://www.w3.org/WAI/ARIA/apg/patterns/grid/

- `grid`role
- live region
- キーボード操作

## いくつかピックアップ

### Layout Grid

`useGridList`で紹介した Layout Grid パターンです。

ここに似たような例があります。
https://www.w3.org/WAI/ARIA/apg/patterns/grid/examples/layout-grids/#ex2_label

### live region

https://github.com/adobe/react-spectrum/blob/main/packages/%40react-aria/tag/src/useTagGroup.ts#L106-L108

live region 属性がついているので、タグに変更があった場合にスクリーンリーダーによって通知されます。
今回の場合は`aria-relevant`が`addition`に設定されているので、何らかの要因によってタグが追加された場合に通知されるようになっているのだと思います。

https://developer.mozilla.org/ja/docs/Web/Accessibility/ARIA/ARIA_Live_Regions

[`aria-live`](https://developer.mozilla.org/ja/docs/Web/Accessibility/ARIA/Attributes/aria-live)は`polite`にすると現在のタスクを中断せずに優先度低く通知します。
[`aria-relevant`](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-relevant)は`addition`にするとノードが追加された場合のみ通知するようになります。
[`aria-atomic`](https://developer.mozilla.org/ja/docs/Web/Accessibility/ARIA/Attributes/aria-atomic)は`false`にして、変更のあった部分のみ読み上げられるように設定しています。

### キーボード操作

Tab でフォーカス後、矢印キーで移動できます。
タグの削除には Delete キーを用います。これは削除可能なタグにフォーカスしたときに、削除方法が accessible description として「タグを削除するには、Delete キーを押します。」のように読み上げられるようになっています。

## まとめ

明日は Number Field の話です。お楽しみにー
