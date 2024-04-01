---
layout: post
title: "SliceMK ErgoDox Wireless 鍵盤"
tags: [keyboard]
redirect_from: /2023/11/12/slicemk-ergodox-wireless.html
---

之前跟團組了一把 [ErgoDoxTW] 之後，一直想找有沒有無線的版本，但是一直沒有找到，本來有往 [Sofle] 或 [Corne] 去找，不過沒有找到特別合適的賣家，很多都是要買一堆零件和 PCB 自己焊，代組好的又有點超出預算，中間也有想過要不要買 [nice!nano] 之類的主控來換掉原本 [ErgoDoxTW] 上面的 Teensy ，不過對我這個硬體新手來說還是太難了，所以一直沒有下手。

最近又搜尋了一下發現有 [SliceMK] 這個賣家，看起來原本是在做小量的團購生產，然後多的就放在網站上賣？（我也不是很確定）不過看起來滿符合我的需求的：只需要自己裝軸（支援熱插拔）和鍵帽、可以自己從 zmk 編譯 firmware （不太喜歡透過特定軟體或一定要透過特定的網頁來設定）、鍵盤之間和鍵盤與電腦之間都是無線的。

最後決定買了 SliceMK ErgoDox Wireless Lite ，關於各種版本之間的差異可以參考 [官方說明](https://www.slicemk.com/pages/ergodox-wireless) 。

剛拿到的樣子：

![](/assets/images/2023-11-12/before-front.jpeg)

背面還沒撕開壓克力膜的樣子，紅色的燈是 LED ：

![](/assets/images/2023-11-12/before-back-led.jpeg)

收到貨的時候，鍵盤的預設 firmware 是測試用 firmware ，在任一個鍵按下的時候會亮紅燈，可以方便的確認軸有沒有裝好，之後裝完需要把 firmware 重刷成正常的。

裝好軸和鍵帽之後的樣子：

![](/assets/images/2023-11-12/after-switch.jpeg)

![](/assets/images/2023-11-12/after-keycap.jpeg)

背面：

![](/assets/images/2023-11-12/after-back.jpeg)

官方在 [這頁](https://www.slicemk.com/pages/split-dongle) 說明了為什麼強烈建議大家搭配接收器使用，根據我的理解，在使用接收器的時候，兩邊的鍵盤都只需要把按鍵資訊傳給接收器，接收器處理完按鍵資訊之後，可以根據需求輸出 USB 鍵盤訊號，或是藍牙鍵盤訊號，這樣可以大大降低鍵盤的功耗，延長鍵盤充一次電的使用時間。

不過我覺得還有兩個很實用的優點官方沒有提到：

- 切換藍牙和 USB 的時候速度超快！我是把接收器插在我的 PC 上，然後用藍牙連接我的筆電，但因為接收器基本上是隨時和所有藍牙裝置連接的，差別只是在使用 USB 模式的時候完全不會送出藍牙鍵盤訊號，所以只要設好切換藍牙裝置的快速鍵，切換的時候完全沒有延遲，不需要等鍵盤重新連上筆電的藍牙！

- 因為是接收器在處理按鍵訊號，所以 key mapping 都是存在接收器上的，要刷 firmware 也只需要刷接收器上的就好，因為可以設定快速鍵讓接收器進入 bootloader 模式，刷完 firmware 接收器也會自己重啟，讓整個更新 key mapping 的流程變得很順暢！

官方有提供收到鍵盤之後的第一次設定流程（在 [這裡](https://www.slicemk.com/pages/ergodox-wireless-guide) ），講的很清楚，推薦有買的拿到都先看一下。

測試完、刷完預設的鍵盤 firmware 之後就可以來改 key mapping 了，官方提供了設定網頁 [SliceMK Configurator](https://config.slicemk.com/ergodox/) ，也提供了 fork 的 zmk 在 [slicemk/zmk](https://github.com/slicemk/zmk) ，環境設定的話可以參考 zmk 的 [Toolchain Setup](https://zmk.dev/docs/development/setup) ，官方文件滿清楚的，印象中沒有遇到什麼問題。

不過從 qmk 換到 zmk 我有遇到一些問題和大家分享：

- qmk 和 zmk 的 keycode 命名不盡相同，如果想看完整的列表可以直接看 [keys.h](https://github.com/zmkfirmware/zmk/blob/main/app/include/dt-bindings/zmk/keys.h) 。

- zmk 裡沒有像是 `QK_GRAVE_ESCAPE` 的 keycode 可以直接用，不過可以參考 [Mod-Morph](https://zmk.dev/docs/behaviors/mod-morph) 來處理，官方的舉例就是用 `grave_escape` ，貼心！

- zmk 裡也沒有 `LM` 這個切換 layer 的 function 可以直接用，不過一樣，可以參考 [Macros](https://zmk.dev/docs/behaviors/macros) ，官方也有舉了 `&lm` 的例子，再次貼心！

用了一陣子，目前想到希望能新增的功能有幾個：

- 能夠回報鍵盤目前剩餘電量，在 SlickMK 的 Discord 裡有看到有相關討論，可能等 [feat(battery): Split battery reporting over BLE GATT](https://github.com/zmkfirmware/zmk/pull/1243) 這個 zmk 的 pull request 被 merge 之後就有機會了？

- 能夠用 LED 顯示目前 layer 的狀態，這個應該是可以直接透過目前的 zmk 來達成，不過還沒研究怎麼做。

我也 fork 了一份 zmk 來放 [我自己的 keymap](https://github.com/dm4/zmk/blob/dm4/app/boards/shields/slicemk_ergodox/slicemk_ergodox.keymap) ，目前也還在持續修改中，有興趣的可以參考看看。用到現在還是覺得很滿意，推薦大家如果有興趣使用無線的 ErgoDox 的話可以考慮看看！

[ErgodoxTW]: https://www.ergokb.tw/products/ergodoxtw/
[Sofle]: https://github.com/josefadamcik/SofleKeyboard
[Corne]: https://github.com/foostan/crkbd
[nice!nano]: https://nicekeyboards.com/nice-nano
[SliceMK]: https://www.slicemk.com
