---
layout: post
title: "用 Docker 架設 Mastodon"
tags: [docker]
redirect_from: /2023/12/30/mastodon-with-docker.html
---

之前就一直想試試看 [Mastodon](https://joinmastodon.org/) ，最近有空成功用 Docker 跑起來，其實滿容易的，網路上也可以找到詳細的教學，主要就是用 [官方提供的 docker-compose.yml](https://github.com/mastodon/mastodon/blob/main/docker-compose.yml) 來執行。

架設的部份大致上參考了：

- [How to Set Up a Mastodon Server](https://www.liquidweb.com/kb/how-to-set-up-mastodon-server/)

- [在 Docker 安裝、運行 Mastodon](https://blog.zeroplex.tw/2022/11/20/install-and-run-mastodon-on-docker/)

- [Configuring your environment](https://docs.joinmastodon.org/admin/config/)

設定的部分我是自己 `cp .env.production.sample .env.production` 之後再修改設定，沒有利用官方提供的 `bundle exec rake mastodon:setup` 。

---

跑起來之後遇到第一個問題是會不斷 redirect 回首頁，查了一下發現官方 github 有 [相關的討論](https://github.com/mastodon/mastodon/discussions/19544) ，原因是我在 mastodon 前面還有一層 nginx proxy 在處理所有的 https certificates ，這個 certificates proxy 是用 http 連到 mastodon 的，所以 mastodon 會不斷 redirect 到 https ，所以要在 nginx 設定裡欺騙 mastodon ，讓它認為自己是 https 。解法是參考：

- [How to run a Mastodon instance behind a reverse proxy](https://blog.vyvojari.dev/mastodon-behind-a-reverse-proxy-without-ssl-https/)

也就是在 nginx 設定裡把 `proxy_set_header X-Forwarded-Proto $scheme;` 改成 `proxy_set_header X-Forwarded-Proto https;` 就好。

---

再來想要換 logo 的時候發現不能上傳圖，這個是 docker volume 的權限問題，簡單的 `chmod` 就可以解決，解法可以參考官方 github issue:

- [Can't upload images · Issue #3676 · mastodon/mastodon · GitHub](https://github.com/mastodon/mastodon/issues/3676)

---

之後按一按發現管理介面裡有個 PgHero 的 dashboard 有跳 warning `Query stats must be enabled for slow queries` ，查了一下是 Postgres 的設定問題，解法參考：

- [Enable query stats in Mastodon with postgres](https://peterbabic.dev/blog/enable-query-stats-mastodon-postgres/)

大概就是進去資料庫裡下個指令，再改一下設定檔之後重開就好了。

---

最後的結果在 <https://social.dm4.tw/> 。
