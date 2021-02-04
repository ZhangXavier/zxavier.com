---
title: "Bochs 2.6.8 的配置文件 bochsrc.disk 修改"
date: 2017-03-29T09:21:27+08:00
categories: [善用佳软]
tags: [bochs]
draft: false
---

最近看《操作系统：真象还原》，由于上面使用的 Bochs 是 2.6.3 版本，由于现在 Bochs 2.6.8 相比之前有些改动，所以书上的配置文件不能直接运行，我在查找了一些资料后针对配置文件修改了下。

<!--more-->

## Bochs 2.6.8 配置文件

``` conf
###############################################
# Configuration file for Bochs
###############################################

# 第一步，首先设置Bochs在运行过程中能够使用的内存，本例为32MB。
# 关键字为：megs

megs: 32

# 第二步，设置对应真实机器的BIOS和VGA BIOS.
# 对应两个关键字为：romimage 和 vgaromimage
# BIOS已经修改，地址可以不加，可以根据文件大小进行推断，如果加地址要与文件大小相匹配

romimage: file=/usr/local/bin/bochs268/bochs/share/bochs/BIOS-bochs-latest
vgaromimage: file=/usr/local/bin/bochs268/bochs/share/bochs/VGABIOS-lgpl-latest

# 第三步，设置Bochs所使用的磁盘，软盘的关键字为floppy。
# 若只有一个软盘，则使用floppya即可，若有多个，则为floppya，floppyb...

#floppya: 1_44=a.img, status=inserted
# 第四步，选择启动盘符。

#boot: floppy  #默认为从软盘启动，把它注释掉
boot: disk

# 第五步，设置日志文件的输出。
log: bochs.out

# 第六步，开启或关闭某些功能。
# 下面是关闭鼠标，并打开键盘。
# 键盘的映射方式也改变了
mouse: enabled=0
keyboard:keymap=/usr/local/bin/bochs268/bochs/share/bochs/keymaps/x11-pc-us.map

#硬盘设置
ata0: enabled=1, ioaddr1=0x1f0, ioaddr2=0x3f0, irq=14
ata0-master: type=disk, path="hd60M.img", mode=flat #创建hd60M.img时系统提示添加的

######################################配置完成#######################################
```
