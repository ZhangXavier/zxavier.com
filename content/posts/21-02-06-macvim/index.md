---
title: "升级 MacOS 自带 Vim "
subtitle: ""
date: 2021-02-06T14:20:35+08:00
lastmod: 2021-03-02T11:30:05+08:00

author: "xavier"
authorLink: ""
description: " MacOS 自带 Vim 版本较低，而且不能方便的升级。这对于 Vim 使用者我们来说是极为不方便的。"

tags: [brew,mac,vim]
categories: []

featuredImagePreview: "new-vim.png"

draft: false
---

<!--more-->

MacOS 自带 Vim 版本一般并不高，且不能方便的升级。这对于 Vim 使用者我们来说是极为不方便的。

{{< image src="old-vim.png" caption="old vim version" alt="old-vim" title="old-vim" >}}

可以看到这里默认的 `vim` 目录为 `/usr/bin/vim`。

而说到 Mac 上软件到更新，就不得不提到包管理系统 Brew。那如果能用 Brew 去管理 vim，自然便是最方便的事情。关于 Brew 的安装这里就不在赘述了。

这里我们选择安装 `MacVim`，并且把 `Path` 设置为 `/usr/local/bin`，这样便是 brew 安装软件为首先项了。

``` bash
brew install --cask macvim

export PATH=/usr/local/bin:$PATH
```

{{< image src="new-vim.png" caption="new vim version" alt="new-vim" title="new-vim" >}}

此时 `mvim` 与 `vim` 均是打开 brew 安装的 vim 了。而 `gvim` 则是打开 `gui` 版本的 `vim`
