---
title: "PopoverとDialogについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は Popover と Dialog について書いていきます。

https://react-spectrum.adobe.com/react-aria/usePopover.html
https://react-spectrum.adobe.com/react-aria/useDialog.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx

```

## 主な a11y 考慮事項

https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/

- `dialog`role
- フォーカス制御
- モーダル化

## いくつかピックアップ

### フォーカス制御

閉じたら戻るとか最初にどこにフォーカスするとか

### 読み上げ

最初にどこが読み上げられるとか`aria-describedby`とか
`aria-haspopup`とかも書くかも

### モーダル化

popover 含めて、背景スクロールもできなくなるとか

## その他

## 疑問点

## まとめ

明日は の話です。お楽しみにー
