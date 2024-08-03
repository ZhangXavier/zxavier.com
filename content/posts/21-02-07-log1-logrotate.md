---
title: "消失不见的lastb （Linux日志 1）"
subtitle: "关于logrotate的一点东西"
date: 2021-02-07T11:26:56+08:00
lastmod: 2021-03-08T11:40:23+08:00

authors: [xavier]
description: "某天 lastb 日志神奇的消失了，竟是默认的 logrotate 配置在捣鬼"

tags: [log,linux,logrotate,lastb,cron]
categories: []

draft: false
---

<!--more-->

## 前言

服务器安全是绝对不可忽视的。而使用`lastb`查看登陆失败的用户记录，便是一个与之相关的常用命令，可以得知我们的服务器的是否被人尝试登录。

## 背景

某一天，我像往常一样输入了 `sudo lastb`，想看看有没有被拒之门外的人。令人意外的是，结果竟然一片空白，之前“友好”的访客们的到访记录竟然消失不见了！我第一反应便是有人入侵了，因为一般不速之客入侵后都会清理掉一些系统日志，特别是有关于登陆的信息。在紧急排查过后，发现并没有被入侵的痕迹，这时我又打开了 `lastb` ，才发现当前日期是某月的一号。这时我才想起，这台服务器的`logrotate`还是默认配置，赶紧打开配置查看。果然是虚惊一场。默认配置的它在每月一号，清理了 `/var/log/btmp` ，而 `lastb` 正是通过访问这个日志输出的。

## 修改 logrotate 配置

这“捣鬼”的 `logrotate` 正是 Linux 上的日志文件轮替程序。它基于 `cron` 运行，其脚本位于 `/etc/cron.daily/logrotate` 。而在运行时，`logrotate` 会根据配置文件 `/etc/logrotate.conf` 以及 `/etc/logrotate.d` 里的文件执行规则。

### 修改 btmp 对应配置

为让我们的 `lastb` 内容不再消失不见，我们需要对其配置文件进行修改，而与之相关的便是 `btmp` 日志了：

其对应配置为 `/etc/logrotate.d/btmp` ，在 Debian 下，其文件默认内容如下：

``` conf
# no packages own wtmp -- we'll rotate it here
/var/log/btmp {
    missingok
    monthly
    create 0660 root utmp
    rotate 1
}
```

其中配置参数意义如下：

- `missingok` 如果日志丢失，不报错继续滚动下一个日志。
- `monthly` 每月切割日志并转储
- `create 0660 root utmp` 轮转时指定创建新文件的属性为0660，用户为root，用户组为utmp
- `rotate 1` 删除之前转储的1次，保留1个备份

将其修改为

``` conf
# no packages own wtmp -- we'll rotate it here
/var/log/btmp {
    missingok
    compress
    monthly
    create 0660 root utmp
    minisize 1M
    rotate 5
}
```

加入 `compress` 压缩转储文件；加入 `minisize 1M` ，只有日志大于 1M 时才转储；修改 `rotate 1` 为 `rotate 5` ，备份 5 个文件。

### 修改 wtmp 对应配置

而除了 `btmp` ，`wtmp` 也是用来记录登陆信息的文件，其记录登陆者与系统重新启动时的时间与来源主机及登陆期间的时间。

其对应配置在 `/etc/logrotate.d/wtmp` ，其在 Debian 默认如下：

``` conf
# no packages own wtmp -- we'll rotate it here
/var/log/wtmp {
    missingok
    monthly
    create 0664 root utmp
    minsize 1M
    rotate 1
}
```

我修改为如下了：

``` conf
# no packages own wtmp -- we'll rotate it here
/var/log/wtmp {
    missingok
    compress
    monthly
    create 0664 root utmp
    minsize 1M
    rotate 5
}
```

### 修改 logrotate 默认配置

而对于 `logrotate` 的通用配置 `/etc/logrotate.conf` ，也由

``` conf
# compress 
```

改为了：

``` conf
compress
```

当然更改后重新启动 `logrotate` ：

``` bash
sudo systemctl restart logrotate.service
```

## logrotate 其他可配置参数

`logrotate` 这个“捣乱份子”，其实是“性本善”的。在我们熟悉其天赋秉性后，是可以为我们所用的。以下便是它的详细可调教参数。

| 配置参数 | 意义 |
| :-----:  | :--- |
| compress | 通过 gzip 压缩转储以后的日志 |
| nocompress | 不做 gzip 压缩处理 |
| copytruncate | 用于还在打开中的日志文件，把当前日志备份并截断；是先拷贝再清空的方式，拷贝和清空之间有一个时间差，可能会丢失部分日志数据 |
| nocopytruncate | 备份日志文件不过不截断 |
| create mode owner group | 轮转时指定创建新文件的属性，如 create 0777 nobody nobody |
| nocreate | 不建立新的日志文件 |
| delaycompress | 和 compress 一起使用时，转储的日志文件到下一次转储时才压缩 |
| nodelaycompress | 覆盖 delaycompress 选项，转储同时压缩 |
| missingok | 如果日志丢失，不报错继续滚动下一个日志 |
| errors address | 专储时的错误信息发送到指定的 Email 地址 |
| ifempty  | 即使日志文件为空文件也做轮转，这个是 logrotate 的缺省选项 |
| notifempty | 当日志文件为空时，不进行轮转 |
| mail address | 把转储的日志文件发送到指定的 E-mail 地址 |
| nomail | 转储时不发送日志文件 |
| olddir directory | 转储后的日志文件放入指定的目录，必须和当前日志文件在同一个文件系统 |
| noolddir | 转储后的日志文件和当前日志文件放在同一个目录下 |
| sharedscripts | 运行 postrotate 脚本，作用是在所有日志都轮转后统一执行一次脚本。如果没有配置这个，那么每个日志轮转后都会执行一次脚本 |
| prerotate | 在 logrotate 转储之前需要执行的指令，例如修改文件的属性等动作；必须独立成行 |
| postrotate | 在 logrotate 转储之后需要执行的指令，例如重新启动 (kill -HUP) 某个服务！必须独立成行 |
| daily | 指定转储周期为每天 |
| weekly | 指定转储周期为每周 |
| monthly | 指定转储周期为每月 |
| rotate count | 指定日志文件删除之前转储的次数，0 指没有备份，5 指保留 5 个备份 |
| dateext | 使用当期日期作为命名格式 |
| dateformat .%s | 配合 dateext 使用，紧跟在下一行出现，定义文件切割后的文件名，必须配合 dateext 使用，只支持 %Y %m %d %s 这四个参数 |
| size(或minsize) log-size | 当日志文件到达指定的大小时才转储，log-size 能指定 bytes(缺省) 及 KB (sizek) 或 MB(sizem). 当日志文件 >= log-size 的时候就转储。 以下为合法格式：（其他格式的单位大小写没有试过）size = 5 或 size 5 （>= 5 个字节就转储）size = 100k 或 size 100k size = 100M 或 size 100M |
