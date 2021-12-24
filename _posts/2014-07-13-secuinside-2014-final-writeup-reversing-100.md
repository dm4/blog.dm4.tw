---
layout: post
title: SECUINSIDE CTF Finals 2014 Writeup - Reversing 100
tags: [ctf, writeup]
---

這題在現場沒解完...因為覺得應該只差一點點，回來之後果然弄出來了啊！

``` text wooyaggo's field https://ctftime.org/task/1174
decrypt it and submit 4th line(without last character 0x0a)

platin text example
1. AAA-111-ASRT-3.1-CC[n]
2. BBB-CCC-ASRT-9.5-DD[n]

http://dl.ctftime.org/144/1174/59a67b2df52c3e0a1330a5952a619462584bba28
http://dl.ctftime.org/144/1174/enc.txt
```

第一個連結載下來之後會發現是一個 zip 檔
``` text
$ file 59a67b2df52c3e0a1330a5952a619462584bba28
59a67b2df52c3e0a1330a5952a619462584bba28: Zip archive data, at least v1.0 to extract
```

看了一下內容，會發現是 iOS App 的結構

``` text
$ unzip -l 59a67b2df52c3e0a1330a5952a619462584bba28
Archive:  59a67b2df52c3e0a1330a5952a619462584bba28
  Length     Date   Time    Name
 --------    ----   ----    ----
        0  07-03-14 13:55   Payload/
        0  07-03-14 13:55   Payload/Inverse.app/
        0  07-03-14 13:55   Payload/Inverse.app/_CodeSignature/
     3074  07-03-14 13:55   Payload/Inverse.app/_CodeSignature/CodeResources
        0  07-03-14 13:55   Payload/Inverse.app/Base.lproj/
        0  07-03-14 13:55   Payload/Inverse.app/Base.lproj/Main.storyboardc/
      258  07-03-14 13:55   Payload/Inverse.app/Base.lproj/Main.storyboardc/Info.plist
        0  07-03-14 13:55   Payload/Inverse.app/Base.lproj/Main.storyboardc/UIViewController-vXZ-lx-hvc.nib/
      953  07-03-14 13:55   Payload/Inverse.app/Base.lproj/Main.storyboardc/UIViewController-vXZ-lx-hvc.nib/objects.nib
      953  07-03-14 13:55   Payload/Inverse.app/Base.lproj/Main.storyboardc/UIViewController-vXZ-lx-hvc.nib/runtime.nib
        0  07-03-14 13:55   Payload/Inverse.app/Base.lproj/Main.storyboardc/vXZ-lx-hvc-view-kh9-bI-dsS.nib/
     4276  07-03-14 13:55   Payload/Inverse.app/Base.lproj/Main.storyboardc/vXZ-lx-hvc-view-kh9-bI-dsS.nib/objects.nib
     4454  07-03-14 13:55   Payload/Inverse.app/Base.lproj/Main.storyboardc/vXZ-lx-hvc-view-kh9-bI-dsS.nib/runtime.nib
    13944  07-03-14 13:55   Payload/Inverse.app/embedded.mobileprovision
        0  07-03-14 13:55   Payload/Inverse.app/en.lproj/
       42  07-03-14 13:55   Payload/Inverse.app/en.lproj/InfoPlist.strings
     1332  07-03-14 13:55   Payload/Inverse.app/Info.plist
   210368  07-03-14 13:55   Payload/Inverse.app/Inverse
    14929  07-03-14 13:55   Payload/Inverse.app/LaunchImage-568h@2x.png
    14929  07-03-14 13:55   Payload/Inverse.app/LaunchImage-700-568h@2x.png
        8  07-03-14 13:55   Payload/Inverse.app/PkgInfo
      150  07-03-14 13:55   Payload/Inverse.app/ResourceRules.plist
 --------                   -------
   269670                   22 files
```

用 `otool` 看一下執行檔有沒有被加密

``` text
$ otool -l Payload/Inverse.app/Inverse | grep -A5 LC_ENCRYPTION_INFO
          cmd LC_ENCRYPTION_INFO
      cmdsize 20
    cryptoff  16384
    cryptsize 16384
    cryptid   0
Load command 13
--
          cmd LC_ENCRYPTION_INFO
      cmdsize 20
    cryptoff  16384
    cryptsize 16384
    cryptid   0
Load command 13
--
          cmd LC_ENCRYPTION_INFO_64
      cmdsize 24
    cryptoff  16384
    cryptsize 16384
    cryptid   0
        pad   0
```

`cryptid` 是 0 表示沒有加密，所以我們可以直接來看 `Payload/Inverse.app/Inverse` ！

看了 code 之後會發現在 `[ViewController viewDidLoad]` 裡會去讀 `/Users/hkpco/tmp/plain.txt` ，把檔案的內容當作參數傳給 `[ViewController es:]` ，在 `[ViewController es:]` 裡會把 `/Users/hkpco/tmp/plain.txt` 和 `/Users/hkpco/tmp/pass.txt` 做一些處理之後，把結果寫到 `/Users/hkpco/tmp/enc.txt`

把整段用 `ruby` 重寫會長得像

``` ruby
require 'digest/sha1'

def s(i)
  Digest::SHA1.hexdigest i.to_s
end

def h2i(h)
  h.to_i(16)
end

def all(h)
  h.split("").map{ |x| h2i(x) }.reduce(:+)
end

def es(plain, pass)
  enc = ""
  hash = s(pass)
  sum  = all(hash)
  (0..plain.length - 1).each do |i|
    result = plain[i].ord + h2i(hash[i % 40]) - sum
    sum    = all(s(plain[0..i])[0..19]) + all(s(sum)[0..19])
    enc   += result.to_s
  end
  enc
end
```

所以看到目前為止，我們要做的應該是「只知道 `enc` ，想辦法推回原本的 `plain` 」，原本的 `pass` 是什麼對我們來說不重要，我們只要知道 `hash = s(pass)` 是什麼就好了！

研究了一下會發現，如果要從 `enc` 逆推 `plain` 的話，只要暴搜每一輪的 `plain[i]` 和 `hash[i % 40]` ， `hash[i % 40]` 因為是 hex ，只會有 16 種可能，暴完 40 位的 `hash` 之後，再看 `sum` 的起始值有沒有滿足 `sum == all(hash)` 就可以知道結果了。

比較討厭的是題目給的 `1. AAA-111-ASRT-3.1-CC[n]` ，真的去試的話會發現用 `1. ` 開頭的 plain 是沒有解的，後面接 `[n]` 也是一樣，所以最後試出來每行的格式應該是像 `[0-9A-Z]{3}-[0-9A-Z]{3}-ASRT-\d\.\d-[0-9A-Z]{2}\n`...解這題的時候因為格式也卡了好久...

``` ruby
enc = [
  -178, -189, -255, -256, -193, -257, -235, -159, -198, -227,
  -187, -187, -267, -221, -225, -228, -235, -249, -259, -287,
  -230, -190, -196, -218, -269, -230, -259, -239, -221, -205,
  -233, -253, -221, -228, -194, -312, -258, -120, -214, -269,
  -255, -186, -215, -258, -252, -229, -234, -289, -215, -229,
  -144, -199, -247, -174, -291, -202, -307, -195, -197, -237,
  -264, -173, -197, -249, -290, -226, -225, -204, -238, -244,
  -201, -152, -270, -244, -263, -155, -253, -229, -247, -319,
  -229, -272, -264, -280, -188, -203, -259, -258, -261, -195,
  -257, -166, -231, -207, -281, -191, -234, -210, -187, -309,
  -170, -172, -239, -225, -195, -238, -265, -260, -291, -185,
  -196, -191, -216, -225, -237, -248, -222, -167, -240, -269,
  -205, -172, -208, -235, -235, -223, -201, -254, -229, -230,
  -209, -265, -312, -240, -201, -274, -258, -180, -223, -250,
]

def dfs(plain, enc)
  round = plain.length
  if round > 39
    p plain
    return
  end

  round %= 20
  if round == 3 || round == 7 || round == 12 || round == 16
    chars = [ "-" ]
  elsif round >= 8 && round <= 11
    s = "ASRT"
    chars = [ s[round - 8] ]
  elsif round == 14
    chars = [ "." ]
  elsif round == 13 || round == 15
    chars = []
    ('0'..'9').each { |c| chars.push(c) }
  elsif round == 19
    chars = [ "\n" ]
  else
    chars = []
    ('0'..'9').each { |c| chars.push(c) }
    ('A'..'Z').each { |c| chars.push(c) }
  end
  chars.each do |c|
    plain = plain + c
    (0..15).each do |i|
      if brute_es(plain, enc, i)
        dfs(plain, enc)
        break
      end
    end
    plain = plain[0..-2]
  end
end

dfs("", enc)
```

一共會跑出 34 組，看到 `SEC-` 和 `KOR-` 就覺得應該沒解錯，不過直接用 ruby 跑其實有點久，有時間再來看一下怎麼加速 XD

``` text
$ time ruby solve.rb
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-J1\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-J2\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-KD\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-KE\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-KJ\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-KM\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-KN\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-KO\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-L0\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-L2\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-MR\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-MX\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-N5\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-NB\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-NC\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-ND\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-PT\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-PY\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-Q0\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-Q1\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-Q2\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-Q3\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-RX\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-SN\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-SQ\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-SR\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-SU\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-T7\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-TE\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-UP\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-UU\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-W0\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-W6\n"
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-W8\n"

real    92m16.033s
user    92m12.464s
sys     0m3.675s
```

接下來就對這幾組，看他們的 `hash` 和 `sum == all(hash)` 了

``` ruby
plains = [
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-J1\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-J2\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-KD\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-KE\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-KJ\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-KM\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-KN\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-KO\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-L0\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-L2\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-MR\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-MX\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-N5\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-NB\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-NC\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-ND\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-PT\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-PY\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-Q0\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-Q1\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-Q2\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-Q3\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-RX\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-SN\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-SQ\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-SR\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-SU\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-T7\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-TE\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-UP\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-UU\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-W0\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-W6\n",
  "SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-W8\n",
]

def rev_es(plain, enc, start)
  hash = ""
  hash += start.to_s(16)

  first_sum = plain[0].ord + start - enc[0]
  sum = first_sum;
  sum = all(s(plain[0])[0..19]) + all(s(sum)[0..19])

  (1..plain.length - 1).each do |i|
    r   = enc[i] - plain[i].ord + sum
    sum = all(s(plain[0..i])[0..19]) + all(s(sum)[0..19])
    return 0 if r < 0 || r > 15
    hash += r.to_s(16)
  end

  p hash if all(hash) == first_sum
end

decode_hash(plains, enc)
```

只有兩組符合條件

``` text
$ ruby solve.rb
"011c945f30ce2cbafc452f39840f025693339c42"
"011c945f30ce2cbafc452f39840f025693339909"
```

接著就直接用這兩組來解 enc ，看看哪個才是正解！

``` ruby
def es_hash(enc, hash)
  output = ""
  sum  = all(hash)
  (0..enc.length - 1).each do |i|
    c = enc[i] - h2i(hash[i % 40]) + sum
    output += c.chr
    sum    = all(s(output)[0..19]) + all(s(sum)[0..19])
  end
  output
end

p es_hash(enc, "011c945f30ce2cbafc452f39840f025693339c42")
p es_hash(enc, "011c945f30ce2cbafc452f39840f025693339909") # error
```

用第一個 `hash` 可以成功還原 `plain`

``` text
$ ruby solve.rb
"SEC-41A-ASRT-3.1-OK\nKOR-A43-ASRT-9.3-KO\nCOV-718-ASRT-7.9-QE\nCON-C1D-ASRT-1.1-CO\nCOM-ICQ-ASRT-1.9-KR\nLSA-IQ1-ASRT-3.0-KR\nAES-JOO-ASRT-9.9-KR\n"
```

所以 flag 應該就是第四行的 `CON-C1D-ASRT-1.1-CO` ！

不過最後正確的 `hash` 是 `011c945f30ce2cbafc452f39840f025693339c42` ，其實 `pass` 就只是 `1111` 而已...
