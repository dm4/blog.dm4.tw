---
layout: post
title: "Maestral (Dropbox Client)"
tags: [software, git]
redirect_from: /2023/12/22/maestral-dropbox-client.html
---

[Maestral][maestral] 是一個 Dropbox 的第三方跨平台 client ，用了一陣子覺得還不錯，最讚的是還可以擺脫 Dropbox 免費帳戶的三個裝置限制。
在 macOS 上用起來大概就像是……最一開始官方的 Dropbox client 一樣，功能很簡單但是很好用。

不過不知道是 Maestral 還是 Dropbox 的問題，有時候在同步整個 git repository 的時候會有些 objects 沒同步到，會出現像是：

```text
fatal: unable to read 243d851ad30c4313e4ce2fc9f936834be3e25fc0
```

不過去 touch 一下對應的 objects 檔案就可以了：

```console
$ maestral filestatus .git/objects/24/3d851ad30c4313e4ce2fc9f936834be3e25fc0
unwatched
$ touch .git/objects/24/3d851ad30c4313e4ce2fc9f936834be3e25fc0
$ maestral filestatus .git/objects/24/3d851ad30c4313e4ce2fc9f936834be3e25fc0
up to date
```

[maestral]: https://maestral.app/
