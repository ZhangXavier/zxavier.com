---
title: "由 screenfetch 到 brew taps"
subtitle: ""
date: 2021-02-06T14:42:28+08:00
lastmod: 2021-02-06T14:42:28+08:00

authors: [xavier]
authorLink: ""
description: "从 brew 的 screenfetch 在 Big Sur 上表现不佳，到创建自己到 taps"

tags: [brew,screenfetch,mac]
categories: []

featuredImagePreview: "macos-logo.webp"

draft: false
---

<!--more-->

在 `Big Sur` 上使用 `brew` 安装了 `screenfetch`，结果发现输出的图标却是 `unix`。

{{< image src="unix-logo.webp" caption="unix-logo" alt="unix-logo" title="unix-logo" >}}


在 `Github` 上一看，原来是因为 `Big Sur` 的ProductName由 `Mac OS X` 更改为了 `macOS`。当然，这个问题已经在最新的开发版里解决掉了。[#692 (comment)](https://github.com/KittyKatt/screenFetch/issues/692#issuecomment-726631900)

由于 `screenfetch` 并没有发布新的版本号，`brew` 仓库里还是老版本，所以出现了问题。按照 `screenfetch` 的 wiki [Installation](https://github.com/KittyKatt/screenFetch/wiki/Installation) 下载 `dev` 版本的 `screenfetch-dev`，替换掉 `/usr/local/Cellar/screenfetch/3.9.1/bin` 下的 `screenfetch`，问题即可成功解决。

## 自己的Tap

由于在 `screenfetch` 新版本号发布前，每次都需要这样更新并不方便。而且官方的 `tap`，在很多时候并不能方便的跟进最新源码，并解决bug。所以以此为契机，制作一个自己的 `tap`，无疑是一个方便的选择。

[Creating a tap](https://docs.brew.sh/How-to-Create-and-Maintain-a-Tap#creating-a-tap)

在参考了一些官方的 `Taps`，后制作了我自己的 tap。地址如下：[Xavier's Homebrew Taps](https://github.com/ZhangXavier/homebrew-xtaps)

现在只需要使用下面这行代码，便可安装最新版的 `screenfetch` 啦。

``` bash
brew install ZhangXavier/xtaps/screenfetch
```

新版安装后，问题已经得到了解决：

{{< image src="macos-logo.webp" caption="Big Sur" alt="macos-logo" title="macos-logo" >}}

当然在 `Catalina` 里，也是正常的：

{{< image src="osx-logo.webp" caption="Catalina" alt="macos-logo" title="macos-logo" >}}
