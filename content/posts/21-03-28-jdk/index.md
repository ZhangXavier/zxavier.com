---
title: "JDK 版本大乱斗"
subtitle: "在 macOS 上安装 JDK"
date: 2021-04-01T16:10:57+08:00
lastmod: 2021-04-01T16:10:57+08:00

authors: [xavier]
authorLink: ""
description: ""

tags: []
categories: []

resources:
- name: "featured-image"
  src: ""
featuredImage: ""
featuredImagePreview: ""

draft: true
---

<!--more-->

## 历史的沿革

首先让我们先一览 Java 所属公司的历史：

1995年5月，Sun 公司发布了 Java。

2006年，Sun 公司在 JavaOne 宣布了 GPL 协议的 Java 的开源计划。之后便一直在实现其计划。

2009年4月15日，在去掉了闭源内容后，Sun 正式发布了 GPL 协议的 OpenJDK。

2009年4月20日（正式发布了 OpenJDK 的5天后），Oracle 宣布收购 Sun 公司。

2010年1月，收购完成。Oracle 接管了 OpenJDK 的项目。

## 两者的区别

其实两者的区别是很小的，最主要的区别便是在许可证上的不同。

在早期，Sun JDK 的能用于商业的许可证是 SCSL（Sun Community Source License）。而伴随着 Sun JDK6 的发布，JRL（Java Research License）许可证开始使用。

在当时，就代码完整性来说，Sun JDK 代码最完整，SCSL 版的代码要比 Sun JDK 版本的少了一些完全不开放的代码，不过这些代码是极其少的。而 JRL 版的代码，又比 SCSL 版的少了一些 closed 目录的内容。不过这些差异的代码并不重要。

此时，已经在开发 JDK7 了。开发的主干便考虑到了 GPL 协议的问题。在开发完成后，其主干开发版便是 OpenJDK 7 了。另外在主干上开了一个 OpenJDK6 的分支，这个分支尽量去除 Java SE7 的新特性，使其尽量符合 Java6 的标准。这便是发布的 OpenJDK6.

而 Oracle JDK7 是在 OpenJDK7 的基础上继续开发，替换掉了少量的源代码。使用 JRL(Java Research License，Java 研究授权协议) 发布。

而到了 JDK8 的时代，因为 OpenJDK 要以 GPLv2 发布，而之前的 JS 引擎 Rhino 是 MPL 协议，与其不兼容。便在 OpenJDK8 里面，使用了新的 Nashorn 引擎以替代 Rhino。而 OracleJDK8 里面，则继续保留了 Rhino 引擎。

在陆陆续续多年的迭代下，现在 OpenJDK 与 OracleJDK 的差异已经越来越小了，而越来越多的公司也从 OracleJDK 转换到了 OpenJDK 的怀抱。16年，Google 便在 Android 7 中，将 JDK 改为了 OpenJDK。

而 Oracle JDK 规定，在一个 Oracle JDK 的生命周期内可以免费商用，而生命周期外可以继续以学习为目的使用，而继续商用则需要为后续的更新付费。所以如果一直想免费使用 Oracle JDK 就需要一直使用其最新版本。

PS：网上有一些人说，OpenJDK 不支持 jstack，jmap，jstat 等工具。这其实是不对的。可以在 JDK 安装目录下的 bin 目录下，查看自己是否安装了这些命令。实测我电脑上安装的adoptopenjdk8，adoptopenjdk11，adoptopenjdk16 都是默认自带的这些工具的。

## 各个版本的JDK

在 Apple 推出了 ARM 芯片 M1 版的 MacBook 后，JAVA 开发者自然想去寻找一个适配了 M1 芯片的JDK。而 zulu jdk 是最先适配了 M1 芯片，一时间大火。那么这个 zulu jdk 是什么呢，它又和 openjdk 有何关系呢。

之前我们说，JDK 有 Oracle JDK 和 Open JDK 两种。而 OpenJDK 其中又包括 Oracle 编译的 OpenJDK 和第三方团体编译的 OpenJDK。

### Oracle JDK

如果想安装 Oracle JDK，

### OpenJDK

这个便是由 Oracle 编译的 OpenJDK

### AdoptOpenJDK

这是一个开源团体

### Zulu JDK

这是由 Azul Systems 公司提供的 JDK，这是一个商业公司。现在在为微软的 Azure 提供 JDK 服务。
