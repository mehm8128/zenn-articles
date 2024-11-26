---
title: "Toastについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は Toast について書いていきます。

https://react-spectrum.adobe.com/react-aria/useToast.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx
function ToastProvider({ children, ...props }) {
  let state = useToastState({
    maxVisibleToasts: 5,
  });

  return (
    <>
      {children(state)}
      {state.visibleToasts.length > 0 && (
        <ToastRegion {...props} state={state} />
      )}
    </>
  );
}

function ToastRegion<T extends React.ReactNode>({
  state,
  ...props
}: ToastRegionProps<T>) {
  let ref = React.useRef(null);
  let { regionProps } = useToastRegion(props, state, ref);

  return (
    <div {...regionProps} ref={ref} className="toast-region">
      {state.visibleToasts.map((toast) => (
        <Toast key={toast.key} toast={toast} state={state} />
      ))}
    </div>
  );
}
```

## 主な a11y 考慮事項

https://w3c.github.io/aria/#alert

- `alert`role
- live region
- キーボード操作

## いくつかピックアップ

### role について

React Aria ではどんなトーストでも`alert`role になり、さらにそれを`alertdialog`のコンテナーで囲うような形になっているのですが、これは実装がよくないと思っています。
以下の画像が、ドキュメントのページでトーストを表示したときの DOM の画像です。

![](/images/adv2024-react-aria/toast-role.png)

MDN の`alertdialog`のページでは以下のような記載があります。

> alertdialog ロールは、ユーザーの即時の注意を要する緊急情報をユーザーに通知するために使用されます。

> その緊急性のために、アラートダイアログは常にモーダルでなければなりません。

https://developer.mozilla.org/ja/docs/Web/Accessibility/ARIA/Roles/alertdialog_role

また、WAI-ARIA にも同様のことが書かれています。

> Content authors SHOULD make alert dialogs modal by ensuring that, while the alertdialog is shown, keyboard and mouse interactions only operate within the dialog.

https://www.w3.org/TR/wai-aria-1.1/#alertdialog

MDN を見ると`alert`role も同様に、緊急度の高い場合のみ使うべきということが書かれています。

https://developer.mozilla.org/ja/docs/Web/Accessibility/ARIA/Roles/alert_role

しかし、`useToast`で表示するようなトーストは必ずしも緊急を要する情報であることは想定されていないですし、少なくともドキュメントのページのデモではモーダルにもなっていません。よって、代わりに`status`role を利用するべきだと考えています。

Chakra-UI で用いられている zag-ui だとここで role が`status`になっています。
https://github.com/chakra-ui/zag/blob/main/packages/machines/toast/src/toast.connect.ts#L59

Vercel のデザインシステムも、このページでトーストを表示してみると`status`role であることが分かります。

https://vercel.com/geist/toast

他にも調べたのがちょっと前なのでどのデザインシステムだったか忘れてしまったのですが、error toast だけ`alert`role、それ以外は`status`role、と分けているデザインシステムもありました。

これについては issue を立ててみたのですが、まだ反応がありません。

https://github.com/adobe/react-spectrum/issues/7318

とはいえ`useToast`はまだ beta 版なので、気長に待つか、PR を立てるなりしてもよさそうです。

### live region

`alert`role を使うと、暗黙的に`aria-live="assertive`がつくので更新が通知されます。`polite`とは違い、現在のタスクを中断してでも緊急で情報を伝えてきます。

### キーボード操作

`useToastRegion`でトースト全体のコンテナーに`region`role をつけて landmark にしていて、そこで使っている`useLandmark`によって`Shift + F6`でトーストにフォーカスできるようになっています。

`F6`がどこからきているのか分からなかったのですが、discussion にありました。

https://github.com/adobe/react-spectrum/discussions/6686

しかし、`F6`を用いて操作できることをスクリーンリーダー利用者や日常的にキーボード操作をする人が知ってるのかどうかは微妙な気がしているので、他に何か情報があれば教えていただきたいです。

## まとめ

明日は の話です。お楽しみにー
