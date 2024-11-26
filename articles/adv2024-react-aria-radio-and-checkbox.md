---
title: "RadioとCheckboxについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は Radio と Checkbox について書いていきます。

https://react-spectrum.adobe.com/react-aria/useRadioGroup.html
https://react-spectrum.adobe.com/react-aria/useCheckbox.html
https://react-spectrum.adobe.com/react-aria/useCheckboxGroup.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx

```

## 主な a11y 考慮事項

https://www.w3.org/WAI/ARIA/apg/patterns/radio/
https://www.w3.org/WAI/ARIA/apg/patterns/checkbox/

- `radiogroup`, `role`roles
- styling
- フォーカス制御
- indeterminate checkbox

## いくつかピックアップ

### styling

スタイリングしやすいように、visually hidden で`input`要素を隠します。
[VisuallyHidden](https://react-spectrum.adobe.com/react-aria/VisuallyHidden.html) コンポーネントがあるので、これで`input`要素を wrap するだけで OK です。

### フォーカス制御

TreeWalker
https://developer.mozilla.org/ja/docs/Web/API/TreeWalker
rtl
https://react-spectrum.adobe.com/react-aria/useRadioGroup.html#rtl
tabindex=-1
aria-activedescendant

### indeterminate checkbox

## まとめ

明日は の話です。お楽しみにー
