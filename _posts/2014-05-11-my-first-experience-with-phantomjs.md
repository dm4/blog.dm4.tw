---
layout: post
title: "PhantomJS 初體驗"
tags: [phantomjs, javascript, nodejs]
redirect_from: /2014/05/11/my-first-experience-with-phantomjs.html
---

最近因為想要爬個網頁資料，結果礙於處理不了萬惡的 `__VIEWSTATE` 和 `__EVENTVALIDATION` ，在時間的壓力下就決定改用 ggm 推薦 [PhantomJS][1] ，一用之下驚為天人啊，太神了！

[PhantomJS][1] 概念就是在裡面跑一個 Webkit ，然後讓你用 JavaScript 控制它，所以可以模擬瀏覽器載入網頁之後 inject script / evaluate script / screenshot 的動作，官方文件也有寫到可以拿來作 web 的 testing ，模擬瀏覽、點擊之類的瀏覽器操作。

官網的 [Quick Start](http://phantomjs.org/quick-start.html) 寫的蠻清楚的， [Examples](http://phantomjs.org/examples/index.html) 也有很多範例可以參考，我自己用到的功能只有連線一個網頁後，對網頁的 `document` 作操作，然後把結果印出來，下面這段簡單的 code 就是連上 <http://blog.dm4.tw> 首頁，拿到最新一篇文章的標題，印出標題之後截圖：

```javascript
var page = require('webpage').create();

page.open('http://blog.dm4.tw', function() {
    var article_title = page.evaluate(function() {
        return document.getElementsByClassName("entry-title")[0].children[0].innerHTML;
    });
    console.log(article_title);
    page.render('blog.png');
    phantom.exit();
});
```

## PhantomJS + Node.js

可以用 <https://github.com/Medium/phantomjs> ，裡面也有提到怎麼用 `node` 跑起來：

```javascript
var path = require('path')
var childProcess = require('child_process')
var phantomjs = require('phantomjs')
var binPath = phantomjs.path

var childArgs = [
  path.join(__dirname, 'phantomjs-script.js'),
  'some other argument (passed to phantomjs script)'
]

childProcess.execFile(binPath, childArgs, function(err, stdout, stderr) {
  // handle results
})
```

不過要注意他是開 process 跑的，不要像我一不注意開太多結果……

## 遇到的問題

因為直接用 `node` 跑的話沒噴錯誤訊息，所以下面這兩個問題找了好久啊，如果發現 `node` 跑怪怪的，可以直接用 `phantomjs` 跑跑看，至少錯誤訊息比較清楚……

### 在 Ubuntu 上跑不起來

因為要 render 的關係，所以要裝 `libfontconfig`

    sudo apt-get install libfontconfig

### 打不開 https 的網頁

有些 https 網頁憑證沒設好，預設很貼心把它擋下來了，如果要打開的話，執行時要加上參數 `phantomjs --ignore-ssl-errors=true` ，如果是用 `node` 跑的話是要加在 arguments 裡：

```javascript
var childArgs = [
  '--ignore-ssl-errors=true',
  path.join(__dirname, 'phantomjs-script.js'),
  'some other argument (passed to phantomjs script)'
]
```

雖然跑起來有點慢，不過功能太強了啊！讓我以後有點懶的去看 http request 用 `curl` 送 query 了 XD

[1]: http://phantomjs.org
