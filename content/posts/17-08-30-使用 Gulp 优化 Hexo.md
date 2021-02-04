---
title: "使用 Gulp 优化 Hexo"
date: 2017-08-30T20:05:53+08:00
categories: [博客本体]
tags: [blog,hexo,gulp]
draft: false
---

## Gulp

gulp 是前端开发过程中对代码进行构建的工具，是自动化项目的构建利器，能对网站资源进行优化，也能对开发中重复的任务使用正确的工具自动完成，这里我们用它优化我们 hexo 博客的访问速度。

[Gulp-GitHub](https://github.com/gulpjs/gulp/tree/master/docs)
[Gulp 官方网址](http://gulpjs.com)
[Gulp 中文官方网址](http://www.gulpjs.com.cn/)

## 安装 Gulp

由于[NPM](https://www.npmjs.com/)的服务器在国外，为了加快安装速度大家可以用[淘宝 NPM 镜像](https://npm.taobao.org/)地址下载，首先安装 cnpm 如下：

``` bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

使用 cnpm 代替 npm 速度就可以快一些啦～

``` bash
npm install -g gulp
#cnpm install -g gulp
```

## 安装 gulp 插件

然后按以下清单文件安装，同样可以把下面的 npm 改为 cnpm。

``` bash
npm install gulp --save
npm install gulp-htmlclean --save
npm install gulp-htmlmin --save
npm install gulp-imagemin --save
npm install gulp-clean-css --save
npm install gulp-uglify --save
npm install gulp-concat --save
```

其中 gulp 是工程的核心程序，Gulp 采用插件方式进行工作，下面的 5 个文件就是基于 Gulp 的插件[.Gulp](https://gulpjs.com/)插件列表。
这里可以看到[gulp-imagemin](https://www.npmjs.com/package/gulp-imagemin)的详细介绍。

新建 gulpfile.js 文件
在网站的目录下面新建文件 gulpfile.js 内容是 gulp 的执行方法，下面放出我的文件内容。

``` js
gulpfile.js
var gulp = require('gulp');

//模块获取
var cleancss = require('gulp-clean-css');
var uglify = require('gulp-uglify');
var htmlmin = require('gulp-htmlmin');
var htmlclean = require('gulp-htmlclean');
var imagemin = require('gulp-imagemin')
var concat = require('gulp-concat');

// 压缩 public 目录 css文件
    gulp.task('minify-css', function() {
        return gulp.src('./public/**/*.css')
            .pipe(cleancss({compatibility: 'ie8'}))
            .pipe(gulp.dest('./public'));
    });

    // 压缩 public 目录 html文件
    gulp.task('minify-html', function() {
      return gulp.src('./public/**/*.html')
        .pipe(htmlclean())
        .pipe(htmlmin({
             removeComments: true,
             minifyJS: true,
             minifyCSS: true,
             minifyURLs: true,
        }))
        .pipe(gulp.dest('./public'))
    });

    // 压缩图片
    gulp.task('minify-images', function () {
        gulp.src('./public/images/*.*')
            .pipe(imagemin({
                progressive: true
            }))
            .pipe(gulp.dest('./public/images/'))
    });

    gulp.task('minify-image', function () {
        gulp.src('./public/image/*.*')
            .pipe(imagemin({
                progressive: true
            }))
            .pipe(gulp.dest('./public/image/'))
    });

    gulp.task('minify-images-avatar', function () {
        gulp.src('./public/uploads/*.*')
            .pipe(imagemin({
                progressive: true
            }))
            .pipe(gulp.dest('./public/uploads/'))
    });

    // 压缩 public/js 目录 js文件
    gulp.task('minify-js', function() {
        return gulp.src('./public/js/**/*.js')
            .pipe(uglify())
            .pipe(gulp.dest('./public/js'));
    });

    //public/lib/fancybox-js压缩
    gulp.task('minify-fancyboxjs', function() {
        return gulp.src('./public/lib/fancybox/source/**/*.js')
            .pipe(uglify())
            .pipe(gulp.dest('./public/lib/fancybox/source/'));
    });

    //public/lib/jquery_lazyload-js压缩
    gulp.task('minify-jquerylazyloadjs', function() {
        return gulp.src('./public/lib/jquery_lazyload/*.js')
            .pipe(uglify())
            .pipe(gulp.dest('./public/lib/jquery_lazyload/'));
    });

    //合并 JS
    gulp.task('jsall', function () {
        return gulp.src('./public/**/*.js')
        //压缩后重命名
            .pipe(concat('app.js'))
            .pipe(gulp.dest('./public'));
    });

    // 执行 gulp 命令时执行的任务
    gulp.task('default', [
        'minify-html','minify-css','minify-images','minify-image','minify-images-avatar','minify-js','minify-fancyboxjs','minify-jquerylazyloadjs','jsall'
    ]);
```

本来是直接复制网上的配置的，结果运行下出现了以下错误：

``` bash
events.js:160
      throw er; // Unhandled 'error' event
```

结合网上的回答得知是因为像`.min.js`这类后缀的 js 无须再次压缩，而像我这种水水的人只好一下一下根据网站目录设置压缩路径，并且结合了下结合 google 的网页加载速度的分析，写出了现在的这个配置文件。
另外注意修改目录成自己目录。

## 运行 gulp

之后我们添加文章之后，编译发布之间需要多压缩命令：

``` bash
hexo clean  # 删除public文件夹
hexo g      # 编译文章，生成public文件夹
gulp        # 压缩文件
hexo d      # 部署到网站
```

现在，相信你的博客速度已经快起来啦。
