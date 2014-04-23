---
layout: post
title: "自製 USB 鍵盤轉藍芽鍵盤轉接器"
date: 2014-04-22 10:03
comments: true
categories: [arduino, hardware, keyboard]
---

## 測試影片

{% youtube RL4jmP1iwRM %}

##材料

- [Arduino Pro Mini](http://www.icshop.com.tw/product_info.php/products_id/10617)
- [USB Host Shield for Arduino Pro Mini](https://www.circuitsathome.com/products-page/arduino-shields/usb-host-shield-for-arduino-pro-mini)
- [BlueSMiRF HID](https://www.sparkfun.com/products/10938)

一開始是因為想要有無線的機械式鍵盤，後來覺得如果可以做成 USB 轉藍芽的模組，就可以把不同的機械式鍵盤都轉成無線的！

不過硬體不太熟，所以就用了比較好寫軟體的 Arduino 來開始，整體流程大致是用 USB Host Shield 來讀鍵盤訊號，然後從 BlueSMiRF HID 送出藍芽的 HID report ，這樣只要有支援藍芽鍵盤的電腦都可以用了！

不過只是 prototype 而且還有很多可以改進的地方，像是現在看起來超醜 XD 而且不知道為什麼用起來有一點點可以感覺到的延遲，是說不知道有沒有廠商要做類似的東西來賣，好想要啊！

下次可以試試看拿 [ATEN CS533][] 拆開來玩，看會不會比較省事 XD

[ATEN CS533]: http://www.aten.com.tw/products/KVM%E5%A4%9A%E9%9B%BB%E8%85%A6%E5%88%87%E6%8F%9B%E5%99%A8/%E7%84%A1%E7%B7%9A%E5%88%87%E6%8F%9B%E7%94%A2%E5%93%81/~CS533.html
