---
title: "NumberFieldについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は NumberField について書いていきます。

https://react-spectrum.adobe.com/react-aria/useNumberField.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx

```

## 主な a11y 考慮事項

https://www.w3.org/WAI/ARIA/apg/patterns/spinbutton/

- `textbox`role
- 様々な format
- spin button

## いくつかピックアップ

### `textbox`role

`input`の`type`を`number`ではなくて`text`にしているので、`spinbutton`role にはなりません。これは後述する色々なフォーマットに対応するためです。

https://github.com/adobe/react-spectrum/blob/b3a4d6c1134aae882aa1dcfce64efba1d8f4308d/packages/%40react-aria/numberfield/src/useNumberField.ts#L212

### 様々な format

format するために stately 必要だよねとか
`aria-roledescription`

### spin button

> Determine the label for the increment and decrement buttons

の話とか APG とか`aria-valuenow`とか role とか input にしかフォーカスしないとか

> override the spinbutton role, we can't focus a spin button with VO

VO=Voice Over

## その他

## 疑問点

## まとめ

明日は の話です。お楽しみにー
