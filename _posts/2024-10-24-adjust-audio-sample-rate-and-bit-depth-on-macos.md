---
layout: post
title: "在 macOS 下調整輸出音訊的取樣率 (Sample Rate) 和位元深度 (Bit Depth)"
---

在 macOS 裡，接上了比較好的外接音效卡 / DAC 之後，記得要打開 **應用程式 → 工具程式 → 音訊MIDI設定** ，在裡面設定一下取樣率和位元深度：

{% include image.md name="settings.png" %}

雖然直接設定成最高，讓系統自動升頻也不一定比較好聽，不過不知道怎麼設定，而一直維持在預設的話難免顯得有點虧。

另外查了一下發現 macOS 好像沒有原生支援 DSD 輸出的方法？不過我手上也沒有什麼 DSD 檔案就是了。

再來有看到 [vincentneo/LosslessSwitcher](https://github.com/vincentneo/LosslessSwitcher) 這個軟體，可以根據目前 Apple Music 播放歌曲的取樣率來調整系統的取樣率，不過……我也沒在用 Apple Music XD

然後 macOS 下的音效卡獨佔模式，就要看音樂播放軟體有沒有提供了，像我比較常用的 [supersonic](https://github.com/dweymouth/supersonic) 就有支援，翻了一下應該是依靠 [mpv](https://mpv.io/) 來處理的。
