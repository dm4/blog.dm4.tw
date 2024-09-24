---
layout: post
title: "WezTerm 設定：讓 Tab 寬度填滿 Tab Bar"
---

這幾天終於認真把 terminal 換成 [WezTerm](https://wezfurlong.org/wezterm/) 了，雖然只是在 macOS 上設定好，不知道之後在 Windows 上會不會踩到什麼雷？

大致上就是把之前在 iTerm 2 喜歡 / 習慣的設定搬一搬，能用 lua 寫設定檔真是不錯，記錄一下目前有關 tab width 的設定。

在設定 `config.use_fancy_tab_bar = false`[^1]之後可以用 retro style 顯示 tab ，也就是會用 terminal fonts 來顯示，大概就像這樣：

{% include image.md name="retro.gif" %}

不過好像沒有找到可以讓 tab 寬度填滿整個 window width 的選項，只找到一個比較像的 issue:  [Add "stretch tabs to fill tab bar" option](https://github.com/wez/wezterm/issues/1914) ，不過看起來是在討論 fancy style 的 tab bar ？

最後決定自己處理，方法大概是：

- 啟動或是 resize 的時候把 tab cols 存進 `wezterm.GLOBAL`[^2]
- 在 `format-tab-title`[^3] 的時候算一下左右要多少空白

整理一下設定就是：

{% gist ecdc7edefa0746488292f1b6e6378714 %}

最後的結果大概長這樣：

{% include image.md name="result.gif" %}

[^1]: <https://wezfurlong.org/wezterm/config/lua/config/use_fancy_tab_bar.html>
[^2]: <https://wezfurlong.org/wezterm/config/lua/wezterm/GLOBAL.html>
[^3]: <https://wezfurlong.org/wezterm/config/lua/window-events/format-tab-title.html>
