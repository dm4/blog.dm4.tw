---
layout: post
title: "在 Home Assistant 使用能偵測用電量的智慧插座：小米插座 / Sonoff S31 / Tapo P110M"
---

最近又在嘗試各種可以偵測用電量的智慧插座，之前用過，確定 Zigbee2MQTT 有支援的 [小米插座 (ZNCZ03LM)](https://www.zigbee2mqtt.io/devices/ZNCZ03LM.html) 好像停產了，現在的 [小米插座 2 (XMZHCZ01Q)](https://www.mi.com/tw/product/xiaomi-smart-plug-2/specs) 在 [Zigbee2MQTT Supported Devices](https://www.zigbee2mqtt.io/supported-devices/#v=Xiaomi&s=XM) 沒看到，看了一下規格才發現原來改成 Wifi + Bluetooth 連線了，不過現在實體店和線上商店都沒貨，殘念。

Sonoff S31 倒是不錯，除了要拆開來刷機的螺絲有點難拆之外（我手殘滑牙了好幾個 QQ ）沒什麼好抱怨的，不過就算不刷成 ESPHome ，也是可以用 [SonoffLAN](https://github.com/AlexxIT/SonoffLAN) 接進 Home Assistant 。

接著又試了 [Tapo P110M](https://www.tp-link.com/zh-hk/home-networking/smart-plug/tapo-p110m/) ，之前用過 Tapo 的 P100 和 H306 ，都可以用 [TP-Link Smart Home](https://www.home-assistant.io/integrations/tplink) 接進 Home Assistant ，想說 supported devices 裡有 P110M ，應該沒問題吧？

結果……果然沒有想像中順利，在手機上的 Tapo app 設定完 P110M 之後，沒辦法在 Home Assistant 用 TP-Link Smart Home integration 新增裝置，在輸入設備 IP 之後，會再跳出要輸入 Tapo 帳號密碼的欄位，接著不管怎麼打都顯示認證失敗，崩潰。

搜了一陣子才找到解法： <https://github.com/petretiandrea/home-assistant-tapo-p100/issues/854#issuecomment-2910024832> ，主要的問題就是在 Tapo app 裡，新增裝置時不要用任何跳出來的建議選項，老老實實的去點右上角的加號，選插座，選 P110M ，再老老實實的告訴它 wifi 帳號密碼讓它使用，之後就可以正常在 Home Assistant 裡找到 P110M 插座了

在 <https://github.com/python-kasa/python-kasa/issues/1353#issuecomment-2686035835> 有人分享了原因，在點了 app 中的建議新增裝置選項時，其實是走了 [TP-Link Simple Setup](https://community.tp-link.com/en/home/forum/topic/692206) 的設定流程，讓新設備直接和現有的設備同步設定，但會造成原本 [python-kasa](https://github.com/python-kasa/python-kasa) 使用的認證流程失敗，而 Home Assistant 裡的 TP-Link Smart Home integration 用到的就是 python-kasa 。

另外這幾個插座，在接進 Home Assistant 之後的 energy entity 的行為也不太一樣，小米插座 ZNCZ03LM 的 energy entity 會不斷累計， Sonoff S31 的會每天歸零， Tapo P110M 則是會有當天累計和當月累計兩個 energy entity ，如果想要弄個會不斷累計的 energy entity 的話可以參考內建的 [Integral](https://www.home-assistant.io/integrations/integration/) 。

不過其實 P110M 單價也是比 S31 高，外型我感覺差不多，都滿普通的，好像也沒什麼一定要用的理由？之後有貨的話也想買個小米插座 2 來試試，究竟能不能直接接進 Home Assistant 呢？
