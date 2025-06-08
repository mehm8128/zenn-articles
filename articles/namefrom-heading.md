---
title: "`nameFrom: heading`について"
emoji: "⛑️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["frontend", "html", "a11y", "waiaria"]
published: true
---

こんにちは、フロントエンドエンジニアの mehm8128 です。

## nameFrom 復習

https://zenn.dev/mehm8128/articles/accessible-name-and-description-computation-1-2#add-name-from-prohibited

## nameFrom: heading とは

大元の issue: https://github.com/w3c/accname/issues/138

一回閉じられちゃった PR: https://github.com/w3c/aria/pull/1018

## accname

https://github.com/w3c/accname/issues/182
https://github.com/w3c/accname/pull/229
https://github.com/w3c/aria/pull/2209

## wpt

https://github.com/web-platform-tests/interop-accessibility/issues/47
https://github.com/web-platform-tests/wpt/pull/44490
https://github.com/web-platform-tests/wpt/pull/50507
https://wpt.fyi/results/accname/name/comp_name_from_heading.tentative.html

## webkit の実装

4/30 にマージされた
https://webkit.org/b/257186

gecko と blink でなんか議論あれば
https://bugzilla.mozilla.org/show_bug.cgi?id=1834463
https://bugs.chromium.org/p/chromium/issues/detail?id=1447981
