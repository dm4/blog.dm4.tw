---
layout: post
title: "用 Fava Extension 在 Fava 裡新增 API Endpoint"
---

在之前的 [這篇](/personal-finance-software-2023) 提到用了 beancount 格式記帳，搭配 fava 來瀏覽帳冊，最近在整理帳務的時候有個新的需求：想讓 fava 提供一個能執行 BQL (beancount query language) 的 API ，能執行 BQL 之後回傳 JSON 格式的結果。

一開始也想說要不要自己簡單用 Flask 加上 bean-query 來跑，一方面是比較麻煩，再來就是如果跑 bean-query 會需要重新解析所有的 beancount files ，直接用 fava extension 應該能直接用 fava 處理好的資料，速度上應該也比較快。

fava extension 用法其實和 beancount plugin 差不多，可以參考官方的 [fava.ext.fava_ext_test](https://github.com/beancount/fava/tree/main/src/fava/ext/fava_ext_test) ，不過如果沒有要用 template 的話，其實可以更簡單：準備一個 `extension/my_ext.py` ，內容如下：

```python
from fava.ext import FavaExtensionBase, extension_endpoint
from flask import jsonify

class MyExt(FavaExtensionBase):
    @extension_endpoint
    def example_data(self):
        return jsonify(["some data"])
```

接著在自己的 beancount file 裡加上：

```beancount
2024-12-09 custom "fava-extension" "extension.my_ext"
```

這樣子就可以做出一個 extension endpoint 來回傳想要的資料了：

```console
$ curl http://<host>/<bfile>/extension/MyExt/example_data
[
  "some data"
]
```

接下來應該就是準備處理把 BQL 傳進來並執行了對吧？我本來也是這樣想的，不過發現了一條捷徑……因為本來就有用的 [fava-dashboards][] 已經有做了一個 query endpoint ，可以把傳進來的 BQL 執行完之後回傳結果（ code 在 [這裡](https://github.com/andreasgerstmayr/fava-dashboards/blob/50a0b7c/src/fava_dashboards/__init__.py#L203-L213) ），用了 [fava-dashboards][] 之後就可以：

```python
import requests

BASE_URL = "http://<host>/<bfile>/extension/FavaDashboards/query"

if __name__ == '__main__':
    bql = """SELECT CONVERT(SUM(position), 'TWD') AS value
WHERE account ~ '^Liabilities:' OR account ~ '^Assets:'"""
    bql = requests.utils.quote(bql)
    url = f"{BASE_URL}?bql={bql}"
    response = requests.get(url)
    result = response.json()
    if result['success']:
        print(result)
```

上面這樣子就可以拿到資產 + 負債的台幣淨值了。

---

不得不說 [fava-dashboards][] 滿讚的，可以簡單的弄出想要的圖表，主要是在 YAML 寫 BQL 和資料的處理，然後生出用 [Apache Echarts](https://echarts.apache.org/zh/index.html) 畫的圖，可以看一下 [fava-dashboards/example](https://github.com/andreasgerstmayr/fava-dashboards/tree/main/example) 。

用 [fava-dashboards][] 最麻煩的地方應該是……要在 YAML 裡面寫 HTML / JavaScript ，縮排沒弄好真的會讓人崩潰（還好有 copilot 讓我可以只寫註解），希望之後有時間再寫一篇來介紹 [fava-dashboards][] ！

[fava-dashboards]: https://github.com/andreasgerstmayr/fava-dashboards
