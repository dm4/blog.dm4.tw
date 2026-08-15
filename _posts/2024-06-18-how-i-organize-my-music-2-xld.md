---
layout: post
title: "我怎麼整理音樂 (2) - XLD"
---

{% include how-i-organize-my-music-toc.md %}

確定手上的 CD 都在 [MusicBrainz] 上面有資料就可以開始轉檔啦！

因為我主要都在 macOS 上作業，所以是用 [XLD] 來轉檔，用 Windows 的話或許可以試試 [EAC]，不過我不太熟就是了。

[XLD] 其實能設定的選項也不多，在巴哈上找到的這篇就已經講的足夠詳細了：

- [【密技】取得 CD-FLAC+100%LOG+CUE 教學(XLD 配置教學)](https://home.gamer.com.tw/artwork.php?sn=5394112)

不過因為文章是 2021 年的，新版本在介面上略有不同，可能有些需要找一下，一樣來分享我的使用心得：

#### Files

照上面的設定，轉完檔應該是會拿到數個 `.flac` 檔以及 `.cue`, `.log` 各一個，雖然 `.cue` 和 `.log` 應該是用不到了，但我還是先留著，之後用 Beets 匯入 `.flac` 的時候也會一起放在同個資料夾下。

#### Errors

轉完檔我覺得最重要的是要看轉檔的時候有沒有遇到問題，在 log 檔案裡的 `All Tracks` 可以看整張 CD 轉檔時有沒有遇到什麼問題：

```text
All Tracks
    Filename : /Users/dm4/Music/XLD/BADA88.flac
    CRC32 hash               : 7B5B4343
    CRC32 hash (skip zero)   : 91103420
    Statistics
        Read error                           : 0
        Jitter error (maybe fixed)           : 0
        Retry sector count                   : 0
        Damaged sector count                 : 0
```

沒意外的話通常都會是 0，如果太多錯誤可能就要注意一下 CD 是不是有髒汙，或是已經有刮傷了 😢

#### AccurateRip

Log 裡面有一段 `AccurateRip Summary`，是會把每首歌都算出 signature，然後和轉過同一張 CD 比較，不過比較可惜的是 [XLD] 可以去抓 AccurateRip 資料庫的內容，但好像不能幫忙上傳新資訊？

在 [AccurateRip Software](https://www.accuraterip.com/software.htm) 上面的 macOS 軟體除了 [XLD] 的連結都失效，要幫忙上傳 AccurateRip 資料可能要靠 [dBpoweramp](http://www.dbpoweramp.com/dmc.htm) 或 [EAC] 了。

如果比較多人轉過的 CD 的 AccurateRip 可能會長這樣：

```text
AccurateRip Summary (DiscID: 0028436c-02021af7-06100d12)
    Track 01 : OK (v1+v2, confidence 400/818, with different offset)
    Track 02 : OK (v1+v2, confidence 400/820, with different offset)
    Track 03 : OK (v1+v2, confidence 400/819, with different offset)
    Track 04 : OK (v1+v2, confidence 400/819, with different offset)
    Track 05 : OK (v1+v2, confidence 400/821, with different offset)
    Track 06 : OK (v1+v2, confidence 400/820, with different offset)
    Track 07 : OK (v1+v2, confidence 400/819, with different offset)
    Track 08 : OK (v1+v2, confidence 400/818, with different offset)
    Track 09 : OK (v1+v2, confidence 400/818, with different offset)
    Track 10 : OK (v1+v2, confidence 400/817, with different offset)
    Track 11 : OK (v1+v2, confidence 400/820, with different offset)
    Track 12 : OK (v1+v2, confidence 400/817, with different offset)
    Track 13 : OK (v1+v2, confidence 400/817, with different offset)
    Track 14 : OK (v1+v2, confidence 400/816, with different offset)
    Track 15 : OK (v1+v2, confidence 400/816, with different offset)
    Track 16 : OK (v1+v2, confidence 400/811, with different offset)
    Track 17 : OK (v1+v2, confidence 400/813, with different offset)
        ->All tracks accurately ripped.
```

裡面的 `confidence 400/818` 是指這曲歌有 818 個人上傳過 signature，有 400 個人和你轉出來的 signature 一樣，通常這裡只要 > 0 我覺得就可以安心了，畢竟要有人轉壞，還要壞軌的地方和你一樣也不容易？

不過更多的時候會長下面這樣就是了……

```text
AccurateRip Summary
    Disc not found in AccurateRip DB.
```

#### Copy Control CD

就是一種內建防拷貝系統的 CD，可以參考[維基的說明](https://zh.wikipedia.org/zh-tw/Copy_Control)。

到目前我只轉過一張 Copy Control CD，一樣設定的 [XLD] 轉了四五個小時，轉出來的 log 裡顯示一堆錯誤，AccurateRip 也沒有比對成功，不過最後的音樂檔案播起來都是正常的，我目前還搞不太懂……

全部 NG 的 AccurateRip：

```text
AccurateRip Summary (DiscID: 001f8a3d-013d6694-ca11560e)
    Track 01 : NG (total 433 submissions)
    Track 02 : NG (total 432 submissions)
    Track 03 : NG (total 430 submissions)
    Track 04 : NG (total 432 submissions)
    Track 05 : NG (total 432 submissions)
    Track 06 : NG (total 434 submissions)
    Track 07 : NG (total 488 submissions)
    Track 08 : NG (total 434 submissions)
    Track 09 : NG (total 439 submissions)
    Track 10 : NG (total 485 submissions)
    Track 11 : NG (total 434 submissions)
    Track 12 : NG (total 436 submissions)
    Track 13 : NG (total 432 submissions)
        ->0 track accurately ripped, 13 tracks not
```

轉出來超多 error：

```text
All Tracks
    Album gain               : -9.43 dB
    Peak                     : 0.999969
    Statistics
        Read error                           : 0
        Jitter error (maybe fixed)           : 0
        Retry sector count                   : 25
        Damaged sector count                 : 2322
```

#### TOC

在 log 最前面可以看到 CD 的 TOC，裡面包含了 CD 裡每首歌從 start / end sector 可以知道歌曲在 CD 裡的開始位置以及歌曲長度，大部份比較兩張 CD 是不是同一張都是用 TOC 的資訊。

```text
TOC of the extracted CD
     Track |   Start  |  Length  | Start sector | End sector
    ---------------------------------------------------------
        1  | 00:00:00 | 02:33:44 |         0    |    11518
        2  | 02:33:44 | 02:37:53 |     11519    |    23346
        3  | 05:11:22 | 02:40:04 |     23347    |    35350
        4  | 07:51:26 | 03:16:61 |     35351    |    50111
        5  | 11:08:12 | 02:23:56 |     50112    |    60892
        6  | 13:31:68 | 04:27:50 |     60893    |    80967
        7  | 17:59:43 | 02:56:70 |     80968    |    94237
        8  | 20:56:38 | 03:27:66 |     94238    |   109828
```

不過可以想見 TOC 其實是有機會有重覆的，特別是在單曲 CD 裡，所以也只是做個參考。TOC 的資訊在算 disc ID 的時候會用到。

#### Disc ID

用 [MusicBrainz Picard][Picard] 可以從光碟機或是 [XLD] / [EAC] 轉檔時的 log 來拿到 disc ID，不過就為了用他的這個小功能而裝整個 [Picard] 實在是有點太麻煩，看了一下其實用到的 code 在：

- [picard/disc/utils.py](https://github.com/metabrainz/picard/blob/master/picard/disc/utils.py)
- [picard/disc/eaclog.py](https://github.com/metabrainz/picard/blob/master/picard/disc/eaclog.py)
- [picard/util/\_\_init\_\_.py](https://github.com/metabrainz/picard/blob/master/picard/util/__init__.py)

後來就拼拼湊湊弄了一個獨立的 script 來做，dependency 就只有 [discid](https://pypi.org/project/discid/)，放在這裡：[xldlog.py](https://gist.github.com/dm4/dbf0b7d3fc383fa3384ef6f797df20dd)。

（雖然叫 `xldlog.py` 但也是可以處理 [EAC] log 啦 😅）

跑起來大概長這樣：

```console
$ ./xldlog.py BADA88.log

Disc ID:
iq8PQhqwbha1jrd4N.n5BuXE13E-

TOC:
1 8 109979 150 11669 23497 35501 50262 61043 81118 94388

Submit to MusicBrainz:
http://musicbrainz.org/cdtoc/attach?id=iq8PQhqwbha1jrd4N.n5BuXE13E-&tracks=8&toc=1+8+109979+150+11669+23497+35501+50262+61043+81118+94388
```

後面 `Submit to MusicBrainz` 的連結就和用 [Picard] 加 disc ID 的連結一樣，一開始要熟悉一下介面，在 `Search by artist` 之後，再接著 `Select` 目標演出者，最後才會出現能夠 `Attach CD TOC` 的頁面。

先搜尋一次出現：

{% include image.md name="1.png" %}

選擇之後才會出現：

{% include image.md name="2.png" %}

[MusicBrainz] 在這頁好像是會顯示歌曲數目和你 TOC 一樣的 release 讓你選擇，然後可以看到 `Digital Media` 是沒辦法新增 disc ID 的。

選好 release 再按 `Attach CD TOC` 就行了，每次新增 disc ID 都讓我覺得自己對人類社會又做了那麼一點點的貢獻，我真棒。

---

下一篇應該就是 [Beets](https://beets.readthedocs.io/) 了，感覺又會是一篇很長的心得……啊還是我可以分上下集？

[MusicBrainz]: https://musicbrainz.org/
[XLD]: https://tmkk.undo.jp/xld/index_e.html
[EAC]: https://www.exactaudiocopy.de/
[Picard]: https://picard.musicbrainz.org/
