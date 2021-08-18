---
title: "升级MacOS自带Vim"
subtitle: ""
date: 2021-02-06T14:20:35+08:00
lastmod: 2021-03-02T11:30:05+08:00

author: "xavier"
authorLink: ""
description: "MacOS自带Vim版本较低，而且不能方便的升级。这对于Vim使用者我们来说是极为不方便的。"

tags: [brew,mac,vim]
categories: []

resources:
- name: "featured-image"
  src: ""
featuredImage: ""
featuredImagePreview: ""

draft: false
---

<!--more-->

MacOS自带Vim版本一般并不高，且不能方便的升级。这对于Vim使用者我们来说是极为不方便的。

{{< image src="old-vim.png" caption="old vim version" alt="old-vim" title="old-vim" >}}

可以看到这里默认的`vim`目录为`/usr/bin/vim`。

而说到Mac上软件到更新，就不得不提到包管理系统Brew。那如果能用Brew去管理vim，自然便是最方便的事情。关于Brew的安装这里就不在赘述了。

这里我们选择安装`MacVim`，并且把`Path`设置为`/usr/local/bin`，这样便是brew安装软件为首先项了。

``` bash
brew install --cask macvim

export PATH=/usr/local/bin:$PATH
```

{{< image src="new-vim.png" caption="new vim version" alt="new-vim" title="new-vim" >}}

此时`mvim`与`vim`均是打开brew安装的vim了。而`gvim`则是打开`gui`版本的`vim`
