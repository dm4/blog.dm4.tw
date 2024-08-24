---
layout: post
title: "升級 GitLab"
---

本來想說只有自己在用的 GitLab server 沒對外開放，實在是懶的升級，拖了好幾個月之後總算有力氣研究，沒想到意外的簡單（也可能只是我單純沒遇到問題……）。

因為實在是拖太久所以跨了一個大版本和幾個小版本，我是從 GitLab CE 16.11 升到 17.3 ，本來就是用 docker compose 跑起來的，所以最重要的其實就是把 compose file 裡的 image 換掉，重啟服務就好了，下面是寫給之後的自己看的：

#### 流程

雖然說換個 image 重啟就好，不過保險一點的話可以：檢查目前運作狀態 → 備份 → 升級 → 確認升級後的運作狀態

#### Upgrade Path

官方提供了一個工具 [Upgrade Path](https://gitlab-com.gitlab.io/support/toolbox/upgrade-path/) ，可以依照你的需求決定升級的順序，不過自己用的東西也沒有什麼 zero downtime 的需求，他還是推薦我 16.11.1 → 16.11.8 → 17.3.1 ，我猜大概是有東西先升到最新的 patch 比較保險？

#### 檢查

確保各種服務運作正常，並且目前的 `gitlab-secrets.json` 裡的 key 是可以正常解開資料庫裡的加密資訊：

```sh
docker compose exec gitlab gitlab-rake gitlab:check
docker compose exec gitlab gitlab-rake gitlab:doctor:secrets
```

#### 備份

可以參考官方的 [Back up GitLab](https://docs.gitlab.com/ee/administration/backup_restore/backup_gitlab.html)

```sh
docker compose exec gitlab gitlab-backup create
```

備份會放在 `/var/opt/gitlab`:

```console
> docker compose exec gitlab ls /var/opt/gitlab/backups
1724417483_2024_08_23_16.11.1_gitlab_backup.tar
```

不過目前因為把一堆 CD 轉出來的檔案都丟上 LFS ，在備份的時候跑好久……沒有用 [git-annex](https://git-annex.branchable.com/) 是不是個錯誤？之後再研究好了 QQ

#### 升級

升級大概是最無腦的一步，打開 `docker-compose.yml` 把 image 換成最新的版本，再 `docker compose up -d` 就好了，如果想要查目前最新的版本是什麼可以看官方的 [Releases](https://about.gitlab.com/releases/categories/releases/) ，或是直接去 [GitLab CE 的 Docker Hub](https://hub.docker.com/r/gitlab/gitlab-ce/tags) 找。

如果有一些特定版本升級的時候會遇到一些已知問題，官方會寫在像是 [GitLab 17 Changes](https://docs.gitlab.com/ee/update/versions/gitlab_17_changes.html) 這種頁面，升級前看一下還是比較保險。

#### Background Migrations

有一些會跑比較久 migrations 可以在你的 `https://<gitlab_server>/admin/background_migrations` 看到，如果要 upgrade path 是要升級好幾次的話，要確保每次升級完所有 migrations 都顯示 **已完成** 。

{% include image.md name="background-migrations.png" %}

---

大概就是這樣，總覺得很多像這種拖著沒做的小事，花一點時間研究之後就會覺得「啊，原來沒有我想的那麼難嘛？」，最難的地方好像是「決定要開始做」的這個行為。

上次看到一本講拖延症的書，印象中對於這種問題的解法是：「告訴自己我就只做 10 分鐘試試看，不行就算了」，我也還沒真的試過，不知道有沒有用？

不過如果就升級 GitLab 這件事來說，過了 10 分鐘我可能也還沒看完文件，這樣我時間到真的會放棄嗎？沒有放棄確實是可以讓當下的事情有比較大的機率被做完，但會不會真的要嚴格的制限自己時間到就放棄，才可以讓下一次啟動時的壓力不要那麼大？

草稿夾裡的東西好像是上次寫到一半就停了，還是之後就給它 10 分鐘試試看？

```console
> ll _drafts
Permissions Size User Date Modified Git Name
.rw-r--r--@ 1.4k dm4  11:36 07-15    -N how-i-organize-my-music-3-beets.md
.rw-r--r--@ 4.1k dm4  01:52 08-25    -N upgrade-gitlab.md
```

不過我這篇沒什麼資訊量也是花了超過 40 分鐘，算起來 10 分鐘可能產出 600 多 bytes …… 下次來驗算看看好了。
