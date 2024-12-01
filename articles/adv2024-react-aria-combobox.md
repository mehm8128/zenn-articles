---
title: "Comboboxについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は Combobox について書いていきます。

https://react-spectrum.adobe.com/react-aria/useCombobox.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx

```

## 主な a11y 考慮事項

https://www.w3.org/WAI/ARIA/apg/patterns/listbox/

- ``role

## いくつかピックアップ

フォーカス移動

https://react-spectrum.adobe.com/blog/building-a-combobox.html

https://zenn.dev/mehm8128/articles/react-aria-combobox

`VoiceOver has issues with announcing aria-activedescendant properly on change`から 3 つ、useEffect で手動読み上げさせてるやつ

https://github.com/adobe/react-spectrum/issues/7228
https://github.com/adobe/react-spectrum/issues/6007
https://github.com/adobe/react-spectrum/issues/3900
https://github.com/adobe/react-spectrum/issues/3306
から面白そうなのを取り上げる

## その他

## 疑問点

## まとめ

明日は の話です。お楽しみにー
