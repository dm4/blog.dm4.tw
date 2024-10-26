---
layout: post
title: "用指令取得音樂檔案的取樣率 (Sample Rate) 和位元深度 (Bit Depth)"
---

分享幾個指令，想要知道音樂檔案的取樣率和位元深度的話可以用 `ffprobe`：

```bash
# 看所有參數
ffprobe -show_streams <file>

# 看指定欄位
ffprobe -show_entries stream=sample_rate,bit_rate,bits_per_raw_sample <file>

# 如果覺得前面有一段 log 很煩可以用 -v 調 log level
ffprobe -v error -show_entries stream=sample_rate,bit_rate,bits_per_raw_sample <file>
```

其中的 `bits_per_raw_sample` 就是位元深度 (bit depth) ，而 `bit_rate` 是位元率，位元率的單位是 bit per second (bps) ，像一個 FLAC 檔案的結果可能是：

```text
[STREAM]
sample_rate=44100
bit_rate=N/A
bits_per_raw_sample=16
[/STREAM]
```

不過如果 AAC 格式的檔案，因為是有損的壓縮編碼，所以並沒有固定的 bit depth ，指令的執行結果可能會長這樣：

```text
[STREAM]
sample_rate=44100
bit_rate=272960
bits_per_raw_sample=N/A
[/STREAM]
```
