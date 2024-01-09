---
layout: post
title: "用 GoAccess 視覺化 Nginx Access Log"
---

在 [用 Docker 架設 Mastodon](/2023/12/30/mastodon-with-docker.html) 之後，本來想說一樣來把 Nginx 的 access log 丟到 EFK 裡觀察一下狀況，沒想到在我把之前 EFK docker compose 裡的 image 版本升一升之後……就跑不起來了……看起來是遇到各種 ssl / service account token 問題 QQ

結果在處理問題的時候剛好有查到 [GoAccess][] 這個工具，可以把 web log 視覺化，預設也支援了滿多種 log 格式，試完之後就放棄把 EFK 跑起來了 XD

裝好 [GoAccess] 之後只要 `goaccess access.log -o report.html` 就可以生出靜態的網頁，十分簡單粗暴！如果想要即時更新的話可以參考 [Manual][] 裡的 `--real-time-html` ，如果想要保存歷史資料的話可以參考 `--persist` / `--restore` 的用法。

最後用 docker 跑起來的指令大概長這樣：

```bash
docker run -d \
  --name goaccess \
  --restart always \
  -u 33 \
  -v /var/log/nginx:/log:ro \
  -v $PWD/data:/data \
  -p 127.0.0.1:7890:7890 \
  -e TZ=Asia/Taipei \
  allinurl/goaccess /log/mastodon-access.log \
    -a -o /data/html/index.html \
    --real-time-html \
    --ws-url <HOSTNAME>:80/ws \
    --log-format COMBINED \
    --db-path /data/db \
    --restore \
    --persist
```

跑起來的樣子：

![](/assets/images/2024-01-09/screenshot.png)

其中特別處理的地方有：

- `-e TZ=Asia/Taipei` 是為了讓 [GoAccess][] 生出來的 html 報表的時間是 `Asi/Taipei` 的時區，不然預設會是 `UTC+0` 。

- 因為 Nginx 有做 log rotate，所以這裡直接把 `/var/log/nginx` 用 read-only 掛進去， [GoAccess] 會自己去讀取新的 access log 。

- 用 `-u 33` 是因為 log rotate 預設的權限是 `create 0640 www-data adm` ，要讓 container 裡也能讀到 log 所以把 uid 設成和 `www-data` 一樣。

- 有用了 `--persist` / `--restore` ，這樣在 container 重啟之後也可以保留歷史資料，不這麼做的話就只能看到最新的 access log ，或是把舊的 log file 也加在參數裡去讓 [GoAccess][] 重新 parse 。

- 用了 `--persist` / `--restore` 之後 [GoAccess][] 也會存下來 log file 的 inode 和上次讀到的位置，所以也不怕重啟 container 之後會重複 parse 。

- 因為不想在機器上單純開個 port 出來給 [GoAccess][] 的 WebSocket 用，所以用了 `--ws-url` 來指定 WebSocket 的位置，再用 Nginx 的 `proxy_pass` 把 `/ws` 接到 [GoAccess][] 的 WebSocket port 上：

```nginx
map $http_upgrade $connection_upgrade {
    default upgrade;
    ''      close;
}

server {
    listen 80;
    server_name <HOSTNAME>;
    root <PATH_TO_GOACCESS_HTML>;
    index index.html;
    location /ws {
        proxy_pass http://localhost:7890/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
        proxy_set_header Host $host;
    }
}
```

[GoAccess] 還有支援 `--geoip-database` 選項，看起來能在 html 裡面顯示 IP 的地理位置，之後有空再來試試。

**UPDATE** 試了 `--geoip-database` ，真的就是載下來給檔案路徑就好，也發現 [wp-statistics/GeoLite2-City](https://github.com/wp-statistics/GeoLite2-City) 有下載連結。

[GoAccess]: https://goaccess.io/
[Manual]: https://goaccess.io/man
