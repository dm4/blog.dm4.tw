---
layout: post
title: "Firefox HackBar Font Setting on Mac OSX"
tags:
- firefox
- extension
- hackbar
---

聽說 [HackBar](https://addons.mozilla.org/en-us/firefox/addon/hackbar/) 是 Firefox 下很好用的 extension ，
不過在 Mac OSX 下的字型實在是......慘不忍賭，所以找了一下設定檔在哪裡，Firefox extension 在 Mac OSX 下的路徑是：

    ~/Library/Application Support/Firefox/Profiles/xxxxxxxx.default/extensions/{F5DDF39C-9293-4d5e-9AA8-E04E6DD5E9B4}.xpi

其中 `xxxxxxxx.default` 可能要自己找一下，找到 `.xpi` 之後可以直接用 `vim` 來改裡面的檔（.xpi 就是一包 .zip），找到
`chrome/skin/hackbar.css` 把 textbox 的部份加上想要的字型和大小：

    textbox#hackBarTargetUrl,
    textbox#hackBarTargetUrlPostField,
    textbox#hackBarTargetUrlReferrerField { font-family: monaco, monospace, courier; font-size: 16px; }

就有舒服的 [HackBar](https://addons.mozilla.org/en-us/firefox/addon/hackbar/) 可以看了~

<a href="http://www.flickr.com/photos/51077287@N06/7636698076/" title="Flickr 上 sunrisedm4 的 hackbar-font-setting"><img src="http://farm8.staticflickr.com/7248/7636698076_ccf7da7915.jpg" width="500" height="318" alt="hackbar-font-setting"></a>

如果想要自己改 source 話，也可以到 [HackBar Firefox Extension](http://code.google.com/p/hackbar/) 來載 source code
