---
layout: post
title: "Command Line 小知識"
date: 2013-01-27 22:35
comments: true
categories: [linux, unix, cli]
---

前幾天看了這篇 [To understand the command line...](http://geekblog.oneandoneis2.org/index.php/2012/09/30/to-understand-the-command-line) 覺得蠻有趣的，大概是在說：

- `awk`: 作者名字的縮寫
- `cat`: concatenate
- `more`: 螢幕最下面會顯示 "More"
- `less`: more 的加強版，因為 "less is more" 所以叫 less
- `ed`: editor
- `sed`: stream editor
- `grep`: 在 `ed` 裡要找有包含特定字串的行並印出，本來是要用 `g/regular expression/p` ，簡寫成 `g/re/p` 就變成 `grep` 了
- hjkl 方向鍵是因為 [ADM-3A](http://www.catonmat.net/blog/why-vim-uses-hjkl-as-arrow-keys/)
- `~` 代表 HOME 也是因為 [ADM-3A](http://www.catonmat.net/blog/why-vim-uses-hjkl-as-arrow-keys/)
- `.` 開頭的檔案是隱藏檔，本來只是要跳過 `.` 和 `..` ，結果寫出有 bug 的 `if (name[0] == '.') continue;`
