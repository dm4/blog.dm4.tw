---
layout: post
title: "Ultron 奧創智慧插座 (Sonoff S31) 刷 ESPHome"
---

之前看到這篇 [SONOFF S31 刷 ESPHome](https://hackmd.io/@LHB-0222/sonoff_S31_ESPHome) 之後就想買幾個來試試，因為我只是想拿來監測一些插座的用電量，用智慧插座很怕想理線的時候誤觸按鈕造成斷電，刷 [ESPHome](https://esphome.io/) 除了方便之後更新設定之外，也可以順便拿掉實體按鈕的繼電器開關功能。

上網查了一下發現好像台灣比較難直接買到 Sonoff S31 ，不過倒是找到一個外型幾乎一樣的 [奧創智慧插座 Lite](https://www.ultronsmart.com/product/smart-outlet) ，看起來價格好像也差不多，就買了個來試刷看看。

拆開來果然發現 PCB 上面就是印著 Sonoff S31 ！不過手上沒有測試勾或是燒錄夾，就用手邊的鱷魚夾試刷 ESPHome ……沒想到真的成功了 XD

{% include image.md name="flashing.jpg" %}

不過刷完之後一直讀不到插座的功率，後來看了別人的 YouTube 影片：

<iframe width="560" height="315" src="https://www.youtube.com/embed/6aeUWPLgaqY?si=xHNOLfi__SOcERPV" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

發現我買到的奧創上面好像少了功率偵測的晶片：

{% include image.md name="compare.png" %}

再認真上 Sonoff 官網查了一下才發現 Sonoff S31 除了區分 Wifi / Zigbee 兩版之外，在 Wifi 裡又有分一般版 / Lite 版，其中 Zigbee / Wifi Lite 都是沒有功率偵測的功能……果然買之前還要再更認真查資料才是。

- [Sonoff S31 Zigbee](https://sonoff.tech/product/smart-plugs/s31-lite-zb/)
- [Sonoff S31 Wifi / Lite](https://sonoff.tech/product/smart-plugs/s31-s31lite/)

不過至少還能當 Wifi 版本的智慧插座用就是了，這樣看起來要在台灣好買、有功率偵測的智慧插座，好像還是小米的 Zigbee 插座，不過之前用小米插座和 z2m 配對的時候有幾顆常常遇到問題，不知道是我買到機王還是品質不穩定……之後再看要不要從淘寶買 S31 回來刷 ESPHome 好了？
