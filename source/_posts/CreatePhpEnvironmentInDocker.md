---
title: Create PHP Environment With Docker
date: 2019-11-11 23:05:48
tags:
- docker
- php
- mysql
- nginx
clearReading: true
thumbnailImage: 20191111.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Back-end Knowledge
---
用docker构建PHP开发环境（mysql、nginx）
<!--more-->
### 开发环境
- Mac OS Mojave Version 10.14
- Docker for Mac

***
### 利用Docker构建开发环境的方法
首先，利用Docker构建PHP开发环境的时候，至少要具备Web服务器，PHP，和数据库这三个主要要素。
其次，用Docker准备你所需要的东西的话有两个方法：
- Docker Registory(DockerHub)开始一个一个的安装并build
- docker-compose.yml里写入你需要的条件，然后一口气安装

明显docker-compose会比较简单明了。

***
### Docker Compose
首先安装Docker Compose
```
curl -L https://github.com/docker/compose/releases/download/1.3.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```
注意：如果出现这个错误，Permission denied则说明/usr/lical/bin路径没有读写权限。
要使用superuser来执行。这个情况下执行sudo -i 之后再执行上面两个命令。

确认docker compose安装情况及版本
```
docker-compose version
```
***
### 步骤
Docker Compose会根据你写在docker-compose.yml文件里的内容来进行容器的管理。
首先新建一个文件夹，文件结构如下：
```
├── docker-compose.yml
├── nginx
│   └── nginx.conf
├── php
│   ├── Dockerfile
│   └── php.ini
├── mysql
│   └── data
└── www
    └── html
        └── index.php
```
***
### docker-compose.yml
然后就是制作作为地基的docker-compose.yml。
docker-compose.yml
```JS
version: '3'
services:
  nginx:
    image: nginx:latest
    ports:
      - 127.0.0.1.8080:8080
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/conf.d/default.conf
      - ./www/html:/var/www/html
    depends_on:
      - php

  php:
    build: ./php
    volumes:
      - ./www/html:/var/www/html
    depends_on:
      - db

  db:
    image: mysql:5.7
    ports:
      - 13306:3306
    volumes:
      - ./mysql/data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret

  phpmyadmin:
    image: phpmyadmin/phpmyadmin:latest
    ports:
      - 8888:80
    depends_on:
      - db
```

nginx/nginx.conf
```JS
server {
    listen 8080;
    server_name _;

    root  /var/www/html;
    index index.php index.html;

    access_log /var/log/nginx/access.log;
    error_log  /var/log/nginx/error.log;

    location / {
        try_files $uri $uri/ /index.php$is_args$args;
    }

    location ~ \.php$ {
        fastcgi_pass php:9000;
        fastcgi_index index.php;    
        fastcgi_param SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include       fastcgi_params;
    }
}
```

php/Dockerfile
```JS
FROM php:7.2-fpm
COPY php.ini /usr/local/etc/php/
```

php.ini
```
date.timezone = "Asia/Tokyo"
```

www/html/index.php
```
<php
phpinfo();
```
***
### 启动容器
```
docker-compose up -d
```
<br>
<img src="./1.png" style="width:600px">

打开localhost:8080就可以看到php的设置画面了。

<br>
<img src="./2.png" style="width:600px">
<br>

### 停止容器
```
docker-compose down
```
***
### mysql
