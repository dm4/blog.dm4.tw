---
layout: post
title: "用 Scriptable 做個 iOS Widget"
---

用 [WaterMinder](https://waterminder.com/) 來記錄自己每天喝水一段時間了，整體用起來滿讚的：可以預設很多種水量、飲料類別，和 HealthKit 的同步也很穩定，不管從 WaterMinder 或健康 app 增減喝水資料都能正常同步， iOS 上的 widget 也很好用！

不過最近大概一兩個月會發生一次 widget 沒辦法正常顯示目前喝水量，再加上也開始想把目前喝水量顯示在其他的地方（像是自己的 [MagicMirror²](https://magicmirror.builders/) 上），開始有點想自己簡單的重建整個流程，因為我主要都是從 widget 來記錄的，決定用直接用 [Scriptable] 來簡單刻個 widget 。

最一開始知道 [Scriptable] 應該是在用 [iOS 原神桌面小工具](https://forum.gamer.com.tw/C.php?bsn=36730&snA=26628) 的時候，就是個讓人可以簡單做出 iOS widget 的小工具，除了主畫面的 widget 之外，也可以自訂 iOS 16 之後才有的鎖定畫面上的 widget 。

預計的做法是這樣：

- 照著 [WaterMinder](https://waterminder.com/) 的 widget ，用 [Scriptable] 刻出一個差不多的
- 點了 widget 上的按鈕之後會觸發特定的 iOS 捷徑，帶入水量當作參數
- 捷徑會把水量呼叫自訂的 HTTP API ，存在自己的伺服器上
- HTTP API 成功之後，捷徑會把水量寫進 iOS HealthKit 裡，方便直接在健康 app 裡查看
- 之後再更新 [Scriptable] widget ，讓它顯示新的水量

整個接好之後大概會長這樣：

{% include image.md name="demo.gif" %}

同時也會有個通知 & 更新鎖定畫面上面的 inline widget

{% include image.md name="lock.png" %}

---

在這個大家都在用 vibe coding 方便的生出厲害的 iOS app 的時代，研究這種舊時代的工具不免感覺有點落後（還是不思進取？）……不過用 Cursor 幫忙刻 widget 畫面還是挺方便的 😃

[Scriptable]: https://scriptable.app/
