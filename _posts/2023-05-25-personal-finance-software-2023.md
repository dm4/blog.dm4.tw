---
layout: post
title: "用過的記帳軟體們 - 2023 版"
tags: [software]
---

結果距離[上一篇]({% link _posts/2015-10-11-personal-finance-softwares.md %})已經過了 8 年，
中間又換過一次記帳軟體，剛好再來記錄一下（順便複習一下應該怎麼發文……）。

## [Beancount]

MoneyWiz 用到後來越變越慢，我猜可能是資料量太大了，不過一時間也找不到什麼方便的替代方案，
後來應該是看到 [Beancount — 純文字記帳工具 (1)](https://yurenju.medium.com/beancount-part-1-20eb4833bb84) 才知道 beancount 的，
雖然完全不知道複式記帳是什麼，不過越看越不錯，既是開源軟體資料格式又是純文字，哪天要搬家好像也滿方便的，在 2020 下半年就開始試用了。

本來是有想過都是純文字的話，應該可以把之前 MoneyWiz 的記錄都匯出一份，然後轉成 beancount 的格式，後來想想覺得好懶而作罷……

因為沒找到什麼好用的 iOS / macOS 軟體（前一篇的時候還是寫 OS X ，真是時代的眼淚），決定弄個簡單的 Telegram bot 讓我在手機或電腦上都能記帳，
然後架個 [fava] 來看記錄，接著想說弄一下 fava 的權限控制好了，就加了 HTTP basic authentication ，又覺得常常 session expire 要重打密碼好麻煩，
於是又試了 client certificate ，結果發現 iOS 上的 Chrome 竟然沒辦法用 client certificate ……

用一陣子之後又想把信用卡帳單、對帳單什麼的加到 beancount documents 裡面，於是用了 [Hazel] 來自動歸檔和寫入 beancount 記錄，
後來覺得每次看對帳單都要打密碼好麻煩，又在流程裡加了 [qpdf] 來去掉 pdf 密碼，
隨著 Hazel 規則越來越多，又大整理一次，把所有 Hazel 規則都寫在同一個 script 裡面，
某次又覺得 [BQL] 好難用，做不到想要的功能，於是又加了一些想要的 BQL functions 。

改來改去覺得開源軟體加上純文字格式真的是很讚，用 VS Code 的時候還有 formatter 可以用，加上 Copilot 之後更是驚為天人，真的是推薦大家都來試試看。

[beancount]: https://beancount.github.io
[fava]: https://beancount.github.io/fava/
[hazel]: https://www.noodlesoft.com
[qpdf]: https://qpdf.readthedocs.io/
[bql]: https://beancount.github.io/docs/beancount_query_language.html
