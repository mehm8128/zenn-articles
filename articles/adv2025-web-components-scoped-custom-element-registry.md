---
title: "Scoped Custom Element Registry - Web Components a11y 1人 Advent Calendar"
emoji: ""
type: "tech"
topics: ["frontend", "webcomponents"]
published: false
---

:::message
この記事は [Web Components a11y 1 人 Advent Calendar - Qiita Advent Calendar 2025](https://qiita.com/advent-calendar/2025/web-components-a11y) の n 日目の記事です。
:::

こんにちは、フロントエンドエンジニアの mehm8128 です。

https://github.com/WICG/webcomponents/blob/gh-pages/proposals/Scoped-Custom-Element-Registries.md

用例

- npm が同じライブラリの複数バージョンをインストールすることがあり、そのときに同じ名前のコンポーネントが複数存在することになる
  - 要出典
- 同じページに複数チーム関わっているときに同じ名前の別のコンポーネントを作ってしまう
- プラグイン機能のあるアプリ
  - kintone 本体のコンポーネントと、プラグイン側で実装しているコンポーネントが同じ名前だった場合とか
- その他拡張機能や CDN でホスティングされているライブラリのコンポーネントなどで競合を発生させたくない

```js
import { OtherElement } from "./my-element.js";

export class MyElement extends HTMLElement {
  constructor() {
    this.attachShadow({ mode: "open", registry });
  }
}

const registry = new CustomElementRegistry();
registry.define("other-element", OtherElement);
```

shadow root のカプセル化の一種として、registry も shadow root 単位でカプセル化する
→ 後に、普通の要素も可能になってる：「Element should support an associated CustomElementRegistry」
→ これは、element.innerHTML で作成されるカスタム要素を registry に紐づけたいら
→

```js
const someElement = document.createElement("some-element", {
  customElementRegistry: registry,
});
someElement.innerHTML = "<other-element></other-element>";
```

とすることで、`registry` に入ってるコンストラクタで `<other-element>` を upgrade できる
https://github.com/web-platform-tests/wpt/blob/master/custom-elements/registries/Element-innerHTML.html

登場人物が 2 人いる

- custom element A: scoped な registry に登録したいカスタム要素
- custom element B: scope 化する shadow root の shadow host であるカスタム要素

- ShadowRoot にも`createElement`などが追加された。`<custom-element>`を見つけたときに親を辿っていって、そのカスタム要素が作成された「context node」を探す。つまり、`contextNode.createElement()`で custom element を作ったときの `contextNode` のこと。そこの registry を見れば constructor が取れるし、それでなかったら `HTMLElement.innerHTML` などで作ったということなので global な registry を見に行く

- DSD のときは`shadowrootcustomelementregistry`をつけることで、ShadowDOM 単位の registry にあるカスタム要素の定義を使用
  - https://github.com/web-platform-tests/wpt/blob/master/custom-elements/registries/CustomElementRegistry-initialize.html
  - https://github.com/WICG/webcomponents/issues/914

https://github.com/whatwg/html/issues/10854
https://github.com/whatwg/html/pull/10869

Webkit で Technical Preview に入ってる
https://github.com/whatwg/html/issues/10854#issuecomment-2816888199
https://github.com/WebKit/WebKit/pull/39865

http://github.com/web-platform-tests/wpt/tree/master/custom-elements/registries
https://html.spec.whatwg.org/multipage/custom-elements.html#custom-elements-api
https://html.spec.whatwg.org/multipage/scripting.html#the-template-element

- `registry.upgrade(shadowroot)`: plain text の`<custom-element>`的なものを、constructor を使ってカスタム要素として動くようにすること
- `registry.initialize(shadowroot)`で、registry が割り当てられていない shadowroot を、この registry を使うように初期化する
  - `this.attachShadow({mode: 'open', registry});`の代わりみたいな

## まとめ

明日はについて紹介します。
