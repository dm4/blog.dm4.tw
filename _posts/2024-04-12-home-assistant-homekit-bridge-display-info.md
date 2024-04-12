---
layout: post
title: "用 Home Assistant 的 HomeKit Bridge 在 HomeKit 裡顯示資訊"
---

[Home Assistant](https://www.home-assistant.io/) 原生就支援 [HomeKit Bridge](https://www.home-assistant.io/integrations/homekit/) ，可以讓 Home Assistant 做為一個 HomeKit 橋接器，讓指定的裝置也能在 HomeKit 上顯示。

不過根據文件裡的 [Supported Integrations](https://www.home-assistant.io/integrations/homekit/#supported-integrations) 會發現並不是所有的裝置都支援，像是 `sensor` 在 HomeKit 上就只能顯示幾種固定的資訊，這幾天嘗試了一下怎麼樣能讓 HomeKit 上顯示更多資訊（主要是能顯示自訂的文字或圖片）。

因為家裡的洗衣機連上 Home Assistant 之後能夠知道剩餘洗衣時間，實體會是一個 `sensor.remaining_time` ，狀態會是一個時間字串 `h:mm:ss` ，想說能不能透過 HomeKit Bridge 來顯示剩餘洗衣時間。

最一開始想透過裝置名稱來顯示想要的字串，結果發現……

> HomeKit will cache the name on the first run so the accessory must be reset for any change to take effect.

看起來沒辦法隨時透過裝置名稱來更新要顯示的字串，找了一陣子覺得沒有發現合適的，最後決定想辦法模擬成 camera 來加進 HomeKit Bridge 裡。

成果長這樣：

![](/assets/images/home-assistant-homekit-bridge-display-info/homekit.png)

大概的做法是：

- 想辦法生出一張圖片包含要顯示的文字
- 透過 [Generic Camera](https://www.home-assistant.io/integrations/generic) 把圖片模擬成一個 camera
- 透過 HomeKit Bridge 把 camera 加進 HomeKit 裡

生圖的部分我就簡單用了 [ImageMagick](https://imagemagick.org/index.php) 裡的 convert 來做：

```bash
convert -size 600x200 xc:black -font WenQuanYi-Micro-Hei \
    -pointsize 35 -fill white -gravity center \
    -annotate +0+0 "{text}" data/wash-remain-time.jpg
```

然後因為 Generic Camera 是需要圖片網址，所以就又接了一個 HTTP API `/ha/wash/<text>` 來回傳內含指定文字的圖片：

![](/assets/images/home-assistant-homekit-bridge-display-info/curl-api-imgcat.png)

（上面用的 `imgcat` 是 [這個](https://pypi.org/project/imgcat/) ，我覺得很讚）

因為 Generic Camera 裡的圖片網址可以是 [template](https://www.home-assistant.io/docs/configuration/templating/) ，所以最後在 Generic Camera 的圖片網址填上像是

{% raw %}

```text
http://localhost:5002/ha/wash/洗衣機剩下 {{ states("sensor.remaining_time") }}
```

{% endraw %}

最後再照著文件，把這這個 Generice Camera 產生的攝影機加到 HomeKit Bridge 裡，我自己因為裝置比較少，所以是一個一個在 `include_entities` 裡面指定想在 HomeKit 裡看到的裝置。

---

順便記錄一下中間學到，但是最後沒用到的 Home Assistant 功能：

本來沒有發現圖片網址是 template ，還先用了 [RESTful Command](https://www.home-assistant.io/integrations/rest_command/) 來發 HTTP request 生成圖片：

{% raw %}

```yml
rest_command:
  wash_remain_time:
    url: "http://localhost:5002/ha/wash"
    method: post
    content_type: "application/json"
    payload: '{"text": "洗衣機剩下 {{ states("sensor.remaining_time") }}" }'
```

{% endraw %}

然後想辦法把圖片放 Home Assistant 的 `config/www` 之後，再利用 [Hosting files](https://www.home-assistant.io/integrations/http/#hosting-files) 來放在 Home Assistant 的 HTTP server 上。

---

Generic Camera 真的是滿通用的，誇張一點的話甚至可以畫一個 dashboard 顯示在 HomeKit 裡？
