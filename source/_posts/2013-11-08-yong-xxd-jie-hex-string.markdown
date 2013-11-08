---
layout: post
title: "用 xxd 解 Hex String"
date: 2013-11-08 13:57
comments: true
categories: [linux, xxd]
---

本來解 hex string 都是用有點醜的 perl one-liner

```
$ perl -ne 'print map chr hex, m/../g' <<< 646d340a
dm4
```

不過昨天又研究了一下 `xxd` 發現一些神秘的功能，像是發現有 C style 的 output ！

```
$ xxd -i data
unsigned char data[] = {
  0x74, 0x68, 0x69, 0x73, 0x20, 0x69, 0x73, 0x20, 0x62, 0x69, 0x61, 0x6e,
  0x72, 0x79, 0x3f, 0x0a
};
unsigned int data_len = 16;
```

還可以調 column 數

```
$ xxd -i -c 8 data
unsigned char data[] = {
  0x74, 0x68, 0x69, 0x73, 0x20, 0x69, 0x73, 0x20,
  0x62, 0x69, 0x61, 0x6e, 0x72, 0x79, 0x3f, 0x0a
};
unsigned int data_len = 16;
```

然後 [atdog](http://blog.atdog.org) 大大看到之後，又教了我怎麼用 `xxd` 解 hex string

- 生 hex string

```
$ xxd -p <<< dm4
646d340a
```

- 解 hex string

```
$ xxd -p -r <<< 646d340a
dm4
```

太帥了！而且比原本 perl one-liner 短好多啊啊啊
