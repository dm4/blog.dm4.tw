---
layout: post
title: "處理 PN51 的 Freeze 問題"
---

> 記錄一下去年處理的問題，清理一下我的 `blog/_drafts` ……

不知道是從什麼時候開始，我的 PN51 就常常不定時的 freeze ，就是那種整台機器畫面會停在最後一個狀態，滑鼠鍵盤怎麼按都沒有反應，只能長按電源鍵來關機的當機狀況。

升級了 OS / Kernel 也沒有解決，認真記錄了一下溫度也不太像是過熱，跑了 `stress-ng` 沒辦法重現問題，調整了 C-State 設定也沒有幫助，所以一直以為是記憶體出問題，拖了很久也一直懶得去跑完整的 Memory Test。於是變成一個禮拜到一個月之內，一定要手動重開機的狀況。

終於拖到某一天，剛好有空在重開機的時候，順便跑了 [MemTest86](https://www.memtest86.com/) ，結果花了一個晚上跑了測試，發現記憶體竟然完全沒事，完全打壞我的美好想像，我本來是想說可以：找到哪條記憶體壞了 → 拔掉 → 修好了棒棒，不是記憶體問題的話我就……又繼續放了一陣子。

後來哪天在研究的時候突然查到這篇 [Random system freeze on PN51-E1](https://forum.proxmox.com/threads/random-system-freeze-on-pn51-e1.117493/page-4) （我也不記得是不是因為想整台重灌成 Proxmox 才查到的），看到有人的 PN51-E1 升級了之後有解決他的 freeze 問題！

> My best uptime with this issue is 11days and 10h.
> So far the system is running for **26 days, 18:54:52** after the bios update. Hopefully this will solve the freezes.

雖然我的 PN51 是比較舊的型號，查了一下發現其實也有新版 BIOS 可以升級，直接從 0401 版 (2021/05/31) 升到 0415 版 (2023/12/11) ，我是 2021/07 買的，出廠兩年之後的升級，我竟然又在兩年之後才跟上……

升級之後就沒問題了，果然還是太菜，沒有想到可能是 BIOS 問題，學習了。

（是說我也很好奇，是不是現在請 LLM 幫忙上網研究的話可以直接搜到這篇關鍵貼文啊？我應該很久之前試過，不過當時提出的建議竟然沒有升級 BIOS ，簡直跟我一樣菜。）
