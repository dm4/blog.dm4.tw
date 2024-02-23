---
layout: post
title: "Starknet 的 Developer Airdrop"
tags: []
---

最近看到有人分享了因為貢獻 open source 專案而領到 Starknet 的 airdrop ，想說來看一下實際的分配機制是什麼。

![](/assets/images/2024-02-23/airdrop.jpeg)

根據 <https://provisions.starknet.io> 裡的說明，和開發者比較相關的是：

> **Starknet developers** You made at least three commits to a repository that appears in the Electric Capital report for Starknet before November 15th, 2023. At least one of these commits occurred during 2018 or later.
>
> **Ethereum developers** You made at least three commits to a repository that appears in the Electric Capital report for Ethereum before November 15th, 2023. At least one of these commits occurred during 2018 or later.
>
> **GitHub developers** You made at least three commits to a repository that is one of the top 5,000 repositories worldwide, ranked by number of stars, before November 15th, 2023. At least one of these commits occurred during 2018 or later.

說明裡提到的 Electric Capital report 應該就是指 <https://www.developerreport.com> ，這個 dashboard 裡的開發者名單應該是依據 [electric-capital/crypto-ecosystems][] 這個 repository 裡的設定，其中的 [starknet.toml][] 和 [ethereum.toml](https://github.com/electric-capital/crypto-ecosystems/blob/master/data/ecosystems/e/ethereum.toml) 定義了 Starknet 和 Ethereum 的相關專案，開發者名單應該就是依據這些專案的 commit 記錄去拿的。

比較有趣的是在 [starknet.toml][] 裡也定義了一些從別的地方 fork 來的 repository ，像是 Substrate 或是 go-ethereum ，這樣的話有些 Polkadot 或 Ethereum 開發者可能會因此撿到意外的 STRK ？

不確定還有沒有其他地方也會照著 [electric-capital/crypto-ecosystems][] 的定義來拿開發者名單，就很容易被大家發 PR 亂塞一堆 repository 進去……

[starknet.toml]: https://github.com/electric-capital/crypto-ecosystems/blob/master/data/ecosystems/s/starknet.toml
[electric-capital/crypto-ecosystems]: https://github.com/electric-capital/crypto-ecosystems
