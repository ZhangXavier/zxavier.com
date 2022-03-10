---
title: "把 hugo 的秘密放进 env"
subtitle: "使用 secrets 部署 hugo"
date: 2021-08-22T15:17:27+08:00
lastmod: 2022-03-10T15:17:27+08:00

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

## 前言

最近更改了下博客的 CI/CD 流程，从私有部署的 Drone 改为了使用 GitHub 的 Actions。可这却有着巨大的安全漏洞。

<!--more-->

## 安全问题

为了使得博客更为开放、博客数据更为安全，我将博客的远程 Git 仓库，从我自己搭建的 [Gitea](https://git.zxavier.com) 改到了 GitHub 上。而对应的自动化部署，也就顺便从 [Drone](https://drone.zxavier.com) 改到了 GitHub Actions。

在使用 Github Actions 部署了几次博客后，我突然想到，博客在用的 Valine评论系统，使用的存储系统是 LeanCloud。而 LeanCloud 的 appId 和 appKey，就直白的写在 config.toml 中，这意味这这两项竟然是公开的。这意味着所有人都能拿到着两个值，来操作这个 LeanCloud 空间了，这可是重大安全漏洞。

另外：我还浏览了一些博客和它们在 Github 上的源码，发现还有好多博客的一些私密配置是公开的。

## 转危为安

现在的 CI/CD 工具，都有 Secrets 功能来存储一些私密的配置项，在编译部署的时候来读取，如 github 在 setting 中的 secrets、drone 在 setting 中的 secrets。

{{< image src="secrets.png" caption="secrets" alt="secrets" title="secrets" >}}

只要我们使用这些来配置 hugo 的 config.toml 既可。而 Hugo 是支持[使用系统环境变量配置](https://gohugo.io/getting-started/configuration/#configure-with-environment-variables)键值的。只需要将想配置的键，转换成大写，使用`_`来表示层级，再加前缀 `HUGO_` 即可。环境变量将覆盖在 `config.toml` 中的配置项。

如上面提到的 `params.page.comment.valine.appid` 将转换成环境变量`HUGO_PARAMS_PAGE_COMMENT_VALINE_APPID`。

使用 env 来在本地测试环境的配置，只需要将环境变量放置在需要测试的命令前即可：

```Bash
env HUGO_PARAMS_PAGE_COMMENT_VALINE_APPID="123" hugo server
```

现在就可以直接在 Secrets 中设置经过测试的配置了：

{{< image src="secret.png" caption="secret" alt="secret" title="secret" >}}

然后在 GitHub-actions 中引用配置好的 Secrets：

```YAML
- name: Build
  run: hugo --minify
  env:
    HUGO_PARAMS_PAGE_COMMENT_VALINE_APPID: ${{ secrets.LEANCLOUD_APPID }}
    HUGO_PARAMS_PAGE_COMMENT_VALINE_APPKEY: ${{ secrets.LEANCLOUD_APPKEY }}
    HUGO_PARAMS_ANALYTICS_GOOGLE_ID: ${{ secrets.GOOGLE_ANALYTICS_ID }}
```

最后附上我的 github actions 和 drone 脚本：

```YAML
on:
  push:
    branches:
      - main

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: 'recursive'
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: true

      - name: Build
        run: hugo --minify
        env:
          HUGO_PARAMS_PAGE_COMMENT_VALINE_APPID: ${{ secrets.LEANCLOUD_APPID }}
          HUGO_PARAMS_PAGE_COMMENT_VALINE_APPKEY: ${{ secrets.LEANCLOUD_APPKEY }}
          HUGO_PARAMS_ANALYTICS_GOOGLE_ID: ${{ secrets.GOOGLE_ANALYTICS_ID }}

      - name: rsync deployments
        uses: burnett01/rsync-deployments@5.0
        with:
          switches: -avzr --delete
          path: public/
          remote_path: ${{ secrets.REMOTE_TARGET }}
          remote_host: ${{ secrets.REMOTE_HOST }}
          remote_port: ${{ secrets.REMOTE_PORT }}
          remote_user: ${{ secrets.REMOTE_USER }}
          remote_key: ${{ secrets.SSH_PRIVATE_KEY }}
```
