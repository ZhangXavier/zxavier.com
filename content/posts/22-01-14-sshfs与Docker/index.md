---
title: "sshfs 与 Docker"
subtitle: "使用 sshfs，解决 docker 空间"
date: 2022-01-14T15:43:18+08:00
lastmod: 2022-03-10T15:43:18+08:00

author: ""
authorLink: ""
description: ""

tags: []
categories: []

hiddenFromHomePage: false
hiddenFromSearch: false

resources:
- name: "featured-image"
  src: ""
featuredImage: ""
featuredImagePreview: ""

draft: true
---

## 前言

我目前使用的是gitea-rootless docker，目前需要转移，

<!--more-->

[https://docs.gitea.io/en-us/backup-and-restore/](https://docs.gitea.io/en-us/backup-and-restore/) 可以参考这里，不过因为我的是rootless，故还需要一些改进

{{< image src="sshfs.webp" caption="sshfs" alt="sshfs" title="sshfs" >}}

还有8.7G的地方

{{< image src="sshfs_1.webp" caption="sshfs_1" alt="sshfs_1" title="sshfs_1" >}}

gitea 占用了1.4G的地方

开始备份

{{< image src="sshfs_2.webp" caption="sshfs_2" alt="sshfs_2" title="sshfs_2" >}}

```Bash
 docker exec -u git -it $(docker ps -qf "name=gitea") bash -c '/usr/local/bin/gitea dump -c /etc/gitea/app.ini'
```

{{< image src="sshfs_3.webp" caption="sshfs_3" alt="sshfs_3" title="sshfs_3" >}}

开始打包备份内容

{{< image src="sshfs_4.webp" caption="sshfs_4" alt="sshfs_4" title="sshfs_4" >}}

很快啊，很快空间就不足了

{{< image src="sshfs_5.webp" caption="sshfs_5" alt="sshfs_5" title="sshfs_5" >}}

{{< image src="sshfs_6.webp" caption="sshfs_6" alt="sshfs_6" title="sshfs_6" >}}

{{< image src="sshfs_7.webp" caption="sshfs_7" alt="sshfs_7" title="sshfs_7" >}}

{{< image src="sshfs_8.webp" caption="sshfs_8" alt="sshfs_8" title="sshfs_8" >}}

安装 sshfs

两边创建目录

``` shell
sshfs awsl:/home/admin/dcvm ~/awsl
```

添加映射

{{< image src="sshfs_9.webp" caption="sshfs_9" alt="sshfs_9" title="sshfs_9" >}}

{{< image src="sshfs_10.webp" caption="sshfs_10" alt="sshfs_10" title="sshfs_10" >}}

[https://serverfault.com/questions/947182/mount-a-sshfs-volume-into-a-docker-instance](https://serverfault.com/questions/947182/mount-a-sshfs-volume-into-a-docker-instance)

{{< image src="sshfs_11.webp" caption="sshfs_11" alt="sshfs_11" title="sshfs_11" >}}

```Bash
sudo umount ~/awsl

sshfs -o allow_other awsl:/home/admin/dcvm ~/awsl

```

{{< image src="sshfs_12.webp" caption="sshfs_12" alt="sshfs_12" title="sshfs_12" >}}

这次 docker-compose up -d 就成功啦

```Bash
 docker exec -u git -it -w /backup $(docker ps -qf "name=gitea") bash -c '/usr/local/bin/gitea dump -c /etc/gitea/app.ini'
```

{{< image src="sshfs_13.webp" caption="sshfs_13" alt="sshfs_13" title="sshfs_13" >}}

这次终于完成啦
