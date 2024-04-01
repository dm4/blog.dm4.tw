---
layout: post
title: "幫 ASUS PN51 換 Asaka Newton A50 散熱機殼"
tags: [hardware]
redirect_from: /2023/10/22/asus-pn51-asaka-newton-a50.html
---

最近把拿來架伺服器的 [ASUS PN51][] 搬進書房之後，發現負載稍微大一點的時候風扇聲有點惱人，於是就想說來換個無風扇的散熱機殼。選擇的是 Asaka 出的 [Newton A50][] ，不過其實我本來是覺得 [Turing A50 MKII][] 比較好看，不過不知道是不是絕版了？找了 Amazon 發現買不到，所以就換成 [Newton A50][] ，後來覺得 [Newton A50][] 也還不錯，畢竟對寸土寸金的書房來說 [Turing A50 MKII][] 可能還是稍大了一點。

換殼的大部份時間都是在拆原本的機殼，主要是參考下面兩個連結：

- <https://linustechtips.com/topic/1271386-asus-pn50-ryzen-3-4300u-teardown/>

- <https://www.youtube.com/watch?v=zcErxnolYFo>

上面兩個連結雖然都是拆 PN50 ，不過其實 PN51 幾乎是一模一樣，第一個連結的文章裡的圖片真的是滿清楚的，看圖片之後如果還不太確定要怎麼施力，可以再參考第二個連結的影片，雖然我是覺得幫助不大。

裝進 [Newton A50][] 的步驟相較之下就簡單多了，基本上說明書講得很清楚，因為我沒有要裝 2.5 吋硬碟又省了一些步驟（那條 SATA 線不知道好不好裝），分享一下裝完之後的幾點心得：

- 在開始拆之前可以先進 BIOS 把 CPU FAN 調成 Ignore ，畢竟裝完之後可能手忙腳亂的，這時候還要找線找鍵盤有點麻煩。

- 原本機殼上的兩片小 PCB 是一定要拆下來的，本來我是想說能不能就留著不要破壞，不過 PCB 的接頭會卡住主機板，最後要把主機板拿出來的時候發現還是得拆，不過因為只是用膠帶黏的所以很好拆。

- [Newton A50] 沒有附 Wifi 天線，需要 Wifi 的話需要自己買， Wifi 天線的安裝說明要另外在上官網找，機殼附的說明書並沒有這一部份。（在 [這裡](https://akasa.co.uk/download/manual/FittingHoles_installation.pdf) ）

- [Newton A50] 後面的一些開孔總覺得有點誤差，不過實際用起來是不影響插線。

- 裝完之後待機 CPU 溫度有降低了，負載時的溫度上升也有變好。

- 待機 M.2 SSD 溫度比原本高了 5 度左右，我猜可能因為原廠的散熱片比較大，而且有鎖在原本機殼的金屬框上的關係。

---

拆裝機過程中間手忙腳亂，以下是僅存的一些照片：

PN51 原本的樣子（已拆掉 M.2 SSD 散熱片）
![PN51 原本的樣子](/assets/images/2023-10-22/pn51-original.jpg)

拆開來的 PN51
![拆開來的 PN51](/assets/images/2023-10-22/pn51-teardown.jpg)

裝到一半的 Newton A50
![裝到一半的 Newton A50](/assets/images/2023-10-22/newton-assemble.jpg)

有公差的 Newton A50
![有公差的 Newton A50](/assets/images/2023-10-22/newton-error.jpg)

把零件塞回 PN51 空殼
![把零件塞回 PN51 空殼](/assets/images/2023-10-22/pn51-done.jpg)

[ASUS PN51]: https://www.asus.com/displays-desktops/mini-pcs/pn-series/mini-pc-pn51/
[Newton A50]: https://akasa.co.uk/update.php?tpl=product/product.detail.tpl&type=CHASSIS%20POWER&type_sub=FANLESS%20CASES&model=A-NUC78-M1B
[Turing A50 MKII]: https://akasa.co.uk/update.php?tpl=product/product.detail.tpl&type=FANLESS%20CASES&type_sub=AMD%20Mini%20PC&model=A-NUC62-M1BV2
