---
title: "TextFieldについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は TextField について書いていきます。

https://react-spectrum.adobe.com/react-aria/useTextField.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx
function TextField(props: AriaTextFieldProps) {
  let { label } = props;
  let ref = React.useRef(null);
  let {
    labelProps,
    inputProps,
    descriptionProps,
    errorMessageProps,
    isInvalid,
    validationErrors,
  } = useTextField(props, ref);

  return (
    <div style={{ display: "flex", flexDirection: "column", width: 200 }}>
      <label {...labelProps}>{label}</label>
      <input {...inputProps} ref={ref} />
      {props.description && (
        <div {...descriptionProps} style={{ fontSize: 12 }}>
          {props.description}
        </div>
      )}
      {isInvalid && (
        <div {...errorMessageProps} style={{ color: "red", fontSize: 12 }}>
          {validationErrors.join(" ")}
        </div>
      )}
    </div>
  );
}
```

## 主な a11y 考慮事項

https://www.w3.org/TR/wai-aria-1.2/#textbox

- `textbox`role
- ラベルや説明文、エラーメッセージとの紐づけ
- 複数行入力可能な場合、`aria-multiline`を付与

## いくつかピックアップ

### フィールドの a11y とバリデーション

React Aria ではフィールド系のコンポーネントにはラベルに加えて説明文とエラーメッセージを紐づけることができます。
説明文を入れている要素には`descriptionProps`を、エラーメッセージを入れている要素には`errorMessageProps`を渡すことで、[`useField`](https://github.com/adobe/react-spectrum/blob/93c26d8bd2dfe48a815f08c58925a977b94d6fdd/packages/%40react-aria/label/src/useField.ts#L31)によって生成されている id を用いて`aria-describedby`でテキストフィールドに紐づけることができます。
また、 React Hook Form などのライブラリと一緒に使うこともでき、そのバリデーション結果のエラーメッセージを、`errorMessageProps`を渡している要素に入れることで紐づけます。

https://react-spectrum.adobe.com/react-aria/forms.html

エラーメッセージには`aria-errormessage`が使われることもありますが、`aria-errormessage`は現状スクリーンリーダーによって上手く読み上げられないため、用いられていないようです。

https://github.com/adobe/react-spectrum/issues/7425

https://a11ysupport.io/tech/aria/aria-errormessage_attribute

### `aria-multiline`

`useTextField`のドキュメントで紹介されている WAI-ARIA を見てみます。

https://www.w3.org/TR/wai-aria-1.2/#textbox

`textbox`role についてのセクションで、NOTE の欄には、1 行のテキストフィールドである`input`タグは Enter キーを押すとデフォルトではフォームが送信されるけど、複数行である`textarea`タグは改行されるだけなので、それを区別するために`aria-multiline`があるという話が書かれています。
`aria-multiline`を`true`にするとスクリーンリーダーでは「複数行」という読み上げがされるので、それによって Enter を押したときにすぐに送信されてしまうか、改行されるだけかという判断がつく、という話だと理解しました。
ただ、複数行のときにはちゃんと`textarea`タグを使っていれば自動で`aria-multiline='true'`の挙動になってくれるので、RTE を触るときなどに気にすることになりそうです。React Aria の`useTextfield`では[`input`か`textarea`タグのみをサポートしている](https://github.com/adobe/react-spectrum/blob/93c26d8bd2dfe48a815f08c58925a977b94d6fdd/packages/%40react-aria/textfield/src/useTextField.ts#L50)ので、実装に明示的に含まれてはいませんでした。

## inputMode

書く

## まとめ

明日は Number Field の話です。お楽しみにー
