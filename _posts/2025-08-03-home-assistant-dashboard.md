---
layout: post
title: "Home Assistant Dashboard"
ogimage: "battery.png"
---

{% include image.md name="battery.png" alt="Battery Dashboard" %}

用了這麼久 Home Assistant 一直都沒有好好整理 dashboard，在又買了幾個溫濕度計之後，決定要來把一堆設備的電量電量都整理一下。

不過整理完發現一堆 100% 電量的……感覺好像不太準，查了一下發現一堆 100% 電量的設備最後一次回報電量都是兩週前，不過都還是有在繼續更新溫濕度，感覺是哪裡有問題？

查一查翻到這篇 [Zigbee device battery status not updating](https://community.home-assistant.io/t/zigbee-device-battery-status-not-updating/761552)，再觀察一下狀況好了……😢
