---
layout: post
title: "小額付費簡訊詐騙"
date: 2013-11-09 12:22
comments: true
categories: android
og:
    image: http://farm3.staticflickr.com/2835/10746549465_c046942cc1.jpg
---

最近好像很流行收到這種奇怪的簡訊

<a href="http://www.flickr.com/photos/51077287@N06/10746549465/" title="Flickr 上 sunrisedm4 的 詐騙簡訊"><img src="http://farm3.staticflickr.com/2835/10746549465_c046942cc1.jpg" width="333" height="500" alt="詐騙簡訊"></a>

用 iPhone 點開網址之後發現會被導到一個已經關閉的無名相簿，研究了一下發現他會判斷 `User-Agent` 來決定要導到哪裡，
如果把 `User-Agent` 設成 Android 瀏覽器的話，會被導到一個下載 Andorid apk 的連結

不過大概是新聞報很大的原因，詐騙連結幾乎都已經失效了

把載下來的 apk 拆開會發現它是一個偽裝成 Google Play Service 的程式
（要拆 apk 推薦使用 [androguard](http://code.google.com/p/androguard/) ，它的 Sublime Text plugin 簡直是神器......）

使用到的權限和用到的 components

{% gist c392ca89782e53ce7915 AndroidManifest.xml %}

可以看到這個 app 要了一些邪惡的權限，
並且有個 receiver 會偵測 `android.provider.Telephony.SMS_RECEIVED` 這個動作，
在收到簡訊的時候被觸發，是和小額付費比較有相關的地方

{% gist c392ca89782e53ce7915 me.java %}

可以看到在收到新簡訊的時候，會判斷內容有沒有包括特定字串，
有的話就會把整個簡訊內容回傳，如果沒有的話只會回傳前六個字元

再往下追可以找到回報資料的伺服器位置和一些東西，不過當然也失效了

{% gist c392ca89782e53ce7915 l.java %}

裝了惡意程式之後會莫名其妙的被盜用小額付費的原因，
我**猜**應該是小額付費的認證簡訊會包含特定內容，把整封簡訊回傳是為了要拿到小額付費的認證碼，
如果不是小額付費的簡訊，這隻惡意程式會回傳前六個字元的原因，
我**猜**可能是覺得簡訊一開頭很有可能有人名吧，
一但拿到人名 + 電話，就又多了一個詐騙的目標......

