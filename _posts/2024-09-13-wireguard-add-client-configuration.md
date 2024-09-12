---
layout: post
title: "WireGuard 新增 Client 的設定"
---

記錄一下怎麼在 [WireGuard][wg] 裡新增一個 client ，我自己的使用上比較單純，只是想讓所有的 client 都能 tunnel 到 server ，然後從 server 對外連線，不是太在意 client 之間互相連線。

## 產生 Key Pair

設定檔案權限之後產生 public key 和 private key

```
umask 077
wg genkey > client.key
wg pubkey < client.key > client.pub
```

## Server Config

在原本 server 的 config 加上新的 client

```
...

[Peer]
PublicKey = <client.pub 裡的內容>
AllowedIPs = <要給 client 的 IP>/32
```

然後 restart wireguard

## Client Config

```
[Interface]
PrivateKey = <client.key 裡的內容>
Address = <要給 client 的 IP>/24
DNS = 1.1.1.1

[Peer]
PublicKey = <server 的 public key>
AllowedIPs = 0.0.0.0/0
Endpoint = <server 的 endpoint>
```

## 中間遇到的問題

- 不知道為什麼我 client 一定要設 DNS 不然不能用 QQ
- 重啟 wireguard 的時候發現 systemd 一直報錯，後來發現我之前可能是直接用 `wg-quick` 跑起來的，最後用 `wg-quick` 關掉之後統一從 systemd 啟動 wireguard

[wg]: https://www.wireguard.com/
