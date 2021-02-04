---
title: "升级 Nginx + 为 Nginx 配置 PageSpeed 优化速度"
date: 2018-02-01T13:14:20+08:00
categories: [博客本体]
tags: [hexo,vps,nginx,pagespeed]
draft: false
---

ngx_pagespeed 是 google 为开发的对网页进行加速，压缩图片、压缩合并 CSS 和 JS 的 nginx 模块，对网页加速效果明显。

## 升级 Nginx

因为我们是使用 apt-get 安装的 nginx，所以这里按官网给的方法来更新。

查看 Ubuntu 版本

``` bash
lsb_release -a
```

例如返回

``` bash
Distributor ID: Ubuntu
Description:    Ubuntu 16.04.3 LTS
Release:        16.04
Codename:       xenial
```

编辑 /etc/apt/sources.list

``` bash
sudo vim /etc/apt/sources.list
```

在末尾添加

``` bash /etc/apt/sources.list
deb http://nginx.org/packages/ubuntu/ codename nginx #将codename换成刚才返回的，例如我的是xenial
deb-src http://nginx.org/packages/ubuntu/ codename nginx #将codename换成刚才返回的，例如我的是xenial
```

对于 Ubuntu，为了验证 nginx 存储库签名，并消除安装 nginx 包期间丢失 PGP 密钥的警告，有必要将用于签署 nginx 包和存储库的密钥添加到 apt 程序密钥环中。

``` bash
sudo wget http://nginx.org/keys/nginx_signing.key
sudo apt-key add nginx_signing.key
```

运行如下命令

``` bash
sudo apt-get update
sudo apt-get install nginx
```

提示如下，直接回车即可

``` bash
Configuration file '/etc/nginx/nginx.conf'
 ==> Modified (by you or by a script) since installation.
 ==> Package distributor has shipped an updated version.
   What would you like to do about it ?  Your options are:
    Y or I  : install the package maintainer's version
    N or O  : keep your currently-installed version
      D     : show the differences between the versions
      Z     : start a shell to examine the situation
 The default action is to keep your current version.
*** nginx.conf (Y/I/N/O/D/Z) [default=N] ?
```

## 为 Nginx 配置 PageSpeed 优化速度

### 下载 pagespeed-ngx

在下面挑选你需要的 pagespeed-ngx 版本

[GitHub 下载](https://github.com/apache/incubator-pagespeed-ngx/releases)

``` bash
wget https://github.com/apache/incubator-pagespeed-ngx/archive/latest-stable.tar.gz
tar -zxvf latest-stable.tar.gz
```

### 下载 nginx 源码

``` bash
sudo apt-get source nginx
```

进入 nginx 目录

``` bash
cd nginx-1.12.2
```

### 重新编译安装

查看一下自己装的 nginx 的编译参数

``` bash
nginx -V
```

显示如下

``` bash
nginx version: nginx/1.12.2
built by gcc 5.4.0 20160609 (Ubuntu 5.4.0-6ubuntu1~16.04.4)
built with OpenSSL 1.0.2g  1 Mar 2016
TLS SNI support enabled
configure arguments: --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-g -O2 -fstack-protector-strong -Wformat -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -fPIC' --with-ld-opt='-Wl,-Bsymbolic-functions -Wl,-z,relro -Wl,-z,now -Wl,--as-needed -pie'
```

重新编译，主要在末尾添加下面这段

``` bash
--add-module=../incubator-pagespeed-ngx-latest-stable
```

例如

``` bash
sudo ./configure --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-g -O2 -fstack-protector-strong -Wformat -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -fPIC' --with-ld-opt='-Wl,-Bsymbolic-functions -Wl,-z,relro -Wl,-z,now -Wl,--as-needed -pie' --add-module=../incubator-pagespeed-ngx-latest-stable
```

编译肯定不会通过，因为我们还有一个步骤，现在会出现如下提示

``` bash
ngx_pagespeed: pagespeed optimization library not found:
./configure: 31: ../incubator-pagespeed-ngx-latest-stable/config: [[: not found

  You need to separately download the pagespeed library:
     $ cd ../incubator-pagespeed-ngx-latest-stable
     $ wget https://dl.google.com/dl/page-speed/psol/1.12.34.2-x64.tar.gz
     $ tar -xzvf 1.12.34.2-x64.tar.gz # expands to psol/

  Or see the installation instructions:
     https://developers.google.com/speed/pagespeed/module/build_ngx_pagespeed_from_source
```

按提示操作

``` bash
cd ../incubator-pagespeed-ngx-latest-stable
wget https://dl.google.com/dl/page-speed/psol/1.12.34.2-x64.tar.gz
tar -xzvf 1.12.34.2-x64.tar.gz
cd ../nginx-1.12.2
```

然后重新编译

``` bash
sudo ./configure --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-g -O2 -fstack-protector-strong -Wformat -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -fPIC' --with-ld-opt='-Wl,-Bsymbolic-functions -Wl,-z,relro -Wl,-z,now -Wl,--as-needed -pie' --add-module=../incubator-pagespeed-ngx-latest-stable
```

可能会出现如下错误

``` bash
error: the HTTP rewrite module requires the PCRE library
error: SSL modules require the OpenSSL library.
```

此时安装后重新编译即可

``` bash
sudo apt-get update
sudo apt-get install libpcre3 libpcre3-dev
sudo apt-get install openssl libssl-dev
```

通过编译后

``` bash
sudo make
```

查看 nginx 安装位置

``` bash
whereis nginx
```

输出

``` bash
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx /usr/share/man/man8/nginx.8.gz
```

可以看到我们的 nginx 二进制文件是在 /usr/sbin/ 目录下的，我们备份下原来的，并用新的替代

``` bash
sudo mv /usr/sbin/nginx ../
sudo cp objs/nginx /usr/sbin
```

查看是否成功

``` bash
nginx -V
```

### 配置 pagespeed

新建缓冲目录

``` bash
sudo mkdir /var/ngx_pagespeed_cache/
```

创建配置文件

``` bash
cd /etc/nginx/conf.d
sudo vim pagespeed.conf
```

内容举例如下：

``` conf /etc/nginx/conf.d/pagespeed.conf
# on 启用，off 关闭
pagespeed on;
# html字符转小写
pagespeed LowercaseHtmlNames on;
# 配置服务器缓存位置和自动清除触发条件（空间大小、时限）
pagespeed FileCachePath /var/ngx_pagespeed_cache/;
pagespeed FileCacheSizeKb 2048000;
pagespeed FileCacheCleanIntervalMs 43200000;
pagespeed FileCacheInodeLimit 500000;
# 移除 html 空白
pagespeed EnableFilters collapse_whitespace;
# 移除 html 注释
pagespeed EnableFilters remove_comments;
# DNS 预加载
pagespeed EnableFilters insert_dns_prefetch;
# 压缩CSS
pagespeed EnableFilters rewrite_css;
# 合并CSS
pagespeed EnableFilters combine_css;
# 重写CSS，优化加载渲染页面的CSS规则
pagespeed EnableFilters prioritize_critical_css;
# google字体直接写入html 目的是减少浏览器请求和DNS查询
pagespeed EnableFilters inline_google_font_css;
# 压缩js
pagespeed EnableFilters rewrite_javascript;
# 合并js
pagespeed EnableFilters combine_javascript;
# 优化内嵌样式属性
pagespeed EnableFilters rewrite_style_attributes;
# 压缩图片
pagespeed EnableFilters rewrite_images;
# 不加载显示区域以外的图片
pagespeed LazyloadImagesAfterOnload off;
# 图片预加载
pagespeed EnableFilters inline_preview_images;
# 移动端图片自适应重置
pagespeed EnableFilters resize_mobile_images;
# 图片延迟加载
pagespeed EnableFilters lazyload_images;
# 雪碧图片，图标很多的时候很有用
pagespeed EnableFilters sprite_images;
# 扩展缓存 改善页面资源的可缓存性
pagespeed EnableFilters extend_cache;
```

在你的网页的配置文件中添加引用 pagespeed.conf

``` bash
sudo vim www.zxavier.com.conf
```

粘贴在 server 中

``` conf /etc/nginx/conf.d/www.zxavier.com.conf
...
server {
        ...
        include /etc/nginx/conf.d/pagespeed.conf;
        ...
}
...
```

测试

``` bash
sudo nginx -t
```

如果可以的话重新加载配置

``` bash
sudo systemctl reload nginx
```
