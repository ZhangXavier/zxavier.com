---
title: "在 Mac 上转换 GitBook"
subtitle: ""
date: 2021-10-02T15:38:55+08:00
lastmod: 2022-03-10T15:38:55+08:00

authors: [xavier]
authorLink: ""
description: ""

tags: []
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
一篇使用转换gitbook的文章

GitBook是一个使用Git和Markdown构建电子书的工具链。对于网上构建好的书籍

针对macOS Big Sur，因为新版的macOS更新了一些地址，导致网上相关的文章均无法使用。新版系统使用如下方法。

```Bash
sudo ln -s /Applications/calibre.app/Contents/MacOS/ebook-convert /usr/local/bin
```

这样我们就可以直接调用ebook-convert。

```Bash
❯ ebook-convert

用法: ebook-convert input_file output_file [options]

Convert an e-book from one format to another.

input_file is the input and output_file is the output. Both must be specified as the first two arguments to the command.

The output e-book format is guessed from the file extension of output_file. output_file can also be of the special format .EXT where EXT is the output file extension. In this case, the name of the output file is derived from the name of the input file. Note that the filenames must not start with a hyphen. Finally, if output_file has no extension, then it is treated as a folder and an "open e-book" (OEB) consisting of HTML files is written to that folder. These files are the files that would normally have been passed to the output plugin.

After specifying the input and output file you can customize the conversion by specifying various options. The available options depend on the input and output file types. To get help on them specify the input and output file and then use the -h option.

For full documentation of the conversion system see
https://manual.calibre-ebook.com/conversion.html

每当向具有它们自己空间的ebook-convert传递参数时，用引号括起这些参数。例如: "/some path/with spaces"

选项:
  --version       显示程序版本号并退出

  -h, --help      显示此帮助信息并退出

  --list-recipes  列出内建的订阅清单名。你可以通过如下命令创建基于内建订阅清单的电子书： ebook-convert "Recipe
                  Name.recipe" output.epub


作者 Kovid Goyal <kovid@kovidgoyal.net>
```
