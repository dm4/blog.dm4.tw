---
layout: post
title: "試用 OpenAI 的 Function Calling API"
tags: [ai]
redirect_from: /2024/03/21/try-gpt-function-calling.html
---

結果試了一下發現 API 很親切耶，只要多給一個 `tools` 參數就可以了，然後再照著回應裡的 `tool_calls` 來呼叫，再把結果傳回去讓 GPT 來接著回應。

`tools` 裡要給的是你想讓 GPT 呼叫的 function 相關資訊，像是 `name`, `description`, `parameters` ，不過好像沒有讓人描述回傳值格式的地方，我猜應該是可以寫在 `description` 裡面，不過如果回傳值是可讀的 JSON 格式的話，我試起來 GPT 是可以自動解析的。

GPT 回應裡的 `tool_calls` 會包含一連串 GPT 想要呼叫的 function 以及相對應的參數，呼叫完之後把回傳值傳給 GPT 就可以讓他（它？）接著回應了。

我試著定義了 `get_home_temperature` 來拿到家裡各個房間的溫度，其中各個房間的定義並沒有特別寫在 `description` 裡面，並回傳一個 JSON 格式的字串像是：

```json
{ "living_room": 26, "bed_room": 24, "dining_room": 23 }
```

接著又定義了一個 `control_appliance` ，來讓 GPT 能控制家裡的電器：

```python
control_appliance(appliance: str, status: str)
```

最後只要告訴 GPT `如果客廳的溫度大於 25 度的話，就幫我開冷氣和電風扇。` ，就會發現 GPT 會先試著呼叫 `get_home_temperature` 來拿到各個房間的溫度，自己判斷那個是客廳，然後再根據溫度來呼叫 `control_appliance` 來控制相對應的電器。

程式碼大概長這樣：

<!-- markdownlint-disable-next-line MD033 -->
<script src="https://gist.github.com/dm4/3571352e85558508f930b454105c035e.js"></script>

執行結果：

```console
$ ./tool.py
[2024-03-21 21:41:45,650] [INFO] [__main__] Calling 1 tool(s):
[2024-03-21 21:41:45,650] [INFO] [__main__] -> get_home_temperature({})
[2024-03-21 21:41:50,265] [INFO] [__main__] Calling 2 tool(s):
[2024-03-21 21:41:50,265] [INFO] [__main__] -> control_appliance({"appliance": "air_conditioner", "status": "on"})
[2024-03-21 21:41:50,265] [INFO] [__main__] -> control_appliance({"appliance": "fan", "status": "on"})
[2024-03-21 21:41:54,819] [INFO] [__main__] 已經幫您開啟了客廳的冷氣和電風扇。
```

不過因為這個範例比較簡單，像是溫度這種資訊其實不佔什麼 token ，應該是可以直接放在 prompt 裡面一起給 GPT 更方便，不過如果是一些比較大量，又不一定會需要用到的資訊的話，用 function calling 去拿資訊應該是滿方便的。

另一個驚豔的地方是 GPT 很好的做到了類似自然語言和程式邏輯的轉換，一般來在建好各種 function 之後，我們本來可能會需要對每個功能實作像是下面的簡單邏輯：

```python
# 如果客廳的溫度大於 25 度的話，就幫我開冷氣和電風扇。
if get_home_temperature()["living_room"] > 25:
    control_appliance("air_conditioner", "on")
    control_appliance("fan", "on")
```

如果之後又想讓 `25` 變成 `26` ，可能又會需要修改程式碼，如果想要新增控制的電器，可能也要修改程式碼，但是透過 GPT 的 function calling API ，看起來只需要簡單的調整 prompt 就行。試完之後確實是可以理解為什麼大家一直在討論各種透過 GPT 來實現 low-code 或 no-code 的情境了。
