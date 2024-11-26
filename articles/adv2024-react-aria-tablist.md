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

### キーボード操作

https://github.com/adobe/react-spectrum/blob/b3a4d6c1134aae882aa1dcfce64efba1d8f4308d/packages/%40react-aria/tabs/src/useTabPanel.ts#L31-L34
の`tabbable`はコンポーネントの tab のことではなくて、tab キーでフォーカスできる要素のこと
↑ に関連して、tabpanel 内に tabbable なものがなければ、panel 全体が tabindex=0 になる

矢印キーとかは`TabsKeyboardDelegate.ts`にある

### `aria-`

controls とか selected とか

### i18n

tab がひっくりかえるよ

## その他

## 疑問点

## まとめ

明日は の話です。お楽しみにー
