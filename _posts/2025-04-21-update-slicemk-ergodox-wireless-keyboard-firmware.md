---
layout: post
title: "更新 SliceMK ErgoDox Wireless 鍵盤 Firmware"
---

從上次提到開始用 [SliceMK ErgoDox Wireless 鍵盤](/slicemk-ergodox-wireless) 之後，在家裡幾乎就沒在換過鍵盤了，無線的分離式鍵盤實在是值得推薦，加上主控是在 USB dongle 上，所以鍵盤本身也不太耗電，依照我自己的用量，實測大概可以撐半年左右。

不過前一陣子為了方便跟上 SliceMK 的韌體更新，想著把 keymap 好好的拆開放，把原本直接 fork zmk 在 local build firmware 的做法，改成用 [ergodox-zmk-config](https://github.com/dm4/ergodox-zmk-config) ，直接在 GitHub Actions 上面 build firmware 再下載。

除了用新方法更新 USB 接收器的韌體之外，那時候想說好像也很久沒更新鍵盤韌體了，也順手照著 [Peripheral Firmware](https://docs.slicemk.com/keyboard/ergodox/peripheral/) 重刷了一下兩邊鍵盤的韌體。

不過更新完之後開始有點悲劇，常常用幾個小時之後會，突然有一邊（或兩邊）的鍵盤沒有反應，重新插拔 USB 接收器之後再等個幾秒通常可以解決。這樣用了一陣子實在覺得太惱人，剛好看到 Discord 上官方有說接下來會有韌體更新，想說我就等新的韌體出來再更新好了，又拿出來之前的 [BLE660C](https://ydkb.io/help/#/en/keyboards/ble660c_980c) 來用。

BLE660C 是一個 Leopold FC660C 的改裝方案，藉由把原本的主控換掉再加上個鋰電池，讓 FC660C 從一把有線鍵盤，變成一把有線 / 藍牙都可以用的鍵盤，也可以從官方提供的 [Keymap Builder](https://ydkb.io/?ble660c) 自訂鍵位，不過在 macOS 上的刷機流程稍微有點麻煩就是了，要認真看一下文件。

不過換鍵盤也讓我發現，有個近期才新增的按鍵設定實在是太好用了，和大家分享一下：把左下角的 Ctrl  `⌃` 設成 Command `⌘` + Option `⌥` ！

在這個年代，感覺把 CapsLock 改掉已經是基本操作了？之前用筆電的時候並不是直接讓 CapsLock 和 Ctrl 交換，而是把 CapsLock 設成 Ctrl ，因為 MacBook 鍵盤右邊沒有 Ctrl ，這樣設定完就是左側會有兩個鍵（ CapsLock / Ctrl ）按下去都是 Ctrl ，保留原本的 Ctrl 一方面是怕突然會需要借人操作電腦，再來也是想說有些 Ctrl  `⌃` + Option `⌥` 之類的快捷鍵比較好按。

不過用久了發現其實用到 Ctrl 的組合鍵不多，這種多個 modifier keys 的快捷鍵，好像最常用到的還是 Command `⌘` + Option `⌥` ！除了一些軟體預設的快捷鍵會用到這種組合外，我後來也找了幾個組合來設成 Raycast 各種常用動作的 Hotkey ，真的是用過就回不去了。

回到 SliceMK ，本來想說等 4.0.0 出來就來更新，沒想到拖著拖著，在前幾天真的想要更新的時候發現都已經 4.1.0 了，照著一樣的流程，也順便改了 [config](https://github.com/dm4/ergodox-zmk-config/commit/ee2a5a9c4598ba55e7bb503dbc5515f89df99fc2) ，到目前用了幾天都沒斷線過，完美的滿血復活！

後來想了一下，我推測之前會時不時斷線可能是更新了鍵盤韌體之後， dongle 那邊可能沒有用到對應的 branch ，看起來之前用的 [slicemk/zmk](https://github.com/slicemk/zmk) main branch 有點久沒更新，不過也懶得再刷回去測試了 XD

最後和大家分享一下目前的鍵位設定，是用 [keymap-drawer](https://github.com/caksoylar/keymap-drawer) 讀我的 [slicemk_ergodox.keymap](https://github.com/dm4/ergodox-zmk-config/blob/main/config/slicemk_ergodox.keymap) 畫的：

{% include image.md name="keymap.svg" %}
