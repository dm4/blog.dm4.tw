---
layout: post
title: "我怎麼整理音樂 (1) - MusicBrainz"
redirect_from: /how-i-organize-my-music-1
---

回 [上一篇](recent-updates-gitlab-obsidian-beets) 最後的問題： 24 天！

這三個星期的空閒時間裡，也仍然是在緩慢的在整理實體 CD ，雖然速度好像明顯的越來越慢，好像有點當初一頭熱想認真更新 blog 的感覺了……不過總共也處理了快一百張 CD ，好像差不多確定流程了，剛好可以來記錄一下。

（我在打上面這段的時候，一直在想我應該用 **CD** / **專輯** / **唱片** / **光碟** 來描述那個圓盤，最後懶的改了就決定用最一開始想到的 CD 吧）

我的目的很明確：想要把手上有的 CD 轉成無損的檔案，並且讓我可以在手機和電腦上串流收聽，所以第一步應該就是轉檔了對吧？

嗯……我一開始也是這麼想的，直到有一張 CD 在轉檔的時候發現軟體無法辨識，發現其實還是有些事情可以先處理的，就是確保這張 CD 的資訊是軟體可以從網路上抓到的，這裡的軟體可能像是 [XLD] 這種轉檔軟體，或是 [Beets] 這種管理系統。而目前看起來好像還是 [MusicBrainz] 比較多人用以及比較多地方支援。

如果要簡單介紹 [MusicBrainz] 的話，我覺得大概就像音樂版的維基百科了吧？註冊完帳號之後就可以試著貢獻資料，幫忙補完喜歡的演出者的專輯清單，或是幫喜歡的專輯找個封面圖之類的。

不過其實網站的介面不太友善，加上很多欄位剛開始實在是不知道要填什麼，新手實在是太容易不小心就弄錯東西，和大家分享一些我使用 [MusicBrainz] 的心得：

#### Release & Release Group

- 先分享我覺得 [MusicBrainz] 有一個最容易搞錯的地方： release group 和 release 不同！因為一張專輯可能有很多不同的發行版，可能是不同國家由不同廠商發行，也可能是經由不同媒體發行，像是：光碟、黑膠、數位檔案，這些不同的發行都是一種 release ，這些 release 都應該要在同一個 release group 裡。
- 如果在新增 release 的時候 release group 是白底，就代表你沒有選到任何已存在的 release group ，而是會新增一個！就算明明就已經有一個同名的 release group 存在還是會新增一個同名的，這樣通常不會是你預期的結果，會變成重覆的 release group ，所以注意在 release group 欄位填完要等網頁跳出選項，選完之後綠底的 release group 才代表你現在新增的 release 會被加到已經存在的 release group 裡
- 舉個例子：下面這樣會新增一個 release group
{% include image.md name="1.png" %}
- 然後下面這樣才是會新增 release 到已經存在的 release group 裡
{% include image.md name="2.png" %}
- 我覺得最保險的方法是先到 release group 頁，然後點選右邊側欄裡的 `Add release` ：
{% include image.md name="3.png" %}

#### Edit / Vote

- 一般來說，新增資料或是修改大小寫之類的動作是會直接生效的，叫做 [auto-edits](https://musicbrainz.org/doc/Edit#Auto-edits)
- 但對現有的資料進行編輯，就會進到 [voting process](https://musicbrainz.org/doc/Introduction_to_Voting) ，如果沒有人反對， 7 天之後你的修改一樣是會生效

#### Sort Name

- 演出者 (artist) 的 sort name 是拿來排序用的字串，如果是中文演出者，不知道要填什麼的話可以先填中文（雖然 Guideline 3. 說只能用拉丁母），如果有英文名稱的話也可以填在這裡
- 有一些特殊條件，像是 `The Beatles` 會希望是被排在 B 開頭的，所以他的 sort name 是 `Beatles, The`
- 因為英文的姓名和中文的順序是反過來的，像 `Eric Clapton` 的 sort name 會是 `Clapton, Eric` ，但 `鄭宜農` 的 sort name 應該是 `Cheng, Enno` 才對
- 更多詳細規則可以參考 [sort name style guildline](https://musicbrainz.org/doc/Style/Artist/Sort_Name)

#### Packaging

- [官方給的包裝介紹](https://musicbrainz.org/doc/Release/Packaging)
- 我也是看了才知道，原來常見的那種兩折或三折的厚紙板，黏上塑膠光碟底座做成的包裝叫做 [Digipak](https://musicbrainz.org/doc/Release/Packaging/Digipak)
- 還是最喜歡 Jewel Case / Slim Jewel Case / Digipak 的包裝，不然超多特別的包裝好難收納……😢

#### Recording / Track

- 官方的 [recording](https://musicbrainz.org/doc/Recording) 說明
- Track 則是指 release 裡的歌曲
- 因為像是精選輯裡的歌曲 (Track) 版本可能會和某張專輯裡的一樣，應該被視為同一個 recording
- [International Standard Recording Code (ISRC)](https://musicbrainz.org/doc/ISRC) 是跟著 recording 的
- [台灣的 ISRC 查詢系統](https://isrc.ncl.edu.tw/C200/C200)

#### Disc ID

- [Disc ID](https://musicbrainz.org/doc/Disc_ID) 是用實體 CD 裡的曲目數量、每首歌的長度、在 CD 裡的 offset 算出來的
- 算法可以參考 [Disc ID Calculation](https://musicbrainz.org/doc/Disc_ID_Calculation)
- 我覺得手上有實體 CD 的話， disc ID 應該是最值得貢獻的資料了，畢竟網路上查不到，而且幾乎可以用來穩定的辨識專輯
- 用 [MusicBrainz Picard][Picard] 可以從光碟機或是 [XLD] / [EAC] 轉檔時的 log 來拿到 disc ID ，或是有光碟 TOC 資料後自己算

#### a‐tisket

- [a‐tisket] 是可以從 Deezer / Spotify / iTunes 抓資料，匯入 [MusicBrainz] 的小工具
- 強力推薦熟悉 [MusicBrainz] 之後再使用，不然很容易填錯東西……
- 特別是 release group ！最好先確定一樣的 release group 是不是已經存在，因為預設只會開一個新的，不會幫你確認有沒有重覆（弄錯了要合併就又要等 7 天的投票……）
- 通常加入的 release ， format 應該填上 `Digital Media` ，數位串流的 release 和實體專輯應該在同一個 release group 裡
- 可以幫忙拿到 Deezer / Spotify / iTunes 上的專輯封面連結，讓你可以方便的新增到 [MusicBrainz] ，通常是 iTunes 上的圖片解析度最高
- 也可以幫忙新增 ISRC 到 [MusicBrainz]
- 其實我一開始不知道 [Deezer](https://www.deezer.com/) 這個服務，看起來賣點是付費用戶能夠聽到 CD 品質的無損檔案？不過台灣目前不能用就是了

#### 我的處理流程

如果要確定整理的 CD 有沒有在 [MusicBrainz] 上，我通常會：

- 先到 [MusicBrainz] 搜看看有沒有 CD 的演出者，
- 看演出者有沒有目標 CD 的 release (release format 為 `CD`)
- 沒有的話就用 [a-tisket] 新增該 CD 的 `Digital Media` release
- 如果 [MusicBrainz] 已經有該 CD 的 `Digital Media` release ，就在該 release group 下點選 `Add release` （沒錯這樣做是在避免重覆的 release group ……真的踩過太多次雷）
- 在 format 為 CD 的目標 release 上加上 disc ID

#### 其它

最後分享一些我覺得寫得很好的 [MusicBrainz] 介紹：

- [MusicBrainz 不完全使用指南](https://www.himiku.com/archives/musicbrainz.html)
- [再见，所有的音乐订阅](https://emptystack.top/good-bye-music-subscriptions/)
- [音乐整理博士​​或：我如何停止担忧并爱上甜菜](https://emptystack.top/dr-musicuration/)

---

下一篇預計是分享 [XLD] ，應該字就會少一點了……吧？

[MusicBrainz]: https://musicbrainz.org/
[XLD]: https://tmkk.undo.jp/xld/index_e.html
[Beets]: https://beets.io/
[a‐tisket]: https://atisket.pulsewidth.org.uk/
[Picard]: https://picard.musicbrainz.org/
[EAC]: https://www.exactaudiocopy.de/
[a-tisket]: https://atisket.pulsewidth.org.uk/
