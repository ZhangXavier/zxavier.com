---
title: "Bochs & NASM 的安装"
date: 2017-03-29T09:21:15+08:00
categories: [善用佳软]
tags: [nasm,bochs]
draft: false
---

最近看操作系统要用到 NASM 和 Bochs, 这里记录下安装。

<!--more-->

## 编译 NASM

1. 在[官网](http://sourceforge.net/projects/nasm/files/nasm%20sources/)下载`NASM`的源码
2. 输入

``` bash
tar -xvf  nasm-2.07.tar.gz
cd nasm-2.07
./configure
make
sudo make install
```

## 编译 Bochs

1. 在[官网](http://sourceforge.net/projects/bochs/files/bochs/)下载`Bochs`的源码
2. 确保安装编译前的依赖项

``` bash
sudo apt-get install build-essential
#gcc,make基本工具支持,注意bochs是用C++写的,可能需要安装g++-sudo apt-get install g++
sudo apt-get install libx11-dev
sudo apt-get install libxrandr-dev
sudo apt-get install xorg-dev #x window的图形的支持
sudo apt-get install libgtk2.0-dev
sudo apt-get install vgabios
```

3. 编译安装

``` bash
tar -xvf  bochs-2.6.8.tar.gz
cd bochs-2.6.8
./configure \
--prefix=/usr/local/bin/bochs268/bochs \
--enable-debugger \
--enable-disasm \
--enable-iodebug \
--enable-x86-debugger \
--with-x \
--with-x11
make
sudo make install
```
