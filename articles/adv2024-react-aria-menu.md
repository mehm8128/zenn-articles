---
title: "Menuについて - React Ariaの実装読むぞ"
emoji: "📖"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "react", "a11y", "reactaria"]
published: false
---

:::message
この記事は [React Aria の実装読むぞ - Qiita Advent Calendar 2024](https://qiita.com/advent-calendar/2024/react-aria) の 12 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。
今日は Menu について書いていきます。ついに半分です。

https://react-spectrum.adobe.com/react-aria/useMenu.html

## 使用例

ドキュメントからそのまま取ってきています。

```tsx
function MenuButton<T extends object>(props: MenuButtonProps<T>) {
  // Create state based on the incoming props
  let state = useMenuTriggerState(props);

  // Get props for the button and menu elements
  let ref = React.useRef(null);
  let { menuTriggerProps, menuProps } = useMenuTrigger<T>({}, state, ref);

  return (
    <>
      <Button
        {...menuTriggerProps}
        buttonRef={ref}
        style={{ height: 30, fontSize: 14 }}
      >
        {props.label}
        <span aria-hidden="true" style={{ paddingLeft: 5 }}>
          ▼
        </span>
      </Button>
      {state.isOpen && (
        <Popover state={state} triggerRef={ref} placement="bottom start">
          <Menu {...props} {...menuProps} />
        </Popover>
      )}
    </>
  );
}
```

## 本題

WAI-ARIA はこちらです。
https://www.w3.org/WAI/ARIA/apg/patterns/menubar/

### サブメニュー

`useSubmenuTrigger`などを利用することで、メニューアイテムをホバーしてさらに子のメニューを表示できるようになります。これについて、フォーカス移動と`useSafelyMouseToSubmenu`の 2 点に着目して詳細を見ていこうと思います。

ただし、APG ではサブメニューは避けることが推奨されていました。

> Although it is recommended that authors avoid doing so, some implementations of navigation menubars may have menuitem elements that both perform a function and open a submenu.

### フォーカス移動

サブメニューのトリガーにフォーカスしている場合、普通のメニューアイテムと同じく`Enter`などでもサブメニューを展開できるし、`→`キー（水平メニューの場合`↓`キー）でも展開できるようになっています。

https://github.com/adobe/react-spectrum/blob/993de98adad65e48bcebad8ac835f5c9e0c94c85/packages/%40react-aria/menu/src/useSubmenuTrigger.ts#L132-L176

逆に、サブメニューにフォーカスしているときに、開いたときと逆の矢印キーを押すとサブメニューを閉じることができます。

https://github.com/adobe/react-spectrum/blob/993de98adad65e48bcebad8ac835f5c9e0c94c85/packages/%40react-aria/menu/src/useSubmenuTrigger.ts#L98-L119

### `useSafelyMouseToSubmenu`

サブメニューのトリガーにホバーしたときにサブメニューが表示されますが、トリガーからサブメニューのアイテムに移動するまでに、サブメニューではない部分をマウスカーソルが通過することがあります。その際にサブメニューが閉じてしまわないように、React Aria では工夫がされています。この工夫は以下のブログ記事に記載されているので、簡単にまとめます。

https://react-spectrum.adobe.com/blog/creating-a-pointer-friendly-submenu-experience.html

React Aria では、トリガー起動時のカーソル位置と、サブメニューの上端と下端（のトリガー起動時のカーソル位置側の各頂点）の 3 点を結ぶ三角形のエリアを想定し、この中をカーソルが移動しているときにはサブメニューを閉じないようにしています（ただ、記事内で書かれているようにこのエリア内でも一定時間カーソル移動がなかった場合には閉じます）。
この判定には[Math.atan2() - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Math/atan2)という関数が用いられています。これは[`tan`](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Math/tan)の逆関数である[`atan`](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Math/atan)を少し改善したものになっていて、違いは上記の MDN か、記事内でも参照されている[atan2 - Wikipedia](https://ja.wikipedia.org/wiki/Atan2)をご覧ください。

$\operatorname{atan2}$を用いて、以下の画像で緑（水平方向）の線を$x$、紫（垂直方向）の線を$y$として、この 2 つから赤く（三角形の内側に）印をつけた角度$\theta$を求めます（ここから先、特に数学の部分などはあまり自信がないので間違いがありましたら教えていただきたいです）。
![三角形を形成する各点に対して、角度の求め方が記された図](/images/adv2024-react-aria/menu-submenu-calc.png)

例えば$\theta_{top}$だと
$x_{top} = 緑（水平方向）の線の長さ$
$y_{top} = 紫（垂直方向）の線の長さ$
$\theta_{top} = \operatorname{atan2}\bigg(\dfrac{y_{top}}{x_{top}} \bigg)$
で求めることができます。

`atan2`を用いて、それぞれの角度（$\theta_{top}$, $\theta_{bottom}$, $\theta_{pointer}$）を導出することで、$\theta_{top} \gt \theta_{pointer} \gt \theta_{bottom}$が満たされている場合にカーソルが三角形のエリア内に含まれていると判別することができるようになっています。
なお、今回の図の場合$\theta_{bottom}$と$\theta_{pointer}$は負の角度が導出されます。

実装はここらへんです。
https://github.com/adobe/react-spectrum/blob/993de98adad65e48bcebad8ac835f5c9e0c94c85/packages/%40react-aria/menu/src/useSafelyMouseToSubmenu.ts#L107-L120

また、Floating UI というライブラリでも同じように三角形のエリアを考えた、ユーザー体験の向上が行われているようです。
https://floating-ui.com/docs/useHover#safepolygon

実装はここらへんにありそうでした。
https://github.com/floating-ui/floating-ui/blob/master/packages/react/src/safePolygon.ts

## まとめ

明日の担当は [@mehm8128](https://zenn.dev/mehm8128) さんで、 Combobox についての記事です。お楽しみにー
