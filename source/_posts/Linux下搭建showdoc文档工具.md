---
title: Linux下搭建showdoc文档工具
date: 2017-11-28 22:23:37
tags: [linux,showdoc]
categories: Linux
---

## Linux下搭建showdoc文档工具

ShowDoc是一个非常适合IT团队的在线文档分享工具，它可以加快团队之间沟通的效率。可以用来编写API文档、数据字典、说明文档，同时包括分享与导出、权限管理、及编辑等功能。如果你有服务器，这篇教程可以帮助你将它部署到自己的服务器上。当然，如果你没有的话也可以使用ShowDoc的官方服务http://www.showdoc.cc。

<!-- more -->

### 1、准备 Nginx + PHP 环境

#### 安装 Nginx

使用 yum 安装 Nginx：

```shell
yum install nginx
```

修改 /etc/nginx/nginx.conf 文件为如下内容：
```shell
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    
    access_log  /var/log/nginx/access.log  main;
    
    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;
    
    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;
    include /etc/nginx/conf.d/*.conf;
    
    server {
        listen       80;
        server_name  127.0.0.1;
        root         /var/www/html;
        index index.php index.html
        error_page  404              /404.html;
        location = /40x.html {
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
        }
        location ~ .php$ {
            root           /var/www/html;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
        location ~ /.ht {
            deny  all;
        }
    }
}
```

启动 Nginx 并设置为开机启动：

```shell
service nginx start
chkconfig nginx on
```

#### 安装 PHP
使用 yum 安装 php-fpm：
```shell
yum install php php-gd php-fpm php-mcrypt php-mbstring php-mysql php-pdo
```
启动 php-fpm 并设置为开机启动：
```shell
service php-fpm start
chkconfig php-fpm on
```
### 2、创建项目
#### 下载安装 Composer
Composer 是 PHP 的一个依赖管理工具，推荐使用 Composer 创建 ShowDoc 项目。
执行如下命令安装 Composer:

```shell
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
```
#### 设置 Composer 使用国内镜像
执行命令设置 Composer 使用国内镜像：
```shell
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```
#### 使用 Composer 创建项目
执行命令创建项目：
```shell
cd /var/www/html/ && composer create-project  showdoc/showdoc
```
#### 设置 showdoc 目录写权限
执行命令赋予 showdoc 下部分目录的写权限:
```shell
chmod a+w showdoc/install
chmod a+w showdoc/Sqlite
chmod a+w showdoc/Sqlite/showdoc.db.php
chmod a+w showdoc/Public/Uploads/
chmod a+w showdoc/Application/Runtime
chmod a+w showdoc/server/Application/Runtime
chmod a+w showdoc/Application/Common/Conf/config.php
chmod a+w showdoc/Application/Home/Conf/config.php
```
创建完毕，您现在可以通过浏览器访问 http://119.29.239.104/showdoc/install/ ，进行语言的选择以后即可通过 http://119.29.239.104/showdoc 查看站点效果。
### 3.准备域名和解析
#### 域名注册与解析
注：如果您不需要通过域名访问您的站点，可以忽略该步骤，如果您需要使用域名，可以先去购买域名并进行解析，这里不过多介绍。
#### 大功告成
至此您的 ShowDoc 站点已经部署完成，您可以通过浏览器访问查看效果。通过IP地址查看：http://yourip/showdoc也可以通过域名查看：http://www.yourdomain.com/showdoc。