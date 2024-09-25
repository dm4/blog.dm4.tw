---
layout: post
title: "用 iTerm2 的 Escape Sequence 來開啟網頁"
redirect_from: /2024/04/01/iterm2-escape-sequence-to-open-url.html
---

最近有個困擾了一陣子的問題：在用 [GitHub CLI](https://cli.github.com) 執行 `gh browse` 的時候，總是沒辦法正確的打開瀏覽器。

在 macOS 上會看到這樣的錯誤訊息：

```console
$ gh browse
Opening github.com/dm4/blog.dm4.tw in your browser.
https://github.com/dm4/blog.dm4.tw: No such file or directory
exit status 1
```

其實解法滿簡單的，只要指定好 `GH_BROWSER` 環境變數就可以，可以參考 [gh environment](https://cli.github.com/manual/gh_help_environment) ，在 macOS 下用 `open` 就可以用預設瀏覽器開啟：

```console
$ export GH_BROWSER=open
$ gh browse
Opening github.com/dm4/blog.dm4.tw in your browser.
```

不過因為我實在太常在遠端機器上工作，所以希望在遠端機器上也能夠正確開啟瀏覽器，因為我用的終端機是 [iTerm2](https://iterm2.com) ，最一開始想到的解法是用 iTerm2 的 [Triggers](https://iterm2.com/documentation-triggers.html) 功能，去偵測畫面上的 `Opening github.com/(.*) in your browser.` ，然後執行指令 `open https://github.com/\1` 來開啟本地的瀏覽器。

Triggers 的設定位置其實不太好找，是在 `Preferences` → `Profiles` → `Advanced` → `Triggers` ，如果是想在 tmux 裡執行 Triggers 的話，要記得把 `Enable in interactive apps` 打勾，不然 Triggers 不會生效。

因為 Trigger 是由第一行的輸出文字觸發的，不需要設定 `GH_BROWSER` ，但若不好好設定又會看到錯誤訊息，這時候可以用 `:` 這個 bash 內建指令來處理， `man bash` 可以看到用法：

```console
$ man bash
...
: [arguments]
        No effect; the command does nothing beyond expanding
        arguments and performing any specified redirections.
        A zero exit code is returned.
...
```

所以可以這樣設定：

```console
$ export GH_BROWSER="bash -c :"
$ gh browse
Opening github.com/dm4/blog.dm4.tw in your browser.
```

這樣子設定完之後是可以正常運作的，不過馬上就遇到了另一個問題：如果 tmux 切換 windows 或是 scroll pane 的時候， Triggers 會被重覆觸發……

本來是想找找看有沒有辦法讓 Triggers 觸發後能替換掉原本的文字，來避免重覆觸發，但可惜沒有找到類似的設定。

後來想從 escape sequence 下手，翻了 [iTerm2 Escape Codes](https://iterm2.com/documentation-escape-codes.html) 沒有看到類似的功能，本來打算先放棄，之後再來試試 [iTerm2 Scripting](https://iterm2.com/documentation-scripting-fundamentals.html) 的時候，搜到了 issue [Custom Control Sequence - Open URL](https://gitlab.com/gnachman/iterm2/-/issues/10994) ，裡面提到 commit [Add OSC 1337 OpenURL=:(base64-encoded URL). Issue 10994](https://gitlab.com/gnachman/iterm2/-/commit/fc9ae5c90f53cb1ed54d338a3bf1e09f22d22894) 已經 merge 了！

看起來功能就是我要的，所以就來試著看簡單的 escape sequence 能不能用，像是改游標形狀的指令：

```bash
printf "\e]1337;CursorShape=1\x07"
```

結果最簡單的嘗試馬上就失敗了……

在 iTerm2 裡正常的 shell 是能夠正常執行的，但只要一進 tmux 就會失效，因為 tmux 會把 escape sequence 給吃掉，所以要想辦法讓 tmux 把 escape sequence 往外傳給 iTerm2 裡面：

```bash
printf "\ePtmux;\e\e]1337;CursorShape=1\x07\e\\"
```

這樣就可以了，所以接著就是照格式把要打開的 URL base64 編碼後傳給 iTerm2 就可以了，簡單弄了一個 [iterm-open-url.sh](https://github.com/dm4/rc/blob/master/bin/iterm-open-url.sh) ，把它放到 PATH 底下，接著 `export GH_BROWSER="iterm-open-url.sh"` 就好了！

之前為了取代 ShellGPT 用了 iTerm2 的 command composer [^1] ，現在又多用了 iTerm2 特有的 escape sequence ，感覺用越多好像越容易被綁在 iTerm2 上……這種問題不知道有沒有什麼好的替代方案，不然可能就像 bashrc 越寫越複雜就不想換到 zsh ，或是 vimrc 寫好就不想換到 neovim 一樣…… 😢

[^1]: [用 iTerm2 的 Command Composer 取代 ShellGPT](https://blog.dm4.tw/replace-shellgpt-with-iterm2-command-composer)
