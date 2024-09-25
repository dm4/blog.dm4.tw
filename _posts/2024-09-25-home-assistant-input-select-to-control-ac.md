---
layout: post
title: "在 Home Assistant 利用 input_select 控制冷氣"
---

因為用 Home Assistant 的 HomeKit Bridge 想要把 SwitchBot Hub 的冷氣控制整個丟到 HomeKit 裡實在是遇到太多問題， HomeKit 對於冷氣遙控器的支援也不太完整（像是沒有風量、風向），後來整理了一下自己平常開冷氣的習慣，發現其實平常最常用的也只有幾種情況而已，這樣的話在 Home Assistant 裡只要用簡單的 `input_select` 就可以滿足。

目前家裡開冷氣的習慣是：開冷氣通常只會用遙控器把冷氣打開，開了之後幾乎不會需要調整溫度，冷氣風量也都是不會調整，如果有要短時間外出的話可能會切成送風，另外在冷氣關機前也會想要先開送風一陣子再關機。

用 `input_select` 搭配幾個簡單的選項就會變成：

```yml
input_select:
  livingroom_ac:
    name: 客廳冷氣
    options:
      - 關閉
      - 冷氣
      - 送風
```

接下來只要在自動化那邊設定，當特定的 `input_select` entity 狀態改變時，送出對應的遙控器訊號就好了，我是直接在網頁上設定了，當然也可以手動寫在 `yaml` 設定檔裡：

{% include image.md name="automation.png" %}

這樣子要用什麼東西開關冷氣 / 切換冷氣狀態，也都變得很容易，只要改變對應的 `input_select` 裡的值就好了，我大部份也都還是在網頁上設定，如果要在 `yaml` 裡寫 action 的話大概會像是這樣：

```yaml
- action: input_select.select_option
  data:
    option: 關閉
  target:
    entity_id: input_select.livingroom_ac
```

這樣的好處其中之一，是可以用簡單的按鈕就可以控制冷氣的狀態，像是如果有個兩個按鍵的控制器，就可以這樣設定：

- 按下開：
  - 如果目前狀態是 **冷氣** 就切換成 **送風**
  - 其他狀態就切換成 **冷氣**
- 按下關：
  - 切換成 **關閉** （想要的話也可以設定成 **送風** 幾分鐘之後 **關閉** ）

而且只要都不用原本的遙控器控制冷氣，實際的冷氣狀態就會完全和 Home Assistant 裡的狀態一致，再也不用擔心出門到底有沒有關冷氣了！

---

另外如果有用 [Zigbee2MQTT](https://www.zigbee2mqtt.io/) 的話，我覺得 IKEA 的 [RODRET 開關](https://www.ikea.com.tw/zh/products/electronics/connectivity-and-control/rodret-art-40559802) （ [IKEA E2201 - Zigbee2MQTT](https://www.zigbee2mqtt.io/devices/E2201.html) ）很讚，兩邊的按鍵都可以長按，一共可以觸發四種動作，而且開關背後的磁鐵可以方便吸在金屬板或是冰箱上。
