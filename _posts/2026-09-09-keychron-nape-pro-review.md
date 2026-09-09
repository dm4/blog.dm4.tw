---
layout: post
title: "Keychron Nape Pro 使用心得"
---

{% include image.md name="keyboard-with-nape-pro.jpg" alt="keyboard-with-nape-pro" %}

滿久以前就在社團看到[有人討論 Nape Pro](https://www.facebook.com/groups/1111882339005914/posts/3005907322936730/)，過了半年台灣終於也在嘖嘖開預購啦！收到貨之前其實也去三創摸到了實體，不過實際收到後，和鍵盤擺在一起才發現……真的好厚 XD

因為用的是分離式鍵盤，收到之前一直在想應該把 Nape Pro 放在哪，是要放兩手中間嗎？還是應該放在某一邊鍵盤的下方？真的拿到實體有試擺了一下，發現如果放在鍵盤下方，靠太近的話在使用鍵盤的時候容易誤觸，放太遠的話又會覺得好像不如直接去用右邊的 MX Ergo？

後來也想過還是來練習一下用左手拇指控球，不過試了一下發現了 Nape Pro 的問題：在一般用這種小軌跡球的時候（通常是拇指球或食指、中指球），手掌都是有支撐的，會像是握著滑鼠的姿勢在使用，不過 Nape Pro 的造型導致了手掌會是懸空使用的，再加上旋轉環又讓整個球體高度更高，手掌又因為要把手指抬高而需要出更多力。

總之我後來是放棄把它當成桌面上的主力軌跡球使用了，雖然 Nape Pro 小巧好攜帶，但也不太會有出門一定要用軌跡球的情況，於是開始研究剩下的按鍵應該放什麼快速鍵好。之前就有用過 Keychron 的鍵盤，不得不說 [Keychron Launcher](https://launcher.keychron.com/) 還是挺方便的，就算沒接觸過 QMK / ZMK 的人，稍微搞懂幾個 layer 操作也能簡單上手。

在思考要在 Nape Pro 上放什麼快速鍵的時候也是猶豫了一陣子，現在用的 [Ergodox layout](https://github.com/dm4/ergodox-zmk-config/blob/main/images/slicemk_ergodox.svg) 其實還有滿多空間可以設定的，如果認真設定的話，基本上能在不離開 home row 的狀態下，做到幾乎所有想要的鍵盤操作。

如果要離開 home row，全部都在 Nape Pro 上解決的情況，我想到的比較像是修圖調參數之類的場景，用各種快速鍵打開各種選單，再用旋轉環增減參數，可惜我沒有這種需求。

也有看到一個官方的範例是設成 coding agents 的各種快速鍵，但這樣子手要離開 home row 去按 Nape Pro，按完又需要再回去打字，感覺不如背好快速鍵？不知道是不是我這種「不如背好快速鍵」的心態導致我開始覺得 Nape Pro 好像有點雞肋。

{% include image.md name="nape-pro-official-coding-agents.jpg" alt="nape-pro-official-coding-agents" %}

> 圖片來源：<https://www.zeczec.com/projects/keychron-nape-pro>

後來想到或許用語音輸入來操作 coding agents 搭配 Nape Pro 會有奇效？畢竟不用打字，手可以直接常駐在 Nape Pro 上，但我目前還沒有語音輸入的習慣，沒辦法測試。

接著我就想說是不是能找一些「不太會需要接著打字的動作」來設定，有想到幾個：

- 鎖定螢幕之後接 Esc：不按 Esc 的話 macOS 不會馬上讓螢幕休眠，而且鎖定螢幕之後就一定不會再用鍵盤了，完美。
- 選定區域的截圖：通常按完會接著用滑鼠（軌跡球）框選目標，本來就會離開鍵盤。
- 切換螢幕的訊號源：一直都知道有 DDC/CI 可以透過 HDMI 來切換螢幕的訊號源，不過因為之前是用二進一出的 HDMI hub 來處理訊號，所以沒有嘗試過。最近因為比較常用 NS2 玩塗擊隊又重新調整了一下 HDMI 線的接法，把 HDMI hub 撤掉了，改用 [m1ddc](https://github.com/waydabber/m1ddc) 來切換訊號源，搭配 [Hammerspoon](https://www.hammerspoon.org/) 來把指令綁上快速鍵，再透過 Nape Pro 觸發。

用起來和遙控器一樣（雖然平常是不會拿起來按啦）：

<video controls width="100%">
  <source src="/assets/images/keychron-nape-pro/nape-pro-m1ddc.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

最後的設定滿無聊的，好像也沒有好好利用到軌跡球和旋轉環，本來是為了軌跡球買 Nape Pro 的，但用起來之後感覺我可能比較適合一個沒有軌跡球、單純帶旋鈕的長條型無線巨集鍵盤？

{% include image.md name="nape-pro-keys.png" alt="nape-pro-keys" %}
