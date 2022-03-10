---
title: "升级 Debian11 的注意事项"
subtitle: "从 Debian10 升级到 Debian11"
date: 2021-08-22T10:39:23+08:00
lastmod: 2021-08-22T10:39:23+08:00

author: "xavier"
authorLink: ""
description: ""

tags: [linux,debian,buster,bullseye]
categories: []

featuredImage: "bullseye.png"

draft: false
---

Debian 11 “bullseye” 在 2 年 1 个月零 9 天的开发后，在 8 月 14 日如期发布了。在本地稍加测试之后，本着对 Debian 稳定性的信任，服务器也第一时间升级到了该版本。

<!--more-->

## 升级

根据 [phoronix 的测试](https://www.phoronix.com/scan.php?page=article&item=debian11-xeon-epyc&num=1)，Debian11 相比于 Debian10.10，提升的幅度巨大。不过这么大的提升幅度，更多是来自于 Linux 内核升级所带来的。我的服务器一直使用的是 Debian Backup 仓库的 cloud 内核，估计性能的提升就不会有这么大了。

对于升级，首先必须要做的就是阅读对应版本的发行说明，我的服务器是 amd64 架构，故需要阅读 [Debian 11 (bullseye), 64-bit PC 的发行说明](https://www.debian.org/releases/bullseye/amd64/release-notes/index.zh-cn.html)。其中第四部分，Debian 团队还是一如既往的贴心的给出了从上一版本的升级说明（[从 Debian 10（buster）升级](https://www.debian.org/releases/bullseye/amd64/release-notes/ch-upgrading.zh-cn.html)）。

升级的命令和注意事项在该升级说明中已经很详细了。这里就不对这部分做过多说明了，只是给出一些简单的命令和一些需要注意的问题。升级有风险，一定要阅读发行说明，一定要知道你的每一步是在干什么。

### 更新 `apt-sources` 文件

1. 将 `/etc/apt/sources.list` 以及 `/etc/apt/sources.list.d/` 下文件中所有的  `buster` 的更改为 `bullseye`。

2. Debian 的 APT 源使用了 CDN 服务，可以直接使用 `deb http://deb.debian.org/debian bullseye main contrib` 来获取更快的下载。不过如果已经用了确定更快的镜像站点，那就不要改啦。

3. 对于安全源，debian11 相比 debian10，有了一定的改动。在发行版后面需要加一个 `-security` 的后缀。即**安全源**需要将 `buster` 改为 `bullseye-security`，需要注意。
前：

``` bash
deb https://security.debian.org/debian-security buster/updates main contrib
deb-src https://security.debian.org/debian-security buster/updates main contrib
```

后：

``` bash
deb https://security.debian.org/debian-security bullseye-security/updates main contrib
deb-src https://security.debian.org/debian-security bullseye-security/updates main contrib
```

### 一步一步来

1. 尽可能的记录升级的过程，使用script或者其他更熟悉的工具。

2. 备份。

   1. 无需关心那些文件可能会被改动和丢失。假设备份的目的是迁移服务器，从而备份尽可能多的内容。

   2. 备份不是单纯的复制一份文件，这只完成了备份工作的一半。用备份的文件在不同地方恢复一遍，并检查恢复的结果，才是一次完整的备份。

3. 看一下是不是还有足够的磁盘空间来存储升级过程中下载或解压产生的临时文件。

4. 装一下 linux-image-* 元包，以便让 debian 管理升级内核

5. 先升级一部分 `apt upgrade --without-new-pkgs`

6. 再升级全部 `apt full-upgrade`

7. 重启

8. 再一次升级 `apt update && apt upgrade`，因为部分软件包，如依赖 `libgc1c2` 的软件包，在 full-upgrade 中可能会升级失败，重启后最好再来一次。

### 特别说明

对上面的步骤，终结一下需要强调的部分

1. 安全源

2. 备份和查看磁盘剩余空间

3. 三次 upgrade，重启前两次，重启后一次。

## 升级之后

### 合并 usr

我相信刚开始了解 Unix 和 Linux 的人，一定会对 `/usr/bin` 和 `/bin`、`/usr/sbin` 和 `/sbin`、`/usr/lib` 和`/lib` 迷惑不解。其实这是由于历史上的原因照成的，主要就当时的磁盘太小了。详情见这篇在 busybox 邮件目录文章：[理解 bin, sbin, usr/bin , usr/sbin 拆分](http://lists.busybox.net/pipermail/busybox/2010-December/074114.html)。

好消息是，随着时代的发展现在我们已经不需要将这些目录一分为二了。近几年来许许多多的发行版都致力于将其合二为一，以及许许多多相关的文章。 [Freedesktop.org 的相应总结](https://www.freedesktop.org/wiki/Software/systemd/TheCaseForTheUsrMerge)、[openSUSE：采取 UsrMerge 措施的理由](https://suse.org.cn/%E6%8A%80%E6%9C%AF%E6%96%87%E7%AB%A0/2021/04/28/%E9%87%87%E5%8F%96-UsrMerge-%E6%8E%AA%E6%96%BD%E7%9A%84%E7%90%86%E7%94%B1.html)、[usrmerge in Debian](https://szlin.me/2017/12/28/usrmerge-in-debian/)等等。

Debian 也在进行这项工作，而 Debian 11 bullseye 将会是最后一个支持 non-merged-usr 布局的 Debian 发布版本。Debian现在提供了 [UsrMerge](https://wiki.debian.org/UsrMerge) 软件包以方便用户在需要的时候进行布局转换。

如果想合并的话，请遵循以下工作：

1. 先用 `ls -l /` 看看现在是不是已经合并了。如下图，即还没合并

{{< image src="merge.png" caption="merge" alt="merge" title="merge" >}}

2. 如果没有的话，使用 `apt update && apt install usrmerge` 安装 `usrmerge`。安装后，会出现如下提示，选择 `YES` 即会开始自动合并。

{{< image src="usrmerge.png" caption="usrmerge" alt="usrmerge" title="usrmerge" >}}

3. 自动合并后，再使用 `ls -l /` 查看，会发现已经合并好了。（注意安装后 usrmerge 并不是一个命令，安装后是 `/usr/lib/convert-etc-shells` 和 `/usr/lib/convert-usrmerge` 两个脚本文件）

{{< image src="merged.png" caption="merged" alt="merged" title="merged" >}}

## 其他需要说明的

接下来将会有一些关于 cloud 内核，bbr2 和 kernel 的关系的内容，待续。。。
