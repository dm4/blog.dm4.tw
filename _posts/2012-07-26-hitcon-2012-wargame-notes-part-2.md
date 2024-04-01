---
layout: post
title: "HITCON 2012 Wargame Notes - Part 2"
tags: 
- hit
- hitcon
- note
- hack
- web
- wargame
- security
redirect_from: /2012/07/26/hitcon-2012-wargame-notes-part-2.html
---

### Web 5

結果回家研究了一下，終於把 Web 5 也解出來了！好後悔在 HITCON 花太多時間在看
[駭客觀察日記](http://www.youtube.com/watch?v=l2dvg3KJoPo) 啊啊啊 orz

這題的 [提示二](http://www.exploit-db.com/wp-content/themes/exploit/docs/19527.pdf)
出來之後，可以發現長檔名的檔案/資料夾會因為 `~` 的縮寫而 disclosure ，所以可以寫個
[script](https://gist.github.com/3180618) 來踹，或是用 hint 裡有提到的 [iis-shortname-scanner-poc](http://code.google.com/p/iis-shortname-scanner-poc/)

不過試了之後發現 file disclosure 只限於有 `~` 的長檔名，
如果只是 6, 7 個字的檔案/資料夾是不會被套用縮寫的規則，所以也就踹不出來，
在 HITCON 的時候我們就是卡在這裡。

回家再查了多一點資料的時候，看到更多有關 [8.3 Filename](http://scilnet.fortlewis.edu/tech/NT-Server/File_Names.htm)
的介紹，其中提到：

> To disable short filename support, use the registration database editor, REGEDT32.EXE,
> and search the HKEY_LOCAL_MACHINE section for the entry HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\FileSystem.
> Change the value of the parameter NtfsDisable8dot3NameCreation to a value of 1 (one),
> and then short filename generation will be disabled.

所以就想到去試一下 <http://wg2.hack.idv.tw:2012/HKEY_LOCAL_MACHINE/System/CurrentControlSet/Control/FileSystem/NtfsDisable8dot3NameCreation/key.txt>
沒想到就...... orz
