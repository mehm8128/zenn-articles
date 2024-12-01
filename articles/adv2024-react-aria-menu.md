---
title: "Menuについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は Menu について書いていきます。

https://react-spectrum.adobe.com/react-aria/useMenu.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx

```

## 主な a11y 考慮事項

https://www.w3.org/WAI/ARIA/apg/patterns/menubar/

- `menu`role
- キーボード操作
- サブメニュー

## いくつかピックアップ

### キーボード操作

大体 listbox とかと同じ

### サブメニュー

https://react-spectrum.adobe.com/react-aria/Menu.html のコンポーネントの方参照するのがよさそう

フォーカス移動、useSafelyMouseToSubmenu など
state の組み合わせ方は RAC を読む

https://react-spectrum.adobe.com/blog/creating-a-pointer-friendly-submenu-experience.html

keyboard shortcut の実装見る。これなに

## まとめ

明日は の話です。お楽しみにー
