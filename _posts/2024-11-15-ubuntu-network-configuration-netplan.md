---
layout: post
title: "Ubuntu 的網路設定 (Netplan)"
---

真的是太久沒有需要幫 Ubuntu 設定網路了，習慣性的想去編輯 `/etc/network/interfaces` 才發現竟然消失了 QQ 查了一下發現原來在 20.04 就 deprecated 了，改成使用 [Netplan](https://netplan.io/) ，設定檔是放在 `/etc/netplan` 下面的 YAML 檔案。

用法還算簡單，一般使用的話也不用看完官方文件就能動了，想直接看設定檔的範例可以參考 [官方的 examples](https://github.com/canonical/netplan/tree/main/examples) ，實用！
