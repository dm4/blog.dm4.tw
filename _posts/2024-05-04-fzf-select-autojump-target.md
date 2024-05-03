---
layout: post
title: "用 fzf 選擇 autojump 的目標"
---

再弄完前一篇 [用 fzf 搜尋 PChome 的商品並顯示預覽圖](/fzf-search-pchome-products-display-preview-images) 之後，又想到可以和 [autojump] 一起用。

[autojump] 是一個可以方便切換目錄的小工具，能記錄常去的目錄，然後用 `j <目錄縮寫>` 來快速切換，可以省去一點 `cd` 要打入完整路徑的麻煩。

[autojump] 的做法其實是記錄了目錄的路徑和使用次數，並依照使用次數來給予目錄不同的權重，可以用 `j -s` 來看目前的記錄。

但有時候可能在不同的地方有相同的目錄名稱，這時候就會有點小困擾，像是：

```console
$ cd
$ mkdir -p abc/foo/bar
$ mkdir -p def/foo/bar
$ j -a ~/abc/foo/bar
$ j -a ~/def/foo/bar
$ j -s
10.0:   /Users/dm4/def/foo/bar
10.0:   /Users/dm4/abc/foo/bar
________________________________________

20:      total weight
2:       number of entries
0.00:    current directory weight

data:    /Users/dm4/Library/autojump/autojump.txt
```

這時候用 `j bar` 可能會不確定要去哪個目錄，雖然官方提供了 Multiple Arguments 的功能，但我測試起來，在上面的狀況下，用 `j abc bar` 並不能保證一定會去 `abc/foo/bar`（詳細的路徑比對邏輯可以參考 [autojump_match.py](https://github.com/wting/autojump/blob/master/bin/autojump_match.py) ），用 `j abc foo bar` 才能確保一定會進到 `abc/foo/bar` 。

```console
$ j abc bar
/Users/dm4/def/foo/bar
$ j abc f b
/Users/dm4/abc/foo/bar
```

所以更可視化的模糊搜尋工具 [fzf] 這時候就派上用場了！利用 [fzf] 提供的 `_fzf_complete_COMMAND` 功能，可以讓我們自訂不同指令的 completion ，詳細說明可以參考官方的 [Custom fuzzy completion
](https://github.com/junegunn/fzf/?tab=readme-ov-file#custom-fuzzy-completion) 。

所以可以這樣設定：

```bash
export FZF_COMPLETION_TRIGGER=',,'
_fzf_complete_j() {
    _fzf_complete --prompt="autojump> " -- "$@" < <(
    j -s | awk -F $'\t' '/^___/{exit} {print $2}' | tac
  )
}
_fzf_complete_j_post() {
  awk '{printf "\"%s\"", $0}'
}
```

就可以在 CLI 打 `j ,,` 之後按 `Tab` 來搜尋 autojump 的目錄，搜尋完之後按下 `Enter` 就可以切換目錄，像是這樣：

[![][autojump.gif]][autojump.gif]{:target="_blank"}

一樣是一些說明：

- `FZF_COMPLETION_TRIGGER` 預設是 `**` ，也就是在 CLI 打兩個 `*` 之後按 `Tab` 會觸發 fzf 的 completion ，這邊我改成 `,,` 單純是因為覺得比較順手 XD
- 沒用過 `**<Tab>` 功能的推薦可以試試看 `kill **<Tab>` 這樣可以看到所有的 process ，搜完之後按下 enter 就可以選定 PID 當成參數，或在 [fzf] 中按 `Tab` 來選取多個 PID 。

[![][kill]][kill]{:target="_blank"}

- 用 `awk` 來讓 `fzf` 只顯示分隔線以上的目錄清單，用 `tac` 來讓最常用的目錄在最上面（原本的是分數低的在上面）。
- 因為目錄名稱可能有空白，所以在 `_fzf_complete_j_post` 把結果用 `"` 包起來。

[fzf]: https://github.com/junegunn/fzf
[autojump]: https://github.com/wting/autojump
[autojump.gif]: /assets/images/fzf-select-autojump-target/autojump.gif
[kill]: /assets/images/fzf-select-autojump-target/kill.gif
