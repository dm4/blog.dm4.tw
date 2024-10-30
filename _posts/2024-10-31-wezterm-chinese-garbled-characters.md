---
layout: post
title: "WezTerm 中文亂碼"
---

在升級 macOS 到 Sequoia 之後，過了一陣子才發現 WezTerm 裡的中文，有些字會變成亂碼，查了一下有翻到 [WezTerm failed to render partial Chinese character](https://github.com/wez/wezterm/issues/6045) 這個 issue ，看起來是因為升級之後預設的中文字體並不會自動安裝。

解決方法也很簡單，在 `應用程式 → 字體簿` 裡搜尋 `蘋方` ，安裝之後重開 WezTerm 就好了！

{% include image.md name="font-book.png" %}
