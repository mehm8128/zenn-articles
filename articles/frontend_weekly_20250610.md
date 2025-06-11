---
title: "Safariの `contrast-color()` など: Cybozu Frontend Weekly (2025-06-10号)"
emoji: "🐌"
type: "idea"
topics: ["frontend", "cybozufrontendweek"]
published: false
publication_name: "cybozu_frontend"
---

こんにちは！ サイボウズ株式会社 フロントエンドエンジニアの [mehm8128 (@mehm8128)](https://x.com/mehm8128) です。

## はじめに

サイボウズ社内では毎週火曜日に Frontend Weekly と題し「一週間の間にあったフロントエンドニュースを共有する会」を開催しています。

今回は、2025/06/10 の Frontend Weekly で取り上げた記事や話題を紹介します。

## 取り上げた記事・話題

### Intent UI

https://intentui.com/

React Aria Components ベースの、 shadcn/ui っぽくコピペして使用できる UI ライブラリです。
[Recharts](https://recharts.org/en-US) を用いたグラフ系のコンポーネントが充実していそうなのが印象的でした。
また、似たようなライブラリに [JollyUI](https://www.jollyui.dev/) や [Draft UI](https://draft-ui.com/) などがあります。

## Creating a more accessible web with Aria Notify - Microsoft Edge Blog

https://blogs.windows.com/msedgedev/2025/05/05/creating-a-more-accessible-web-with-aria-notify/

DOM の変更に紐づかない UI の変更をスクリーンリーダーに通知できるように、命令的に通知する Aria Notify の紹介です。

記事で紹介されている、以下のようなサンプルコードで "Selected text is bold" という文字列を通知することができます。

```js
document.querySelector("#text-editor").ariaNotify("Selected text is bold");
```

この機能は Microsoft Edge 136 のオリジントライアル、もしくは `--enable-blink-features=AriaNotify` の feature flag を有効化することで試すことができます。

## Emotion から CSS Modules に移行しました | PR TIMES 開発者ブログ

https://developers.prtimes.jp/2025/05/09/migrate-from-emotion-to-css-modules/

PR TIMES で Emotion を CSS Modules に移行した背景や移行方法の紹介です。

Tailwind と CSS Modules との比較や、CSS の詳細度の問題など移行に伴う注意点などが挙げられています。

## Add wide gamut P3 and alpha transparency to your color picker in HTML | WebKit

https://webkit.org/blog/16900/p3-and-alpha-color-pickers/

Safari 18.4 でサポートされた `<input type="color">` の `colorspace` と `alpha` オプションの紹介です。

以下のように記述することで、DCI-P3 色空間とアルファ透明度を設定することができるようになります。

```html
<input type="color" colorspace="display-p3" alpha />
```

またこれにより、色のデフォルト値を設定する `value` 属性を様々な書き方で指定できるようになります。

```html
<input
  type="color"
  value="oklab(59% 0.1 0.1 / 0.5)"
  colorspace="display-p3"
  alpha
/>
<input
  type="color"
  value="color(display-p3 0.3737 0.2103 0.5791)"
  colorspace="display-p3"
/>
```

## feat: add react-server-dom-vite

https://github.com/facebook/react/pull/31768
https://github.com/facebook/react/pull/33152

React Server Components を Vite を動かすことができるようにする PR です。

今までは RSC が Webpack でしか動かなかったのですが、今回の PR で Vite でも動くようになるとのことです。

## How to have the browser pick a contrasting color in CSS

https://webkit.org/blog/16929/contrast-color/

渡した色に対して、黒か白のうちコントラスト比が高くなる色を選択してくれる `contrast-color()` という CSS の紹介です。

例えば以下のコードで、`purple` という色に対して黒か白のうちコントラスト比が高い方の色が文字色として反映されます。

```CSS
color: contrast-color(purple);
```

現在は WCAG2.1 のアルゴリズムに従って算出していますが、将来的に WCAG3 で議論されている [APCA (Accessible Perceptual Contrast Algorithm)](https://github.com/Myndex/SAPC-APCA) に置き換えることも可能になっているようです。

この機能は現在、Safari Technology Preview で利用できます。

## MS の大規模レイオフで rbuckton 氏が退職

https://x.com/rbuckton/status/1922364558426911039

Microsoft の rbuckton 氏が退職されたというポストです。

rbuckton 氏は [typescript-go](https://github.com/microsoft/typescript-go) の committer や、TC39 では [Explicit Resource Management](https://github.com/tc39/proposal-explicit-resource-management) の champion、[ES Enum](https://github.com/tc39/proposal-enum) の推進なども行っていました。

## tanstack/db

https://github.com/TanStack/db
ローカルの state を永続化するやつ？
electric sql（Saas）が裏にいる
オフラインでも動いて、オンラインに復活したら electric sql と同期する
sql は別に他のところでも動く

## 開発を止めない段階的フロントエンドリプレイスの実践

https://www.m3tech.blog/entry/frontend-replacement-plan

3 記事に渡る、レガシーなフレームワーク・ライブラリから React への移行記事です。

1 では基盤整備や段階的なリプレイスの方法など移行戦略の話、2 では Radix UI をベースとした digikar-ui という独自コンポーネントライブラリなど具体的な移行についての話、3 ではチーム体制や情報共有など、組織の話が書かれていました。

## Video with alpha transparency on the web

https://jakearchibald.com/2024/video-with-transparency/

Airbnb が開発した新しいメディアフォーマット「Lava」の解説です。

透明度付き動画をサポートし、Web とモバイルの両方で動く軽量なフォーマットとのことです。
透明度付き動画フォーマットの既存の問題点を説明し、それに対する解決策が解説されています。

## CSS Function and Mixins Module

https://www.w3.org/TR/2025/WD-css-mixins-1-20250515/

`@function()` と `@mixin()` の標準化に関する、 First Public Working Draft です。

## まとめ

React Aria Components ベースの UI ライブラリや AriaNotify、`contrast-color()` などアクセシビリティ周りの話題が充実していました。今後が楽しみです。
