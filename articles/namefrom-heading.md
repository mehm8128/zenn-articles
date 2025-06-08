---
title: "`nameFrom: heading`について"
emoji: "⛑️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "html", "a11y", "waiaria"]
published: false
---

こんにちは、フロントエンドエンジニアの mehm8128 です。

## nameFrom 復習

https://zenn.dev/mehm8128/articles/accessible-name-and-description-computation-1-2#add-name-from-prohibited

## nameFrom: heading とは

大元の issue: https://github.com/w3c/accname/issues/138

article などに勝手に accessible name つけたいよねみたいな
なぜなら、スクリーンリーダーを使って記事だけローターしていったときに、ちゃんと全部 aria-labelledby とかで紐づけていないと「記事」「記事」みたいな読み上げになってしまっているから
→article は landmark ではないから、NVDA では再現できなさそう？
Voice Over の rotor で、ジャンプする role を選べるので、そこで Article を選択したときの話っぽい

結局、article などのロールが、その子孫の heading ロールから acc name がつけられる
table 内の caption が table を name するんだから良くない？みたいな

https://github.com/w3c/aria/pull/1860

自動でつけすぎても、landmark が増えて冗長になるのでよくない
結局以下の 3 つの role についた

- alertdialog
- article
- dialog

complementary と region は、aside 要素が場合によって適切な role が違うみたいなやつで消えた
aside 要素が実際に動的に role が変わるのかは確認してみる必要がある＆別で issue 開くらしいので、あるかどうか確認
→ なさそう

https://blog.w0s.jp/entry/669
https://github.com/w3c/aria-practices/issues/1469
https://www.w3.org/WAI/ARIA/apg/patterns/landmarks/examples/complementary.html

### DFS vs IDS

https://github.com/w3c/aria/pull/1018
https://github.com/w3c/accname/issues/182
https://www.w3.org/2024/03/07-aria-minutes.html#t06

DFS だと、パフォーマンスが問題になる可能性があるけど、大体の場合で良さげなのが取れる
IDS だと、幅優先探索みたいな感じになる。パフォーマンスが良さそうだけど、予想外のものが取れてしまう可能性がある

DFS になった

## accname

accname の計算方法書いてあるやつへの追記と、上で挙げた探索方法の議論
https://github.com/w3c/accname/issues/182
https://github.com/w3c/aria/pull/2209

## wpt

テストの追加
https://github.com/web-platform-tests/interop-accessibility/issues/47
https://github.com/web-platform-tests/wpt/pull/50507
https://wpt.fyi/results/accname/name/comp_name_from_heading.tentative.html

## webkit の実装

4/30 にマージされた
https://webkit.org/b/257186

`Source/WebCore/accessibility/AccessibilityNodeObject.cpp`
が本質部分
`descendantsOfType`で descendants を全部取って順番に見ていくので、DFS pre-order traversal になっているはず
`descendantsOfType`の本質部分は見つけられなかった

## sectionheader と sectionfooter

https://github.com/w3c/aria/issues/1915
https://github.com/w3c/aria/pull/1931

sectionheader と sectionfooter ほしいよねって話
現在は section 系の要素の中にある header, footer は role を持たなくて、ただの`<div>`と同じような感じだけど、article とかの header, footer も持つようにしたいという話
landmark ではない。もし landmark なら、5.3.4 Landmark Roles に入る

https://github.com/w3c/html-aam/issues/585
https://github.com/w3c/aria/pull/2543

`<main>`じゃなくて`<div role="main">`だった場合も同様に sectionheader になる？という話
https://github.com/w3c/html-aam/issues/586

### wpt

https://github.com/w3c/aria/issues/2295
https://github.com/web-platform-tests/interop-accessibility/issues/136
https://github.com/web-platform-tests/wpt/pull/45916

### 実装

webkit: ちょうど進行中
`Source/WebCore/accessibility/AccessibilityNodeObject.cpp`が本質部分
471 行目間違えてるけどレビュー書いていいのか分からん（Footer→Header）
https://github.com/WebKit/WebKit/pull/46361

blink:マージ済み
https://issues.chromium.org/issues/337094897
既に`kHeaderAsNonLandmark `と`kFooterAsNonLandmark`があったからそれの名前を変えるだけでよかったらしい

## まとめ
