---
layout: post
title: "在 Mac OSX 上編譯 GH60 RevCHN Firmware"
tags: [keyboard, gh60]
redirect_from:
    - /2015/03/17/build-gh60-revchn-on-mac.html
    - /blog/2015/03/17/build-gh60-revchn-on-mac/
---

來記錄一下在 Mac OSX 上，怎麼編譯和刷 GH60 RevCHN 的韌體

### 參考資料

- http://www.v2ex.com/t/161887
- https://www.ptt.cc/bbs/Key_Mou_Pad/M.1426361327.A.EEB.html
- https://www.ptt.cc/bbs/Key_Mou_Pad/M.1390542876.A.073.html

### 步驟

根據 http://www.v2ex.com/t/161887 首先要先用 brew 裝crosspack 和 dfu-programmer ，不過因為時間過太久，有點忘記詳細的安裝指令， crosspack 需要 `brew tap` 的樣子。

裝完之後可以來載 source ，因為 [tmk/tmk_keyboard][tmk_keyboard] 不支援 RevCHN ，所以要用 [kairyu/tmk_keyboard_custom][tmk_keyboard_custom]

```
git clone https://github.com/kairyu/tmk_keyboard_custom.git
```

需要在 `config.h` 加上設定，來切換成 GH60 RevCHN

```
cd keyboard/gh60
vim config.h
```

在檔案裡加上（如果你不確定加在哪，加到 `#define CONFIG_H` 下面就可以了）

```
#define GH60_REV_CHN
```

本來以為這樣就可以了，沒想到一直刷失敗，後來看了 http://www.v2ex.com/t/161887 和 https://www.ptt.cc/bbs/Key_Mou_Pad/M.1426361327.A.EEB.html 兩篇，發現應該是要清空 EEPROM 的問題（可是我硬體知識不足，根本不知道那是什麼啊啊啊）

ptt 那篇的解法是「在插入 usb 前，按住 backspace 與 space 」，不過我有找到 Makefie 裡有個設定，刪掉（註解掉）也是可以的，和大家分享一下：

```
vim Makefile
```

把下面這行刪掉

```
KEYMAP_IN_EEPROM_ENABLE = yes # Read keymap from eeprom
```

接下來就插上鍵盤， OSX 應該會抓到一隻 GH60 ，可以用 `system_profiler SPUSBDataType` 指令檢查一下

```
(master) [~/workspace/tmk_keyboard_custom/keyboard/gh60] ➟  system_profiler SPUSBDataType
...
GH60:

  Product ID: 0x6060
  Vendor ID: 0xfeed
  Version:  0.01
  Speed: Up to 12 Mb/sec
  Manufacturer: geekhack
  Location ID: 0x14100000 / 15
  Current Available (mA): 500
  Current Required (mA): 100
```

按一下背面的按鈕會進入 dfu 模式，這時候 OSX 應該會抓到一個 Atmel 的設備

```
(master) [~/workspace/tmk_keyboard_custom/keyboard/gh60] ➟  system_profiler SPUSBDataType
...
ATm32U4DFU:

  Product ID: 0x2ff4
  Vendor ID: 0x03eb  (Atmel Corporation)
  Version:  0.00
  Serial Number: 1.0.0
  Speed: Up to 12 Mb/sec
  Manufacturer: ATMEL
  Location ID: 0x14100000 / 16
  Current Available (mA): 500
  Current Required (mA): Unknown (Device has not been configured)
```

然後就可以直接試刷預設的 poker layout 看看

```
make dfu
```

如果成功，在一堆 `avr-gcc` 的編譯訊之後，會看到

```
dfu-programmer atmega32u4 erase
Checking memory from 0x0 to 0x6FFF...  Not blank at 0x1.
Erasing flash...  Success
Checking memory from 0x0 to 0x6FFF...  Empty.
dfu-programmer atmega32u4 flash gh60_lufa.hex
Checking memory from 0x0 to 0x69FF...  Empty.
0%                            100%  Programming 0x6A00 bytes...
[>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>]  Success
0%                            100%  Reading 0x7000 bytes...
[>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>]  Success
Validating...  Success
0x6A00 bytes written into 0x7000 bytes memory (94.64%).
dfu-programmer atmega32u4 reset
```

這樣子最基本的 layout 就成功了，如果不確定是不是真的有刷進去，可以改一下 `keymap_poker.c` 裡的 layout 看是不是真的刷進去了（例如把 `Q` 改成 `X` 之類的）

再來就可以自訂 layout 了，本來想很硬派的直接寫 c code ，不過覺得 code 裡很難排版，最後還是用 [keyboard-layout-editor (KLE)][KLE] 和 [TMK Keymap Generator (TKG)][TKG] 來生成 c code ，我用的 layout 是：

- Layer 0 (Default)
    - http://www.keyboard-layout-editor.com/#/layouts/cc85b037963edc17fe98f84e970e3ca4
- Layer 1 (Fn1 層)
    - http://www.keyboard-layout-editor.com/#/layouts/243985ccfe04859cdb078166f60cb781
- Layer 2 (Fn0 層)
    - http://www.keyboard-layout-editor.com/#/layouts/e9453e67d6a099406e002812aaca8687

然後在 [TKG][TKG] 上的設定為：

- Keyboard
    - GH60 (RevCHN)
- Layer Mode
    - Normal
- Number of Layers
    - 3
- Fn0
    - Layer action > Momentary layer 2
- Fn1
    - Layer action > Toggle layer 1

這裡要注意，我把 Fn0 層放在 layer 2 ，是因為 key mapping 會從高層讀到低層，如果把 Fn1 層換到 layer 2 ， Fn0 換到 layer 1 ，這樣子在切到 Fn1 層之後，按著 Fn0 也沒辦法切到 Fn0 層（同樣的問題在 https://www.ptt.cc/bbs/Key_Mou_Pad/M.1390542876.A.073.html 也有解釋）

接著就在 [TKG][TKG] 下載 `.c` 檔，把載下來的 `keymap.c` 放到資料夾下並改名，就可以準備刷了

```
mv ~/Downloads/keymap.c ./keymap_tkg.c
make KEYMAP=tkg dfu
```

這時候如果你看到以下的 error 的話

```
keymap_tkg.c:29:78: error: macro "KEYMAP_TKG" requires 62 arguments, but only 61 given
         LCTL,LALT,LGUI,          SPC,                     FN0, RGUI,RALT,RCTL),
```

是因為 [TKG][TKG] 生 `.c` 檔的時候好像有 bug ，載下來的檔案，裡面的

```
ESC, 1,   2,   3,   4,   5,   6,   7,   8,   9,   0    MINS,EQL, BSPC, \
```

`0` 和 `MINS` 中間少了 `,` ，所以改成

```
ESC, 1,   2,   3,   4,   5,   6,   7,   8,   9,   0,    MINS,EQL, BSPC, \
```

再編一次應該就 ok 了

### 後記

有一些 Fn action 現在還沒摸熟，看起來可以送 key / macro 的樣子，看到 https://github.com/kairyu/tmk_keyboard_custom/blob/master/keyboard/hhkb/keymap_hasu.c 有一些 function / macro 的用法還沒研究。

另外雖然編起來了，不過因為底層不熟，還不太清楚架構，希望研究一下之後可以把它和藍牙發射器接起來，之前的 [自製 USB 鍵盤轉藍芽鍵盤轉接器](http://blog.dm4.tw/convert-usb-keyboard-to-bluetooth-keyboard) 會有點 latency ，不知道如果直接讓 GH60 送訊號給藍牙模組會不會比較好。

[tmk_keyboard]: https://github.com/tmk/tmk_keyboard
[tmk_keyboard_custom]: https://github.com/kairyu/tmk_keyboard_custom
[KLE]: http://www.keyboard-layout-editor.com/
[TKG]: http://www.enjoyclick.org/tkg/
