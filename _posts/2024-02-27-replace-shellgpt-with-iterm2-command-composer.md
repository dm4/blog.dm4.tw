---
layout: post
title: "用 iTerm2 的 Command Composer 取代 ShellGPT"
tags: []
---

在這個到處都是 AI 的年代，不管用 VSCode 或是 Vim 都有超讚的 GitHub Copilot Extension 可以用，不過在終端機裡想要快速查指令還是沒有找到特別好用的工具。

一開始試了 Raycast 的 [ChatGPT Extension](https://www.raycast.com/abielzulio/chatgpt) ，對於拿來問簡單的問題滿方便的，不過因為我也是用 Raycast 在管理我的剪貼薄，如果是想在問題貼上之前複製的文字就很困擾，常常快速鍵按一按就不知道自己在 Raycast 的哪一層選單裡……

接著也試了 [Warp][] 這個很潮的終端機，各種 AI 功能都整合的很好，除了簡單的描述功能生成指令之外，也可以開一個側邊欄來和 AI 互動，我剛剛看網站好像有新的 Workflow 功能，看起來可以自動化一些常用的指令，連指令的補完也做的不錯，不過對我來說最大的缺點應該是對 tmux 的支援有點差，因為 [Warp][] 看起來會對每個指令和對應的結果做處理，不過在 tmux 裡面完全失效，官方有在 [這裡](https://github.com/warpdotdev/Warp/discussions/501) 討論支援 tmux ，不過看起來也是走 tmux contorl mode ，之前在 iTerm2 也有試過……不太符合我的使用習慣。

後來回到 iTerm2 又嘗試了 [ShellGPT][] ，是一套 Python 寫的小工具，讓你可以在 shell 裡方便的呼叫 OpenAI API 我最常用的也只是預設的 code role 用起來大概像：

```console
$ alias cgpt='sgpt --code'

$ cgpt 'how to remove all docker images'
docker rmi $(docker images -q) -f

$ cgpt 'in rust, how to convert Vec<u8> to String'
String::from_utf8(vec).unwrap()
```

對我來說其實就很夠用了，真的要說缺點可能就是在沒安裝 [ShellGPT] 的機器上，需要切回本機的 shell 來查指令。

為了想解決「要切回本機 shell 問問題」這件事，最近的嘗試是直接用 iTerm2 的 Command Composer 來取代 [ShellGPT][] ，本來 Command Composer 應該只是跳出一個文字框，讓大家打指令的時候比較方便編輯，或是方便貼上多行的指令來執行。

不過大概在一年前左右的 `3.5.0beta9` ，新增了 AI 相關的功能：

> iTerm2 3.5.0beta9 (OS 10.15+) - Built on January 2, 2023
>
> Add AI-powered natural language command
> generation. Enter a prompt in the composer and
> select Edit > Engage Artificial Intelligence.
> You will need to provide an OpenAI API key since
> GPT costs money to use.
>
> from [iTerm2 Changelog](https://iterm2.com/downloads.html)

在設定頁多了一個地方可以填入 OpenAI API Key 和想要的 Prompt

![](/assets/images/2024-02-27/iterm2-ai-settings.png)

Prompt 可以參考 [iTerm2 - AI Prompt](https://gitlab.com/gnachman/iterm2/-/wikis/AI-Prompt) 的說明，不過我自己是覺得預設的不太好用，所以直接抄了 [ShellGPT][] 的 [code role system prompt](https://github.com/TheR1D/shell_gpt/blob/d86d04c/sgpt/role.py#L30-L35)

```text
Provide only code as output without any description.
Provide only code in plain text format without Markdown formatting.
Do not include symbols such as ``` or ```python.
If there is a lack of details, provide most logical solution.
You are not allowed to ask for more details.
For example if the prompt is "Hello world Python", you should return "print('Hello world')".

\(ai.prompt)
```

設定好就可以用 **⌘ ⇧ .** 來叫出 Command Composer，然後打完問題之後按 **⌘ Y** 來生成指令，如果確定要執行再用 **⇧ ↩** 讓 shell 執行指令。

不知道會不會再踩到什麼雷，但目前試了一兩個星期好像還行，可以再觀察一下。

[Warp]: https://www.warp.dev
[ShellGPT]: https://github.com/TheR1D/shell_gpt
