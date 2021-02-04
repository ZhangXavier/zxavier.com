---
title: "MathType 不兼容解决方案"
date: 2017-01-01T15:46:32+08:00
categories: [善用佳软]
tags: [vs code,latex]
draft: false
---

MathType 在 office word 中会出现各种各样的兼容问题，在网上尝试了各种各样的方法后，经过总结和实测，这里给出一个可行的方法。

<!--more-->

我的环境：`win10 x64`+`office2016`(32 位和 64 位均测试成功)+`MathType 6.9b`。（估计其他类似，找到相应文件夹就可以）

## 过程

> 这里以 office2016 x64 为例，如果是 32 位的 office 把下面的 Program Files 换成 Program Files(x86), 关于 MathType 的路径中 64 换成 32 即可

1. 要确保路径被 office 信任，打开 word→文件→选项→信任中心→信任中心设置→添加新位置，添加 C:\Program Files\Microsoft Office\root\Office16\STARTUP\
{{< image src="MathType1.png" caption="添加信任路径" title="添加信任路径">}}

2. 然后在 MathType 安装目录下找到以下文件：

    - D:\Program Files(x86)\MathType\Office Support\64\WordCmds.dot
    - D:\Program Files(x86)\MathType\Office Support\64\MathType Commands 6 For Word 2016.dotm
    - D:\Program Files(x86)\MathType\MathPage\64\MathPage.wll

3. 将 WordCmds.dot 和 MathType Commands 6 For Word 2016.dotm, 拷贝到 C:\Program Files\Microsoft Office\root\Office16\STARTUP\
将 MathPage.wll 拷贝到 C:\Program Files\Microsoft Office\root\Office16

4. 关闭 word 重启。

{{< image src="MathType2.png" caption="成功" title="成功">}}

MathType 不兼容解决方案

## 其他问题

1. 最近使用中出先了 "MathType has detected an error inAutoExecCls.Main: 文件未找到……" 的错误如下图片。

{{< image src="error1.png" caption="开启报错" title="开启报错">}}

2. 点击 word 里面的 mathtype 后，就会弹出如下的提示 "运行错误 53，未找到 MathPage.WLL"

{{< image src="error2.png" caption="53错误" title="53错误">}}

测试发现上面的方法也可以解决
