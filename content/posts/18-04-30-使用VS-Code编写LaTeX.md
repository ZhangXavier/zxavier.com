---
title: "使用 VS Code 编写 LaTeX"
date: 2018-04-30T08:35:29+08:00
categories: [善用佳软]
tags: [vs code,latex]
draft: false
---

近日开始学习 LaTeX, 发现可以用 VS Code 编辑，很是方便，不过安装网上的教程安装后我并无法使用，后经改正现已正常使用，这里就和大家分享一下。由于我刚刚开始学习 LaTeX，所以有错误之处还是希望大家可以给我指出。

## 前期准备

首先跟大家列一下我的 LaTeX 的学习环境:

- Windows 10
- TeX Live 2017
- Visual Studio Code –1.22.1
- LaTeX Workshop –5.3.1 (VS Code 插件)
- SumatraPDF 阅读器

## LaTeX Workshop 配置

LaTeX Workshop 在升级到 5.0 版本后，配置上有了一些变动，这也使得网上相关的资料不多。不过据称仅仅是把过去的 `tool.chain` 改成了 `recipe`，应该是一样的。

### 编译方式（tool）

``` js
"latex-workshop.latex.tools": [
      {
        "name": "xelatex",
        "command": "xelatex",
        "args": [
          "-synctex=1",
          "-interaction=nonstopmode",
          "-file-line-error",
          "%DOCFILE%"
        ]
      },
      {
        "name": "latexmk",
        "command": "latexmk",
        "args": [
          "-synctex=1",
          "-interaction=nonstopmode",
          "-file-line-error",
          "-pdf",
          "%DOC%"
        ]
      },
      {
        "name": "pdflatex",
        "command": "pdflatex",
        "args": [
          "-synctex=1",
          "-interaction=nonstopmode",
          "-file-line-error",
          "%DOC%"
        ]
      },
      {
        "name": "bibtex",
        "command": "bibtex",
        "args": [
          "%DOCFILE%"
        ]
      }
    ],
```

> 注意：这里添加了 xelatex 的编译方式，并且是 `%DOCFILE%`，这里和网上其他的配置不一样的，不过在我电脑上这样才能正常编译。

另外 (关于上述编译方式的一点说明)：

1. TeX - pdfTeX - XeTeX 都是排版引擎，按照先进程度递增。
2. LaTeX 是一种格式，基于 TeX 格式定义了很多更方便使用的控制命令。上述四个引擎都有对应的程序将 LaTeX 格式解释成引擎能处理的内容。
3. CTeX,MiKTeX,TeX Live 都是 TeX 的发行，他们是许许多多东西的集合。
4. bibtex 是一种格式，用于协调 LaTeX 的参考文献处理。
5. latexmk 是 LaTeX 自动编译工具

### 编译组合（recipe）

``` js
"latex-workshop.latex.recipes": [
      {
        "name": "XeLaTeX",
        "tools": [
          "xelatex"
        ]
      },
      {
        "name": "PDFLaTeX",
        "tools": [
          "pdflatex"
        ]
      },
      {
        "name": "latexmk",
        "tools": [
          "latexmk"
        ]
      },
      {
        "name": "BibTeX",
        "tools": [
          "bibtex"
        ]
      },
      {
        "name": "pdflatex -> bibtex -> pdflatex*2",
        "tools": [
          "pdflatex",
          "bibtex",
          "pdflatex",
          "pdflatex"
        ]
      },
      {
        "name": "xelatex -> bibtex -> xelatex*2",
        "tools": [
          "xelatex",
          "bibtex",
          "xelatex",
          "xelatex"
        ]
      }
  ],
```

### 指定编译方式

LaTeX Workshop 可以在 lex 文件的首行指定编译方式（`% !TEX program`）以及主文档（`% !TEX root`）。`% !TEX program` 和 `% !TEX root` 被称为 `Magic Command`。

## 其他配置

设置 SumatraPDF 作为 PDF 阅读器。关闭自动编译。

``` js
"latex-workshop.view.pdf.viewer": "external",
"latex-workshop.latex.autoBuild.onSave.enabled": false,
"latex-workshop.view.pdf.viewer": "external",
"latex-workshop.latex.autoBuild.onSave.enabled": false,
```
