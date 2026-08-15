---
layout: post
title: "試著幫自己的網站加上驗證：VPN / Basic Authentication / Client Certificate / Single Sign-On"
---

之前在 [用過的記帳軟體們 - 2023 版](/personal-finance-software-2023) 這篇裡提到，把記帳的 fava 跑起來之後，有試了幾個方法來做認證，這篇來詳細記錄一下。

## VPN

應該是最直觀的？fava 架在家裡的 server 上，每次要看記錄的時候都連回家裡 VPN 看，缺點也滿明顯的，就是很麻煩……雖然可以在 iOS 上加一些捷徑自動連線，不過常常忘記斷開 VPN 導致網路不穩也是滿困擾的。

有些家用 Router 或是 NAS 都可以方便的把 VPN server 跑起來，也有試過 [WireGuard](https://www.wireguard.com/) 覺得滿方便的，這也是為什麼之前會研究 [WireGuard 新增 Client 的設定](/wireguard-add-client-configuration)……XD

## HTTP Basic Authentication

設定滿簡單的，把帳號密碼記錄在 htpasswd，再過一層 web server 來控管權限，過了 auth 之後再 reverse proxy 到真正的 fava server。

缺點大概是如果認證過期的時候要打密碼很煩，就算有密碼管理工具也是要在那裡等個兩三秒的 Face ID，急著要開網頁的時候簡直度日如年。

## HTTP Client Certificate

要用 self sign client certificate 的設定就比較複雜了，流程大概是：

- 先生出 `ca.key`
- 用 `ca.key` 生出 `ca.crt`，這就是我們自簽憑證 (self sign certificate) 的根憑證 (root certificate)
- 為每個想要使用的設備生一把 `client.key`（當然要重複使用也不是不行）
- 用 `client.key` 生出 `client.csr` 這個是要讓根憑證簽名的簽名請求 (certificate signing request)
- 只有我覺得用中文講根、自簽、憑證、簽名比中文還難看懂嗎？
  - <https://www.youtube.com/watch?v=abWGRc3wsEI>
- 用 `ca.key` `ca.crt` 來簽 `client.csr`，生出 `client.crt`
- 把簽好的 `client.crt` 加上本來的 `client.key` 打包變成 `client.p12`
- 把 `client.p12` 裝到目標設備上

用到的指令大概就是：

```bash
# ca
openssl genrsa -out ca.key 4096
openssl req -new -x509 -days 3650 -key ca.key -out ca.crt

# client
openssl genrsa -out client.key 4096
openssl req -new -key client.key -out client.csr
openssl x509 -req -days 365 -in client.csr -CA ca.crt -CAkey ca.key -set_serial 01 -out client.crt
openssl pkcs12 -export -clcerts -in client.crt -inkey client.key -out client.p12
```

缺點除了設定有點麻煩之外，也沒辦法臨時在別的裝置上瀏覽，之前剛好遇到 fava 在 iOS Safari 有 bug，結果 iOS 上的 Chrome 沒辦法正確使用裝在系統裡的 `client.p12` 😢

不過設定完之後不用考慮打密碼的問題真的很讚，如果沒有需要在其他裝置上瀏覽的話應該是首選！

## Single Sign-On (SSO)

後來也想要找有沒有辦法可以直接靠現有的 Google 帳號直接登入，看到這篇 [OAuth2 Proxy 為你的後台提供認證](https://weii.dev/oauth2-proxy/) 之後就試了 [OAuth2 Proxy](https://github.com/oauth2-proxy/oauth2-proxy)，設定也是有點小麻煩，不過架好、設定好之後只需要修改 `oauth2-proxy-emails.txt` 的內容就可以新增 / 刪除帳號，如果是要讓別人登入，或是常常需要新增 / 刪除帳號的話可以試試，當然也是需要使用者都有一個在 [OAuth2 Proxy / Providers](https://oauth2-proxy.github.io/oauth2-proxy/configuration/providers/) 列表裡的帳號就是了。

用 OAuth2 登入還有個好處是：就算自己的 OAuth2 Proxy 驗證到期了，重新跑一次登入流程在大部分的時候也都不用再打密碼，畢竟通常用到的服務提供商的 session 都沒這麼容易過期，像是用 Google 帳號的話就是點兩三個按鈕就可以完成登入。

另外 fava 可以用 `fava-sidebar-link` 來把登出的連結放到側邊欄上，雖然自己用的話好像完全不會需要登出 XD

---

試過幾種方式之後，目前覺得最適合自己的方法應該是：先用 client certificate 驗證，如果偵測到沒有 client certificate 的話就改用 OAuth2 Proxy 來驗證。

如果是用 Nginx 的話大概就是把 `ssl_verify_client` 設成 `optional`，之後再用 `$ssl_client_verify` 確定是不是有通過憑證的驗證，如果沒有成功驗證就再走 OAuth2 Proxy 驗證流程。

完全不用打密碼就是讚啦！
