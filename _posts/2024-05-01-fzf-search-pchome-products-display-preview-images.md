---
layout: post
title: "用 fzf 搜尋 PChome 的商品並顯示預覽圖"
---

最近終於開始認真把常用的環境都改用 zsh ，也想順便熟悉一些小工具來取代內建的指令，像是用 [eza] 來取代 `ls` ，用 [fzf] + [fd] 來取代 `find` 。

雖然之前就裝過 [fzf] ，不過通常就是用預設的 Ctrl-R 來搜尋歷史指令，最近認真設定了 fzf 的 preview 之後發現真的很讚，搭配一些指令就可以做到像是這樣的效果：

[![][gif1]][gif1]{:target="_blank"}

可以在 command line 搜尋 PChome 上的商品，而且還可以顯示預覽圖！也可以多選之後一次打開：

[![][gif2]][gif2]{:target="_blank"}

整個 function 其實滿簡單的：

{% gist 1ea4417718e0369404caae1e4677484b %}

主要解釋一下每一步做的事情：

- 用 `mktemp` 拿到暫存檔的路徑，之後放搜尋結果
- 用 `curl` 拿到搜尋結果
- 用 `jq` 把搜尋結果整理成 `價格 名稱:::網址:::圖片` 的格式
- 用 `awk` 把後面的 `:::網址:::圖片` 套上比較不明顯的顏色，這裡主要是為了讓結果好看一點，也可以把顏色調整成完全看不到後面的網址和圖片連結
- 用 `fzf` 來搜尋，並且用 `--preview` 來顯示圖片，這裡用到了 [Inline Images Protocol](https://www.iterm2.com/documentation-images.html) 來顯示圖片
  - 我用的不是 iTerm2 自己的 [imgcat script](https://iterm2.com/utilities/imgcat) 而是用 [imgcat on PyPI](https://pypi.org/project/imgcat/)
- 用 `awk` 把選擇的結果中的網址拿出來，並且用 `xargs` 呼叫 `iterm-open-url.sh` 來開啟瀏覽器
  - 會用 `iterm-open-url.sh` 主要是為了能夠在遠端機器也能夠開啟本機的瀏覽器，可以參考 [用 iTerm2 的 Escape Sequence 來開啟網頁](/iterm2-escape-sequence-to-open-url)

[eza]: https://github.com/eza-community/eza
[fd]: https://github.com/sharkdp/fd
[fzf]: https://github.com/junegunn/fzf
[gif1]: /assets/images/fzf-search-pchome-products-display-preview-images/1.gif
[gif2]: /assets/images/fzf-search-pchome-products-display-preview-images/2.gif
