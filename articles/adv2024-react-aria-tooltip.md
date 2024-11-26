---
title: "Tooltipについて - React Ariaの実装読むぞ"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は Tooltip について書いていきます。

https://react-spectrum.adobe.com/react-aria/useTooltipTrigger.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx
function Tooltip({ state, ...props }) {
  let { tooltipProps } = useTooltip(props, state);

  return (
    <span
      style={{
        position: "absolute",
        left: "5px",
        top: "100%",
        maxWidth: 150,
        marginTop: "10px",
        backgroundColor: "white",
        color: "black",
        padding: "5px",
        border: "1px solid gray",
      }}
      {...mergeProps(props, tooltipProps)}
    >
      {props.children}
    </span>
  );
}

function TooltipButton(props) {
  let state = useTooltipTriggerState(props);
  let ref = React.useRef(null);

  // Get props for the trigger and its tooltip
  let { triggerProps, tooltipProps } = useTooltipTrigger(props, state, ref);

  return (
    <span style={{ position: "relative" }}>
      <button
        ref={ref}
        {...triggerProps}
        style={{ fontSize: 18 }}
        onClick={() => alert("Pressed button")}
      >
        {props.children}
      </button>
      {state.isOpen && (
        <Tooltip state={state} {...tooltipProps}>
          {props.tooltip}
        </Tooltip>
      )}
    </span>
  );
}
```

## 主な a11y 考慮事項

https://www.w3.org/TR/wai-aria-1.2/#tooltip

- `tooltip`role
- `aria-describedby`
- キーボード操作

## いくつかピックアップ

### スクリーンリーダー読み上げ

WAI-ARIA を読んでみると、tooltip が表示されるタイミングで`aria-describedby`によって tooltip が参照されるべきと書いてあります。

https://www.w3.org/TR/wai-aria-1.2/#tooltip

React Aria では`isOpen`のときに`triggerProps`の`aria-describedby`に`tooltipId`を渡しています。これによって、trigger ボタンの accessible description に tooltip の中身が入り、キーボードでフォーカスして tooltip が表示されたときに、ちゃんと tooltip の中身が読み上げられます。

https://github.com/adobe/react-spectrum/blob/5ed06068ee2742f32e066ffa8eb55fd93a083123/packages/%40react-aria/tooltip/src/useTooltipTrigger.ts#L140

例えば Example の 1 つ目の ✏️ ボタンだと

```
クリック可能 ✏️ ボタン Edit
```

というように読み上げられます。最後の「Edit」が tooltip の中身です。

### キーボード操作

フォーカス中に tooltip が表示され、Esc キーで tooltip が閉じられるようになっています。

## その他

### tooltip 自体へのホバー

トリガーとなるボタンからホバーが外れたとしても、tooltip 自体をホバーしていれば消えないようになっています。
[Web アプリケーションアクセシビリティ本](https://amzn.asia/d/erlHCpO)では、「画面拡大時に tooltip が画面外にはみ出てしまったときでも、tooltip をホバーしながら画面移動することで tooltip を表示したまま内容を全て読めるようにするため」にこの機能が必要だと書かれています。
また、tooltip とトリガーとなるボタンとの間に隙間があるときにカーソルを移動させると tooltip が消えてしまわないように、tooltip が消えるまで数秒の遅延を挟んでいます。

## 疑問点

`useButton`ではモバイルでも`isPressing`が対応されていましたが、今回はモバイルで触っても tooltip が表示されないのがちょっと気になりました。
ただ、ボタンと違って tooltip は基本的には補助的なコンテンツに用いるものなので、そんなに問題ないのかなと思いました。

## まとめ

明日は Number Field の話です。お楽しみにー
