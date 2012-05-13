---
layout: post
title: ".bashrc sourced twice"
date: 2012-05-11 21:08
comments: true
categories:
-   bash
-   shellscript
-   linux
---

昨天解決了困擾了一陣子的問題，在 `.bashrc` 裡，如果有改到 `$PATH` ：

    export PATH=~/bin:$PATH

又因為我會用到 [tmux][tmux] ，所以在 `tmux` 裡開新的 shell 時，會再 source 一次 `.bashrc` ，本來我的解法是這樣：

    [ -z "$TMUX" ] && export PATH=~/bin:$PATH

也就是在 `tmux` 裡新開啟的 shell 並不會再改一次 `$PATH` ，可是這樣又遇到了另一個問題：當新的 shell 打開時，本來應該被加在前面的 `~/bin` 會被接到後面去，我在想應該是開新 shell 時 bash 自己做的處理，我在 `/etc/bashrc` 裡也沒有看到相關的 script 。

對於這個問題我最後的解法是參考 [perlbrew][perlbrew] 裡的這段 script ：

    export PATH_WITHOUT_PERLBREW="$(perl -e 'print join ":", grep { index($_, $ENV{PERLBREW_ROOT}) } split/:/,$ENV{PATH};')"

    if [[ -z "$PERLBREW_PATH" ]]; then
        export PERLBREW_PATH="$PERLBREW_ROOT/bin"
    fi

    export PATH="$PERLBREW_PATH:$PATH_WITHOUT_PERLBREW"

最後的結果：

    export PATH="~/bin:$(perl -e 'print join ":", grep { $_ ne "~/bin" } split/:/, $ENV{PATH};')"

[tmux]: http://tmux.sourceforge.net/
[perlbrew]: http://perlbrew.pl/

**Update** 2012-05-13

為了避免 `~/bin` 和 `$HOME/bin` 混用，所以改了一下 script

    export PATH=$HOME/bin:"$(perl -e 'print join ":", grep { $_ ne "$ENV{HOME}/bin" && $_ ne "~/bin" } split/:/, $ENV{PATH};')"
