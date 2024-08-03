---
title: "SQL Server2016 中文乱码问题"
date: 2016-12-05T14:33:12+08:00
categories: [善用佳软]
tags: [sql server]
draft: false
---

前一段时间在学习数据库，使用 SQL Server 2016 时，遇到了中文乱码的问题，在这里贴下解决方法。

<!--more-->

## 过程

1. 在数据库`属性`-`选项`-`排序规则`中改为 `Chinese_PRC_CI_AS`，如下图

    {{< image src="server2016.png" title="Cinese_PRC_CI_AS" >}}

2. 数据类型有中文就改成 `nchar` 或者是 `nvarchar` 存英文或者是数字才用 `char` 或者是 `varchar`, 如果还不能解决，则在用语句插入数据时，要在中文字符串前面添加 N 如：

    {{< admonition type=example >}}

    ``` sql
    insert into student
    values ('9512102', N'刘晨', N'男', '20', N'计算机系')
    ```

    {{< /admonition >}}
