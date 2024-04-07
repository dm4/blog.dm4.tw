---
layout: post
title: "四天連假來玩 Magic Mirror / Arc / WireGuard / Home Assistant / ESPHome / Persona 5 Royal"
---

難得的四天連假幾乎都沒有出門，花了時間把一些舊東西重新架起來，也玩了一些新東西。

### [Magic Mirror](https://magicmirror.builders/)

一年多之前搬家之後就沒有再架起來，這次把 Magic Mirror 重新架起來，一樣是用來看個天氣、時間、行事曆之類的。這次就不像之前一樣跑在 Raspberry Pi 上，改成在自己的機器上用 Docker 跑 server mode ，在 Raspberry Pi 上單純跑 client mode 來顯示畫面在螢幕上。

也發現 Raspberry Pi 上關掉 HDMI 的指令好像和之前不一樣？舊的指令像是 `vcgencmd`, `tvservice` 好像都不能用了，試了 `xrandr` 也失敗，後來查到可以用 `wlr-randr` ：

```bash
# 拿裝置資訊
WAYLAND_DISPLAY="wayland-1" wlr-randr
# 開關 HDMI-A-1
WAYLAND_DISPLAY="wayland-1" wlr-randr --output HDMI-A-1 --off
WAYLAND_DISPLAY="wayland-1" wlr-randr --output HDMI-A-1 --on
```

## [Arc](https://arc.net/)

一個看上去很潮的瀏覽器，用了一兩天之後決定把預設瀏覽器從 Safari 換成它試試看， Tab 集中在左邊我覺得很讚，設成預設瀏覽器之後在其他程式裡點連結會打開一個暫時的 Little Arc 也很方便，不過它現在主打的 AI 功能我沒什麼感覺就是了。

邀請連結： <https://arc.net/gift/662735f6>

## [Wireguard](https://www.wireguard.com/)

其實也是因為 Arc 的其中一個 AI 功能 `Ask on Page` 我一直沒辦法用，後來聽說有人在國外可以用？就想說在日本的 VPS 機器上裝個 VPN 試試看，第一次試 Wireguard 覺得真的是方便好設定，如果只是要簡單弄個 VPN Server/Client 的話滿推薦的。

然後架完發現 `Ask on Page` 真的過 VPN 就可以用了……

## [Home Assistant](https://www.home-assistant.io/)

在一年多前的租屋處本來是用 [Homebridge](https://homebridge.io/) 來接一些裝置在 HomeKit 上，一樣搬家之後就沒有再架起來，想說反正都要重新加一堆設備了，索性直接換成支援更多裝置的 Home Assistant 來試試看，印象中可能兩三年前也有試過一次 Home Assistant ，但是當時覺得設定太麻煩，加上已經有東西跑在 Homebridge 上，就沒認真研究。不過這次體感上設定比印象中容易一點？不知道是錯覺，還是這幾年真的改善了很多流程。

不得不說 Home Assistant 對裝置的支援度真的是滿高的，家裡的藍牙 BLE 溫濕度計都可以無痛加入（甚至還搜到了隔壁的小米體重計……），像洗衣機、印表機的也有找到第三方的 Integration 可以用。

還沒有研究太多功能，之後應該會先試 [Zigbee2MQTT](https://www.zigbee2mqtt.io/) ！

## [ESPHome](https://esphome.io/index.html)

放假也同樣翻出來了好久之前買的 NodeMCU 和 DHT22 溫濕度計，之前是用 Arduino IDE 來讓 NodeMCU 可以讀 DHT22 的資料，再想辦法讓 Homebridge 可以讀到這些資料，這次試了 ESPHome 來做同樣的事，簡單的讀值只要幾行 yml 就可以做到，然後透過 ESPHome 也可以直接 OTA 更新 NodeMCU 的程式，驚為天人。

## [Persona 5 Royal](https://store.steampowered.com/app/1687950/)

玩的超慢，大概七十幾個小時才跑完 P5R 無印版結局，之後應該會從分歧點繼續跑皇家版的劇情，不知道還可以玩多久 XD
