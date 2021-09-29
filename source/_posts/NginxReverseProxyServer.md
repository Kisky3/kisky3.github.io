---
title: Nginx Basic and Create a Reverse Proxy Server With Docker
date: 2021-08-16 09:17:28
tags:
  - Nginx
  - Reverse Proxy Server
clearReading: true
coverCaption: 'Hello World, Hello Programming'
coverSize: partial
comments: false
categories: Back-end Knowledge
---

Nginx 的基础以及用 Docker 创建一个 Nginx 反向代理服务器

<!--more-->

这篇文章是用于介绍 Ngin 的基本知识，
主要包括 Nginx 的启动,停止,以及如何使用 Nginx 和 Docker 来搭建一个反向代理服务器。

### 前提条件

其实反向代理服务器的话概念很广,有 HTTP 反向代理和 TCP 的反向代理。
这次我们就用 Nginx 来搭建一个本地 HTTP 的反向代理服务器。
请自己在系统里安装好 Nginx,详细请参照[Nginx 的安装](https://runebook.dev/ja/docs/nginx/install)

### 什么是 Nginx

Nginx 是 Web 服务器里面作为反向代理服务器的优秀服务器软件代表！

一般情况下 Web 服务器都使用特定的 application(Apacge or Nginx)之类的来监听特定的番号(port),以此来连接 TCP 和客户端。客户端发送 HTTP 请求的时候, 做好特定处理之后再将服务器返回的响应传送回去客户端。HTTP 的反向代理能够利用客户端的请求的不同的 HTTP path 来将其连接到到不同的服务器上。

今天就试着在 Web Site 里造一个汪星人用的 content, 再造一个猫星人用的 content,可以通过各自 URL 的 path 连接到网页。比如客户端向 nginx 发送`/dog` 或者`/cat`的路径来连接到`localhost:80`的服务器上。
这样的话, nginx 反向代理就先在中间对路径进行分析,然后再通过事先设定好的网络服务器进行不同路径的不同路由处理。
如果服务器端返回了响应的话, 代理服务器就将其返回。看起来好像代理服务器自身处理了请求一样。(但其实不是的～)

### 目标

请求`localhost/cat`的时候, 显示`我喜欢喵星人`的网页。
请求`localhost/dog`的时候, 显示`我喜欢汪星人`的网页。
猫星人用的服务器和汪星人用的服务器是分开互不影响的。
客户端不知道服务器的存在以及番号。

### 项目构造

```
.
├── docker-compose.yml
├── cat-server
│   └── index.html
├── dog-server
│   └── index.html
└── reverse-proxy
    └── nginx.conf
```

---

### 项目代码

首先我们用 docker 来启动。使用`docker-compose up`的话就可以启动两个网络服务器和一个反向代理服务器了。
对每个 content 来说的话, 将所需要的文件作为 volume 挂载。
docker-compose.yml

```
version: '3'

services:
  dog-server:
    image: nginx
    container_name: 'dog-container'
    volumes:
      - ./dog-server:/usr/share/nginx/html
    ports:
      - 7000:80

  cat-server:
    image: nginx
    container_name: 'cat-container'
    volumes:
      - ./cat-server:/usr/share/nginx/html
    ports:
      - 7001:80

  reverse-proxy:
    image: nginx
    volumes:
      - ./reverse-proxy/nginx.conf:/etc/nginx/nginx.conf
    ports:
      - 80:80
```

---

### Index Page (喵星人网页和汪星人网页)

dog-server/index.html

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>汪星人的页面</title>
  </head>
  <body>
    <h1>汪星人的页面</h1>
  </body>
</html>
```

cat-server/index.html

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>喵星人的页面</title>
  </head>
  <body>
    <h1>喵星人的页面</h1>
  </body>
</html>
```

就是两个非常非常简单的网页。

---

### 反向代理用的设定文件

reverse-proxy/nginx.conf

```json
events {
    worker_connections  16;
}
http {
    server {
        listen 80;
        server_name localhost;
        location /dog {
            proxy_pass http://host.docker.internal:7000/;
            proxy_redirect off;
        }
        location /cat {
            proxy_pass http://host.docker.internal:7001/;
            proxy_redirect off;
        }
    }
}
```
