---
layout: post
title: "Llama 2 Prompt"
tags: [ai]
redirect_from: /2023/11/03/llama-2-prompt.html
---

如果不透過任何 wrapper ，直接操作 Llama2 的話，想要做到 system prompt 或是歷史對話的效果，其實是要用特殊的格式來建構 prompt 。

根據 [這篇 Hugging Face 的文章](https://huggingface.co/blog/llama2#how-to-prompt-llama-2) ，可以知道格式如下：

```text
<s>[INST] <<SYS>>
system_prompt
<</SYS>>

user_message_1 [/INST]
```

每次的 prompt 都是沒有 `</s>` 結尾的，在 model 回應之後，第二次的 prompt 就會長得像：

```text
<s>[INST] <<SYS>>
system_prompt
<</SYS>>

user_message_1 [/INST] model_resposne_1 </s><s>[INST] user_message_2 [/INST]
```

其中 `<<SYS>>\n` 和 `\n<</SYS>>\n\n` 裡的換行很容易忘了加，實際測試發現有正確加上換行效果真的比較好。

不過感覺這些特殊 tag 還是有機會出現在 user message 裡，不知道如果對話內容有這些 tag 會不會有問題？有空再來試試看好了。
