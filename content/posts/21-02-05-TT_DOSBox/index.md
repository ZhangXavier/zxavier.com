---
title: "打字练习利器 -- TT"
subtitle: "在DOSBox上使用TT练习键入速度"
date: 2021-02-05T16:56:16+08:00
lastmod: 2021-02-05T16:56:16+08:00

description: "在DOSBox上使用TT练习键入速度"

tags: ["DOSBox", "TT"]
categories: []

resources:
- name: "featured-image"
  src: ""
featuredImage: ""
featuredImagePreview: ""

lightgallery: true
license: ""

draft: false
---

<!--more-->

对于编程来说，键入速度的提高似乎意义不大。好像是思路有了，键入几个关键字，是一个无比简单的事情。但是其实日常的实践告诉我事实并不是这样的，对于大部分场景，如debug之类的，其实是在边输入边思考的。在这些场景下，键入速度意味着一个连贯的、肌肉记忆的输入体验，这种下意识的输入，对于一个连贯思考过程是大有裨益的。

在知乎里[如何练习编程的手速？ - 韦易笑的回答](https://www.zhihu.com/question/27021761/answer/53323794)里，韦易笑老师给出一个使用 `TT`，练习的建议，相当推荐阅读。

## DOSBox

在[win10 中使用 debug](https://www.zxavier.com/17-09-20-win10%E4%B8%AD%E4%BD%BF%E7%94%A8debug/)中，已经介绍过 DOSBox 了，这里不再赘述。

## 使用TT

韦易笑不光给出了推荐，还制作了 `TT放大版.BAT`。
下载地址（DosBox版本）：[TT-Dosbox.7z](http://www.skywind.me/mw/images/e/eb/TT-Dosbox.7z)

运行 `DOSBox`

在 `DOSBox` 里输入

``` cmd
mount c ~/Developer/TT
c:
tt.exe
```

这样就可以进行练习了。

## 一些其他的网站

[typing](https://typing.io/)

这个网站里，是用一些代码片段做为训练文本的，在无法使用 `TT` 的时候推荐使用。
