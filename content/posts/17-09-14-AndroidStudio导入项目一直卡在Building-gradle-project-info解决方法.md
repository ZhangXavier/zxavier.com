---
title: "AndroidStudio 导入项目一直卡在 Building-gradle-project-info 的解决方法"
date: 2017-09-06T08:07:23+08:00
categories: [善用佳软]
tags: [android,android studio]
draft: false
---

AndroidStudio 导入项目一直卡在 Building gradle project info，实际上是因为你导入的这个项目使用的 gradle 与你已经拥有的 gradle 版本不一致，导致需要下载该项目需要的 gradle 版本，不知是被墙了还是什么原因，反正就是会一直卡住，直至下载完成 (如果能下载完成的话 :joy:)

## 解决方案 一

1. 随便找一个你能运行的 androidstudio 项目

2. 打开 gradle-wrapper.properties，文件目录：`./gradle/wrapper/gradle-wrapper.properties`

3. 复制 distributionUrl 这一整行的内容，eg: `distributionUrl=https\://services.gradle.org/distributions/gradle-2.4-all.zip`

4. 打开你要导入的项目的 `gradle-wrapper.properties` ，具体步骤与步骤 2 相同

5. 把步骤 3 复制的内容，替换你要导入的项目的 `gradle-wrapper.properties` 文件的 `distributionUrl` 这一行

6. 再重启 android studio，导入项目就可以了

## 解决方案 二

另外一种更简单的方法就是去官网下载 gradle 的版本。我把它放到了我的[百度云盘](http://pan.baidu.com/s/1dFB1Kpr)(密码：2165) 里了，需要的同学可以下载。
