---
title: "关于字体那些问题"
subtitle: "由更纱字体到更多"
date: 2021-04-01T18:27:44+08:00
lastmod: 2021-04-01T18:27:44+08:00

author: "xavier"
authorLink: ""
description: ""

tags: [font]
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

<!--more-->

## 关于字体

## 接触字体

我上小学的时候，学校经常会布置一份家庭作业 ———— 做手抄报。做的好的还会被贴在班级墙上，甚是风光。一份可以被贴上墙的手抄报，排版、画画、剪贴、抄写各个方面都是要精益求精的。可这对于将上墙作为目标的我来说，无疑是一个浩大的工程。每次为了上墙，都至少要花费我周末一天的时间。

后来的某一天，我发现有一位同学交了一份打印的电子报，而且竟然通过了老师的验收关。这对于我来说，就像哥伦布发现了新大陆一般，打印的“手抄报”竟然也可以通过，如果设计、制作的够好甚至也能被表扬。而打印相比手抄可是省事了不止一点啊，排版、内容都方便了不少，果然科学技术就是第一生产力！

在学着制作电子“手抄报”的期间，我第一次接触到了 Word 里的各种字体和艺术字。没想到这里就内置了这么多好看的字体啊，只要动动鼠标，就比我自己辛辛苦苦手绘的要漂亮多了！而这也是我第一次知道原来世界上有这么多好看字体。

## 字体推荐

而优秀的字体自然不能被尘封，只有推荐出来，被使用才能发挥它们最大的价值！这里就先推荐一些和编程更配的，也是经常被大家推荐的一些字体。

1. 更纱黑体 sarasa
2. 文泉驿字体
3. 思源黑体
4. Fire连体字体
5. jetbrains 字体

## 一些字体说明

在安装这些字体的时候，会遇到一些名词，这里解释如下。

### unhint

- unhint 表示字体是不微调的，适合在 macOS 上使用。
- 微调的适合 Windows 和 Linux

### 字体粗细相关

| 字体 | 说明 | 备注 |
| :---: | :---: | :--- |
| extralight | 特别细 |  |
| light | 细的 |  |
| regular | 正常的 |  |
| semibold | 稍微有点粗的 |  |
| bold | **粗的** |  |
| italic | 倾斜的 | 在一些编辑器里面可以显示为手写体 |

### Serif

衬线，指的是字母笔画头上有没有小短线。具体说明可以看 [维基百科 -- 衬线体](https://zh.wikipedia.org/zh-hans/%E8%A1%AC%E7%BA%BF%E4%BD%93)。

{{< image src="serif-en.png" alt="serif-en" title="衬线体-英文" >}}
{{< image src="serif-zh.png" alt="serif-zh" title="衬线体-中文" >}}

### 关于 sarasa 的补充说明

对于 sarasa 需要安装的字体。

首先 SC 就是简体中文，选这个就 ok。

其次，slab 就是{{< relref "#Serif" >}}衬线，这个在上面已经介绍过了。

`sarasa-mono` 安装后显示为`等距更纱黑体`，其特点是全角的中文省略号，没必要安装，因为用于编程才需要等距，在编程时不需要全角的中文省略号。

`sarasa-gothic` 安装后显示为 `更纱黑体` 是全角的中文省略号和引号，可以安装，因为都是全角的，适合日常用。

`sarasa-ui` 安装后显示为 `更纱黑体ui` 是全角中文省略号和半角引号，相比下 gothic 更适合一些，因为只有一部分是全角的。

`sarasa-fixed` 安装后显示为，是不支持连字的。

`sarasa-term` 安装后显示为，是支持连字的，编程可以优先选这个。

其后缀的意义如下：

引号（“”）为全角 —— Gothic

引号（“”）很窄 —— UI

有连字，Em破折号（----）为全宽 —— Mono

有连字，Em破折号（----）是半角 —— Term

无连字，破折号（----）为半角 —— Fixed

正字法尺寸

CL：古典拼字法

SC，TC，J，K，HC：区域拼字法，遵循Han Sans标记法。

在更纱黑体中，有以下几种字型：

cl：代表 Cyrillic & Latin，包含西里尔和拉丁字符  （根据「思源黑体」家族 cl 字体定义推想，不确定在「更纱黑体」中是否亦如此）

hc: 代表 Hong Kong Chinese（或 CJK Hong Kong），即港版汉字（根据「思源黑体」家族 hc 字体定义推想，不确定在「更纱黑体」中是否亦如此。另外「朝鲜汉字」也有一个本地化的英文名词“Hanja”，但中日港台都用意译英文单词而非本地化的特定名词表示字体名称，所以我不认为在这里 hc 代表 Hanja Chinese，除非作者特意为之。）

j：代表 Japanese，即日文，包含和制汉字

sc：代表 Simplified Chinese，即简体中文，就是简化字

tc：代表 Traditional Chinese，即传统中文，就是繁体字（或称「正体字」）

其他术语：

LGC：代表 Latin-Greek-Cyrillic subset，即拉丁-希腊-西里尔字符集

mono：代表 monospace，即等宽字体

还有

1.LGC 设定为思源黑体（Noto Sans / Source Han Sans）字型的情形：

引号为全角时，为 Gothic（哥特式）字体

引号比较窄时，为 UI（用户界面）字体

1. LGC 设定为 losevka 字型的情形：

包含 ligature（连体字符）以及 Em dashes（即长连接符，长度为 En dashes 的两倍 ，而最短的称作 hyphen）为全角时，是 MonoT 字体

包含 ligature（连体字符）以及 Em dashes 为半角时，是 Mono 字体

没有 ligature（连体字符）而 Em dashes 为半角时，是 Term 字体
