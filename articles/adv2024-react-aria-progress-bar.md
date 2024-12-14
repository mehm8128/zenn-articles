---
title: "ProgressBarについて - React Ariaの実装読むぞ"
emoji: "🌀"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

:::message
この記事は [React Aria の実装読むぞ - Qiita Advent Calendar 2024](https://qiita.com/advent-calendar/2024/react-aria) の 19 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は ProgressBar について書いていきます。

https://react-spectrum.adobe.com/react-aria/useProgressBar.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx
function ProgressBar(props) {
  let {
    label,
    showValueLabel = !!label,
    value,
    minValue = 0,
    maxValue = 100,
  } = props;
  let { progressBarProps, labelProps } = useProgressBar(props);

  // Calculate the width of the progress bar as a percentage
  let percentage = (value - minValue) / (maxValue - minValue);
  let barWidth = `${Math.round(percentage * 100)}%`;

  return (
    <div {...progressBarProps} style={{ width: 200 }}>
      <div
        style={{
          display: "flex",
          justifyContent: "space-between",
        }}
      >
        {label && <span {...labelProps}>{label}</span>}
        {showValueLabel && <span>{progressBarProps["aria-valuetext"]}</span>}
      </div>
      <div style={{ height: 10, background: "lightgray" }}>
        <div
          style={{
            width: barWidth,
            height: 10,
            background: "orange",
          }}
        />
      </div>
    </div>
  );
}
```

## 本題

WAI-ARIA はこちらです。
https://www.w3.org/TR/wai-aria-1.2/#progressbar

### `aria-`属性

React Aria の実装では 4 つの`aria-`属性がつけられています。

https://github.com/adobe/react-spectrum/blob/5ed06068ee2742f32e066ffa8eb55fd93a083123/packages/%40react-aria/progress/src/useProgressBar.ts#L63-L66

`aria-valuemin`と`aria-valuemax`は最小値と最大値、`aria-valuenow`は現在どこまで進んでいるかを表す値です。また、`aria-valuetext`はスクリーンリーダーに読み上げさせるテキストで、`progressbar`role はこれが設定されていれば`aria-valuenow`の代わりにこれが読み上げられます。今回の hook では i18n 対応も行われているようです。

https://github.com/adobe/react-spectrum/blob/5ed06068ee2742f32e066ffa8eb55fd93a083123/packages/%40react-aria/progress/src/useProgressBar.ts#L53

ただし、現在の進捗状況が不明（indeterminate）なときには`isIndeterminate`を渡すと`aria-valuenow`と`aria-valuetext`が`undefined`になり、NVDA では`ビジーインジケーター`と読み上げられます。

### `aria-`属性 2

`useProgressBar`自体にはついていないですが説明したい`aria-`属性がまだあります。
ProgressBar がページ内のどこかがローディング中であることを表している場合、そのローディング中の要素で`aria-describedby`によって ProgressBar を参照し、さらに`aria-busy`を`true`にする必要があります。

`aria-busy="true"`は、要素の更新中に live region の通知が行われるのを防ぐためのものらしいです。

> the global `aria-busy` state indicates an element is being modified and that assistive technologies may want to wait until the changes are complete before informing the user about the update.

https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-busy

Cybozu Inside Out でも関連記事を見つけたので貼っておきます。

https://blog.cybozu.io/entry/2023/12/01/080000#Poca11y-%E3%83%81%E3%83%BC%E3%83%A0%E3%81%A8%E3%81%AE%E5%8D%94%E5%8A%9B

### button の`isPending`について

progressbar は、実は 2 日目の記事でも登場していました。

https://zenn.dev/mehm8128/articles/adv2024-react-aria-button#ispending%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6

## まとめ

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、Combobox についての記事です。お楽しみにー
