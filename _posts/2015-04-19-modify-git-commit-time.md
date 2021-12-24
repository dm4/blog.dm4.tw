---
layout: post
title: "更改 git commit 時間"
tags: [git]
---

最近剛好需要更改 git repo 的 commit time ，研究了一下順便把它記錄下來。

首先需要知道的是，在 git 裡，因為 commit 是可以被修改的，在一個 commit 裡其實會記錄 **author** 和 **committer** ， **author** 就是寫 code 的人， **committer** 則是真正提交 code 的人，

用法大概是：對某個你不是 maintainer 的 project ，你寫了一個新功能，然後用 `git format-patch` 生出了 patch ，接著你把這個 patch 送給 maintainer ，在 maintainer 決定採用你的 patch 之後，用 `git apply` 來採用你的 commit ，接著他再 `git commit` ， code 才進到 repo 裡，這個新 commit 的 **author** 就是你， **committer** 就是真的做 `git commit` 的那個 maintainer 。

為什麼改 commit 時間，會要知道 git 裡的 author 和 committer 呢？因為在更改 commit 時間時，如果沒有特別注意的話，會只改到 commit time ，並且該 commit 的 committer 會變成自己，原本的 author 資訊和 author time 會保留下來，所以如果不想讓人發現曾經改過 commit 的話，必須把 committer 設成原本的 author ，也要把 author time 改成想要的 commit time 。

知道這些之後大概就沒問題了，可以用 `git filter-branch` (http://git-scm.com/docs/git-filter-branch) 來做：

## Script & Setup

```
[~/modify-commit-time] ➟  ls
modify_commit_time.sh*
[~/modify-commit-time] ➟  cat modify_commit_time.sh
# clear previous backup
rm -rf "$(git rev-parse --git-dir)/refs/original/"

commit="$1"
date=$(date -d "$2" +%s)
git filter-branch --env-filter \
    "if [ \$GIT_COMMIT = '$commit' ]; then
         export GIT_AUTHOR_DATE='$date'
         export GIT_COMMITTER_DATE='$date'
         export GIT_COMMITTER_NAME="\$GIT_AUTHOR_NAME"
         export GIT_COMMITTER_EMAIL="\$GIT_AUTHOR_EMAIL"
     fi"
[~/modify-commit-time] ➟  git init
Initialized empty Git repository in /home/dm4/modify-commit-time/.git/
(master) [~/modify-commit-time] ➟  git add modify_commit_time.sh
(master) [~/modify-commit-time] ➟  git commit -m 'add script'
[master (root-commit) 9c4c1d0] add script
 1 file changed, 12 insertions(+)
 create mode 100755 modify_commit_time.sh
(master) [~/modify-commit-time] ➟  echo '# Hello' > README.md
(master) [~/modify-commit-time] ➟  git add README.md
(master) [~/modify-commit-time] ➟  git commit -m 'add README'
[master a2913a7] add README
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
```

可以看到現在有兩個 commit

```
(master) [~/modify-commit-time] ➟  git log --pretty=fuller
commit a2913a7c39977dd3c4f99f8cabc6fae5b3694cb2
Author:     dm4 <test@example.com>
AuthorDate: Sun Apr 19 16:48:33 2015 +0000
Commit:     dm4 <test@example.com>
CommitDate: Sun Apr 19 16:48:33 2015 +0000

    add README

commit 9c4c1d0b4499683ab0ba80dd7041b477c0adb2f0
Author:     dm4 <test@example.com>
AuthorDate: Sun Apr 19 16:48:33 2015 +0000
Commit:     dm4 <test@example.com>
CommitDate: Sun Apr 19 16:48:33 2015 +0000

    add script
```

## Modify Commit Time

```
(master) [~/modify-commit-time] ➟  ./modify_commit_time.sh 9c4c1d0b4499683ab0ba80dd7041b477c0adb2f0 '3 month ago'
Rewrite a2913a7c39977dd3c4f99f8cabc6fae5b3694cb2 (2/2)
Ref 'refs/heads/master' was rewritten
```

## Result

```
(master) [~/modify-commit-time] ➟  git log --pretty=fuller
commit 9552b631000134921a75f173fa7313c1a90cb96e
Author:     dm4 <test@example.com>
AuthorDate: Sun Apr 19 16:48:33 2015 +0000
Commit:     dm4 <test@example.com>
CommitDate: Sun Apr 19 16:48:33 2015 +0000

    add README

commit 67e6f5a501703e98f8d9711ade8ab3a51775c265
Author:     dm4 <test@example.com>
AuthorDate: Mon Jan 19 16:49:02 2015 +0000
Commit:     dm4 <test@example.com>
CommitDate: Mon Jan 19 16:49:02 2015 +0000

    add script
```

這裡可以看到雖然只改了 `add script` 的 commit ，但是連 `add README` 的 commit id 也變了，這是因為 git 在算 hash 的時候，也會包含 parent 的 commit id ，所以 parent 的 commit id 變了，所有的 child commit id 都會跟著變，這個在寫 script 批次處理的時候需要注意一下。

