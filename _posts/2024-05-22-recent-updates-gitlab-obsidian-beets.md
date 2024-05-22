---
layout: post
title: "最近的一些更新： GitLab, Obsidian, Beets"
---

## GitLab

最後還是把 GitLab 在自己機器上架起來了，在看文件設定 GitLab 的時候有一種夢回 Skymizer 的感覺，總覺得在 2024 年的現在，用 docker 跑 GitLab 好像容易了一點？

## Obsidian

GitLab 跑起來之後順手把 [obsidian-git](https://github.com/denolehov/obsidian-git) 也設定好，總算可以安心的同步筆記了。

說到筆記，最近才後知後覺的發現原來 Obsidian 是可以設定 `儲存新建筆記的資料夾` ，弄個 draft 或 inbox 之類的資料夾來放那些還沒整理好的筆記，有效減輕開新筆記時的心理負擔，推薦給大家。

時隔多年，這幾個星期又重新開始用了 Obsidian 內建的 daily notes ，來看這次能撐多久，另外發現 [Day Planner](https://github.com/ivan-lednev/obsidian-day-planner) 功能越來越多也一起裝回來了，現在竟然還能顯示 Google Calendar ，深感佩服。

## Beets

一直想好好整理音樂檔案很久了，直到最近才開始嘗試用 [Beets](https://beets.io/) 來整理手上音樂，之前大部分的音樂都是直接用 [XLD](https://tmkk.undo.jp/xld/index_e.html) 轉成單個 flac ，然後和 cue 跟 log 一起放在 `演出者/專輯` 的資料夾裡，想著之後有空再來整理。

最近開始研究了一下發現不管是音樂檔案管理軟體，還是之預計要弄的串流軟體，對整張專輯的單個 flac 檔案都不太友善……雖然想起來滿合理的，但就是覺得切成單曲之後少了些什麼資訊（歌曲間隔之類的），不過串流時代還需要在意這些嗎？（沒錯我是在試著說服自己）

雖然之前就有用過 [MusicBrainz](https://musicbrainz.org/) ，也幫忙新增過一些資料（上面的台灣樂團資料少的可憐），但送了幾張之後就還是懶了，真的是佩服那些有毅力持續貢獻社會的人，不過我最近也幫忙新增了在 2011 年發行的 [昆蟲白 - 自然人](https://musicbrainz.org/release-group/72a90a46-c11a-448d-ba20-4aa37ec98cbd) 上去（就說少的可憐了吧，連 2011 年的專輯都沒有資料），好像也是可以佩服一下我自己？（說服自己 again ）

本來以為我大概了解 MusicBrainz 了，那上手 beets 應該很快吧？結果完全不是這麼一回事，一嘗試就被各種問題困擾：怎麼切 flac / 那 cue 還要留嗎 / import 之後東西到底是存在哪 / flac metadata 被寫了什麼 / 什麼東西會在 beets sqlite 什麼東西會進 flac metadata / 怎麼比對或幫忙上傳 [AcoustID](https://acoustid.org/) / 我的光碟機 offset 到底是多少 / 怎麼比對或上傳 AccurateRip / ...

恩，對，問題還在持續增加當中。

目前我也沒有什麼好結論，慢慢來吧，至少我在裡面開始新增了第一張專輯：

```console
$ beet ls -a
草東沒有派對 - 瓦合
```

還好只嘗試了一張專輯，不然中間我重新 import 數次應該會崩潰，如果也有人想嘗試 Beets ，我目前只有一個心得可以分享的：不要 `pip install beets` ， PyPI 上面的超～級～舊～ ，直接從 GitHub 裝吧。

結果有關 Beets 的寫了一大堆，最後和大家分享在找 Beets 心得時找到的超讚 blog ： [空栈顶](https://emptystack.top/) ，其中這兩篇和 Beets 有關：

- [再见，所有的音乐订阅](https://emptystack.top/good-bye-music-subscriptions/#Beets)
- [音乐整理博士​​或：我如何停止担忧并爱上甜菜](https://emptystack.top/dr-musicuration/)

還有另外這篇也很讚，同樣是用 Obsidian + Beancount 的我看的淚流不止：

- [计时记账记笔记](https://emptystack.top/tracking-and-note-taking/)

為了內心平靜而記錄真的是說到心坎裡，可惜 blog 好像停止更新了，相見恨晚。

---

本來想著可以每周至少寫一些東西來保持更新頻率，結果馬上就失敗，那我前兩個星期上班之外的時間在幹嘛呢？

看了一本 [奪取天下的少女](https://www.books.com.tw/products/0010971339) ，可愛的日常小說。

去看了 [Master of Master](https://thewalllivehouse.kktix.cc/events/masteromaster) ，這也是為什麼我會幫忙上傳 [昆蟲白 - 自然人](https://musicbrainz.org/release-group/72a90a46-c11a-448d-ba20-4aa37ec98cbd) 的原因，橙草什麼時候出新專輯？

崩壞：星穹鐵道 2.2 更新，主線很讚，打完只想問： 2.3 什麼時候出？

玩了幾個小時的 [九日](https://store.steampowered.com/app/1809540/) 測試版，果然老人就是要選劇情模式玩，太難我可是不玩的哦。

---

大概就這樣吧，猜猜我下一篇會隔幾天？（我自己也很想知道）
