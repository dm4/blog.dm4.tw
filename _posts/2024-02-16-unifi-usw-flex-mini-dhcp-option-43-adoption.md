---
layout: post
title: "Unifi USW Flex Mini DHCP Option 43 Adoption"
tags: []
redirect_from: /2024/02/16/unifi-usw-flex-mini-dhcp-option-43-adoption.html
---

因為買了 Flex Mini 之後發現沒辦法 ssh 進去 set-inform 來讓 controller 接管，所以先研究了一下 L2 Discover ，但是因為我 controller 開在 container 的 bridge network 裡，所以沒辦法用 L2 Discover （我猜用 host network 應該可以，不過沒實驗過），只好嘗試使用 DHCP Option 43 來接管裝置。

> Adopting a flex mini switch is different from all other UniFi devices as it doesn't support SSH so you have to adopt them via DHCP option 43 or local discovery L2 method. - [Adopting UniFi Flex Mini](https://help.cloudunifi.com/unifi/adopting-unifi-flex-mini/)

不過因為我不是用 Unifi 的 router ，查了 [RFC 2132](https://www.rfc-editor.org/rfc/rfc2132.html#section-8.4) 發現 DHCP Option Code 43 好像是給供應商自定義的選項？所以找了一下怎麼在 Mikrotik router 裡設定，後來在 Mikrotik forum 看到 [這篇](https://forum.mikrotik.com/viewtopic.php?t=150300#p740526) 裡的回應寫到：

> Also for UAP you need to add 0104 in front of hex IP address, in my example the value for option 43 should be 0104c0a8000f

大概就是以下幾個步驟：

- 把 controller IP 轉成 hex ，像是 `192.168.1.1` → `C0 A8 01 01`
- 在 Mikrotik 的 IP / DHCP Server 裡的 Options ，新增：
  - Name: `Unifi`
  - Code: `43`
  - Value: `0x0104<HEX>` ，以上面為例就是 `0x0104C0A80101`

![](/assets/images/2024-02-16/add-dhcp-option.png)

- 在 Mikrotik 的 IP / DHCP Server 裡的 Networks ，編輯你要用的 DHCP Server 的設定，在 DHCP Options 裡新增剛剛設定的 Unifi

![](/assets/images/2024-02-16/network-setting.png)

- 重開 Flex Mini ，就可以在 controller 看到 Flex Mini 了
