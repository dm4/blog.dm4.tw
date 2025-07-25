---
layout: post
title: "小米藍牙溫濕度計 & TelinkMiFlasher"
---

最近需要在家裡多放幾個帶螢幕的溫濕度計，買的時候也不太在意是走藍牙或是 Zigbee ，反正能接進 Home Assistant 就行，最後發現好像還是小米的最便宜，一共買了三種不同的來試用看看：

* [小米藍牙溫濕度計 2 (LYWSD03MMC)](https://www.mi.com/tw/item/3202100014?skupanel=1)
* [小米藍牙溫濕度計 3 (MJWSD05MMC)](https://www.mi.com/tw/product/xiaomi-smart-temperature-and-humidity-monitor-3/?skupanel=1)
* [小米藍牙溫濕度計 3 mini (MJWSD06MMC)](https://www.mi.com/tw/product/xiaomi-smart-temperature-and-humidity-monitor-3-mini/?skupanel=1)

（是說不知道為什麼跳過了 SD04MMC ……？）

不過這幾款也不是一開始就能直接從 Home Assistant 讀到資料，剛好有大大做好了可以把原裝韌體刷掉的工具 [pvvx/ATC_MiThermometer](https://github.com/pvvx/ATC_MiThermometer) ，可以直接在瀏覽器上執行 [TelinkMiFlasher] 。

上面提到的三種設備上，只有小米藍牙溫濕度計 3 (MJWSD05MMC) 最容易設定，只要：

* 把設備重置（新買的可以不用重置）
* 在 [TelinkMiFlasher] 網頁上配對
* 連上設備後，一樣在網頁上啟用
* 在網頁上選擇預設的韌體，直接 Start Flashing

另外兩款小米藍牙溫濕度計 2 (LYWSD03MMC)、小米藍牙溫濕度計 3 mini (MJWSD06MMC)，就比較麻煩一點：

* 把設備重置（新買的可以不用重置）
* 在米家 app 先將溫濕度計配對好
* 用 [Xiaomi-cloud-tokens-extractor](https://github.com/PiotrMachowski/Xiaomi-cloud-tokens-extractor) ，拿到設備上的 ble key 跟 token
* 斷開設備和手機間的藍牙連線！
  + 如果沒斷開的話，會沒辦法讓 [TelinkMiFlasher] 和設備連線
* 在 [TelinkMiFlasher] 網頁上配對
* 連上設備後，填上剛剛的 ble key 和 token 後，點 Login
* 在網頁上選擇預設的韌體，直接 Start Flashing

我一開始忘記斷開設備和手機的藍牙連線，想說怎麼一直沒辦法用 Flasher 連線……還又重置了幾次，然後每次重置都會重新產生新的 key 和 token ……有夠麻煩。

刷完之後在 Home Assistant 的 BTHome 整合裡，應該就會看到剛剛刷好的設備，不過刷完的溫濕度計在米家就沒辦法看到了，我看 README 有寫到可以把 key 填回去，再設定一下就可以重新連到米家，不過我倒是沒有這個需求。

用了幾天一切正常，實在有夠讚，感謝各位開源大佬。

[TelinkMiFlasher]: https://pvvx.github.io/ATC_MiThermometer/TelinkMiFlasher.html
