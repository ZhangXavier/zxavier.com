---
title: "NextCloud优化"
date: 2018-04-26T08:35:29+08:00
categories: [折腾有理]
tags: [vps,nextcloud]
draft: false
---

自从上次 NextCloud 搭建后已经好久没有讲过它了，最近使用 NextCloud 发现它可以更新了，那就顺便说下 NextCloud 的优化吧～

## 优化

> PHP 的设置似乎有问题，无法获取系统环境变量。使用 getenv ("PATH") 测试时仅返回空结果.

修改 php 的 www.conf 文件

``` bash
sudo vim /etc/php/7.0/fpm/pool.d/www.conf
```

通常情况下，你会发现文件中已经有一些或全部环境变量，但是像这样注释掉了：

``` conf /etc/php/7.0/fpm/pool.d/www.conf
/etc/php/7.0/fpm/pool.d/www.conf
;env[HOSTNAME] = $HOSTNAME
;env[PATH] = /usr/local/bin:/ usr/ bin：/bin
;env[TMP] = /tmp
;env[TMPDIR] = /tmp
;env[TEMP] = /tmp
```

把前面的 ; 删除掉，取消注释
重启 php-fpm

``` bash
sudo systemctl restart php7.0-fpm
```

> The “X-Frame-Options” HTTP header is not set to “SAMEORIGIN”. This is a potential security or privacy risk, as it is recommended to adjust this setting accordingly.
> The “Strict-Transport-Security” HTTP header is not set to at least “15552000” seconds. For enhanced security, it is recommended to enable HSTS as described in the security tips.

修改 nginx 配置

``` bash
sudo vim /etc/nginx/conf.d/xxx.conf
```

``` conf /etc/nginx/conf.d/xxx.conf
server {
    listen 443 ssl http2;
     ......
     add_header X-Frame-Options SAMEORIGIN;
     add_header Strict-Transport-Security "max-age=15552000; includeSubdomains" always;
     ......
}
```

``` bash
sudo vim /etc/php/7.0/fpm/php.ini
```

开启 opcache

输入 /opcache 找到 opcache 的相关配置，找到下面的项修改

``` conf
/etc/php/7.0/fpm/php.ini
[opcache]
; Determines if Zend OPCache is enabled
opcache.enable=1

; Determines if Zend OPCache is enabled for the CLI version of
opcache.enable_cli=1

; The OPcache shared memory storage size
opcache.memory_consumption=128

; The amount of memory for interned strings in Mbytes.
opcache.interned_strings_buffer=8

; The maximum number of keys (scripts) in the OPcache hash table.
; Only numbers between 200 and 1000000 are allowed.
opcache.max_accelerated_files=10000

; How often (in seconds) to check file timestamps for changes to the shared
; memory storage allocation. ("1" means validate once per second, but only
; once per request. "0" means always validate)
opcache.revalidate_freq=1

; If disabled, all PHPDoc comments are dropped from the code to reduce the
; size of the optimized code.
opcache.save_comments=1
```

重启 php7.9-fpm

``` bash
sudo systemctl restart php7.0-fpm
```
