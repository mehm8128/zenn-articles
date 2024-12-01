---
title: "TabListについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は TabList について書いていきます。

https://react-spectrum.adobe.com/react-aria/useTabList.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx

```

## 主な a11y 考慮事項

https://www.w3.org/WAI/ARIA/apg/patterns/listbox/

- `tab`role
- キーボード操作
- `aria-`
- i18n

## いくつかピックアップ

### role と`aria-`属性

1 つ 1 つのタブボタンは`tab`role、それらを囲っているものが`tablist`role、タブ選択時にコンテンツが表示される領域が`tabpanel`role です。
全ての`tab`role にはそれぞれに対応する`tabpanel`の id を`aria-controls`に指定し、さらにアクティブは`tab`role には`aria-selected="true"`をつけます。

### キーボード操作

Tab キーで現在アクティブなタブにフォーカスが当たり、矢印キーでタブの移動ができます。

また、タブにフォーカスが当たっているときに Tab キーを押した場合、タブパネルがその中にフォーカス可能な要素を含んでいればその要素にフォーカスが当たります。フォーカス可能な要素を含んでいなければタブパネル自体が`tabindex="0"`になってタブパネル全体にフォーカスが当たります。

https://github.com/adobe/react-spectrum/blob/b3a4d6c1134aae882aa1dcfce64efba1d8f4308d/packages/%40react-aria/tabs/src/useTabPanel.ts#L31-L34

（注意: `tabbale`、`tabbing`は Tab キーでフォーカスできる、Tab キーを押す、などの意味で、`tab`はタブコンポーネントを意味しています）

### タブの配置方向

縦に配置する場合と、左右反対に配置するパターンがあります。

縦に配置する場合は`aria-orientation="vertical"`をつけ、上下矢印キーでタブを移動できるようにします。
また、右から左に読む言語を利用している場合、最初のタブが一番右、最後のタブが一番左に配置されるようにする必要があり、さらにキーボード操作も左右逆にします。ドキュメントのデモ部分で devtools から`dir="ltr"`を`dir="rtl"`にすると体験できます。

### Pointer Cancellation

過去に こんな issue がありました。

https://github.com/adobe/react-spectrum/issues/4336

WCAG の [Success Criterion 2.5.2 Pointer Cancellation](https://www.w3.org/TR/WCAG22/#pointer-cancellation)に準拠せず、pointerdown のタイミングでしかタブの選択ができなかった時期があったようです。今回のケースは 4 つ挙げられているパターンのうちの「Abort or Undo」に当たると思います。
そこで、[useTab: adds support for shouldSelectOnPressUp #4342](https://github.com/adobe/react-spectrum/pull/4342)で`shouldSelectOnPressUp`props がサポートされ、間違えてタブをクリック（pointerdown）してしまったときでも pointerup する前にタブからカーソルを移動すればタブの選択をキャンセルすることができるようになりました。

### keyboard delegate

このタイミングでちゃんと確認するとよさそう

## まとめ

明日は の話です。お楽しみにー
