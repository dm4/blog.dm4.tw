---
layout: post
title: "用 ShellGPT 幫忙寫 Commit Message"
---

雖然裝了 [ShellGPT][] 一陣子，但其實真正用到的次數其實不多，主要是因為一些比較麻煩、或是需要來回修正的工作，還是丟到其他 app 聊天視窗來處理比較方便，有些不在 console 下的小任務也都有更適合的工具來處理，像是： [PopClip](https://github.com/hirakujira/ChatGPT-Grammar-Check-PopClip-Extension) / [Raycast](https://www.raycast.com/abielzulio/chatgpt) /[沉浸式翻譯](https://immersivetranslate.com/zh-TW/) 之類的。

最常用 [ShellGPT][] 的時候大概都是讓它幫忙生一些 one-liner commands ，用在像是 `ffmpeg` 這種參數完全記不起來的指令特別好用，像之前要產生 [更新 SliceMK ErgoDox Wireless 鍵盤 Firmware](/update-slicemk-ergodox-wireless-keyboard-firmware) 的 `og:image` 就有用到：

```bash
$ sgpt -c 'use ffmpeg to create a image with 1200x630 white bg and put keymap.png at the center of it'

ffmpeg -f lavfi -i color=c=white:s=1200x630 -i keymap.png -filter_complex "overlay=(main_w-overlay_w)/2:(main_h-overlay_h)/2" -frames:v 1 output.png
```

雖然官方有一些分析 log 的範例，像是：

```bash
docker logs -n 20 my_app | sgpt "check logs, find errors, provide possible solutions"
```

但分析完 log 之後，如果要來回討論可能的做法， `sgpt --chat` 用起來其實不太好用（至少對我來說有點麻煩 QQ ）。

不過像是官方產生 git commit message 的範例，我就覺得滿讚的：

```bash
git diff | sgpt "Generate git commit message, for my changes"
```

畢竟 commit message 生出來不滿意的話，還是可以自己手動修改，當成簡單的 template 也是不錯。

不過在這個基礎上，我也希望能多做到：

- 不要只看 `git diff` ，畢竟有些 diff 可能沒有要進 commit ，我的習慣是會 `git add -p` 完，用 `git diff --staged` 看確定要進 commit 的修改。
- 希望產生的 commit message 可以參考之前的格式，畢竟每個 repository 格式其實不太統一，希望也能把最近的 commit message 放進 prompt ，讓 model 參考一下格式。

最後整理成了兩個 bash functions ：

- `gpt-git-message` 會根據 staged diff 產生 commit message
- `gpt-git-commit` 用產生的 commit message 直接呼叫 `git commit` ，會打開編輯器讓我有機會再手動修改一下訊息，另外傳給 `gpt-git-commit` 的參數也會直接傳給 `git commit` ，例如要加上 sign off 的話，可以直接 `gpt-git-commit -s` 。

```bash
function gpt-git-message {
    local gitdiff=$(git diff --staged)

    if [ -z "$gitdiff" ]; then
        echo "No changes to commit."
        return 1
    fi

    if ! command -v sgpt &>/dev/null; then
        echo "sgpt command not found."
        return 1
    fi

    local gitlog=$(git log --oneline --no-decorate --format="%s" | head)
    local prompt=$(cat <<< "generate one-line git commit message following the previous git log format.

# previous git log (one commit each line)

\`\`\`
$gitlog
\`\`\`

# current git diff

\`\`\`
$gitdiff
\`\`\`
")
    sgpt -c "$prompt"
}

function gpt-git-commit {
    gpt-git-message || return $?
    local message=$(gpt-git-message)
    git commit -m "$message" -e "$@"
}
```

用了幾天都還算滿意，希望也能幫助到和我一樣，不太會寫 commit message 的人 XD

---

另外有空的話也還想試一下 [ShellGPT][] 的 [Function Calling](https://github.com/TheR1D/shell_gpt/discussions/416) ，有機會再來試試看好不好用！

[ShellGPT]: https://github.com/TheR1D/shell_gpt
