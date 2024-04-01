---
layout: post
title: "macOS 裡的刪節號"
redirect_from: /2024/01/08/ellipsis-in-macos.html
---

前陣子在試著加 macOS 的快速鍵設定的時候，發現像訊息選單裡 **封鎖成員⋯** / **刪除對話⋯** 之中的刪節號一直打不出來，如圖：

![](/assets/images/2024-01-08/messages-menu.png)

查了一下才發現一般中文輸入法打出來的刪節號是 Unicode 的 `U+2026 …` ，而 macOS 選單裡的刪節號是 `U+22EF ⋯` ，不過定義上其實 `U+2026` 才應該是正確的刪節號， `U+22EF` 是被用在數學上的三個水平的點（數學不好，我也不知道它是拿來做什麼 QQ ）。

多翻了幾篇也發現破折號也有類似的問題（ `U+2014 —` 和 `U+2500 ─` ），下面是一些相關討論的連結：

- [中文標點符號差異 · Issue #12 · dpublishing/epub3guide · GitHub](https://github.com/dpublishing/epub3guide/issues/12)

- [TWOBE▶開源節流 - 一直在想為什麼iOS的刪節號「⋯⋯」跟微軟新注音打出來的刪節號「……」長得不一樣，爬了一下才知道原來iOS內建的刪節號在Unicode中被定義為數學符號的水平刪節號，而且還找不到正確的刪節號 - Plurk](https://www.plurk.com/p/nvfnrg)

- [字嗨 / ‌國外先前有一則關於「應該由哪個符號充當撇號」的議題，近期我也對臺灣的標點符號應採用何種符號產生... / Facebook](https://www.facebook.com/groups/enjoyfonts/permalink/1137907106364163/)
