---
title: "想看 cron 的日志 （Linux日志 2）"
subtitle: "关于 rsyslog 的一点东西"
date: 2021-02-08T11:38:56+08:00
lastmod: 2021-03-08T11:38:56+08:00

author: "xavier"
description: "之前消失的 lastb 我们找到了“捣乱份子” logrotate 。这次没被记录的 cron 日志，背后又是因为谁呢？"

tags: [log,linux,rsyslog,cron]
categories: []

featuredImage: ""
featuredImagePreview: ""

draft: false
---

<!--more-->

## 前言

`cron` 是 `Linux` 上十分重要的服务。其基于时间的任务管理。让我们可以自由的在固定时间、日期、间隔下，运行定期任务。而其基于时间的任务管理，也让其日志有着重要的参考价值。可当我们去查看 `cron` 的时候，却发现其日志并没有被记录下来，这又是什么原因呢。

## rsyslog 日志管理

在之前的[消失不见的 lastb （Linux日志 1）](../21-02-07-log1-logrotate)一篇中，我们介绍了 `logrotate` 工具的一些内容。那 `cron` 的日志，是不是也要去找 `logrotate` 呢？其实并不是的，因为 `logrotate` 是一个日志分割管理软件，是对已有日志进行存储管理的。而这回则是日志是否保存的问题，这是由另外一个名为 `rsyslog` 的软件决定的。

## 修改 rsyslog 配置

其配置位于 `/etc/rsyslog.conf` ，找到其中 `cron` 的行如下所示：

``` conf
#cron.*       /var/log/cron.log
```

其被注释掉了，我们取消掉它的注释。

``` conf
cron.*       /var/log/cron.log
```

在重启 `rsyslog` 服务，即可

``` bash
sudo systemctl restart rsyslog.service
sudo systemctl restart cron.service
```

这时，我们在 `/var/log` 下面，应该就能找到 `cron` 的日志了。
