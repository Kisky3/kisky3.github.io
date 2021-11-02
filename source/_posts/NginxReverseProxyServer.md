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

```json
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

```js
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

---

### Nginx 设置文件的写法

1.events
Nginx 的设定文件是由叫做`context`(也就是由花括号{}包围起来的块)组成的。
最初的 context 一般都是`events`。引用一下官方文档。

> Syntax: events { ... }
> Default: —
> Context: main
> Provides the configuration file context in >which the directives that affect connection >processing are specified.

也就是关于全体连接(connection)的设定相关的 context。
`worker_connections`操作系统启动多少个工作进程运行 Nginx
注意是工作进程，不是有多少个 nginx 工程。(在 Nginx 运行的时候，会启动两种进程，一种是主进程 master process;一种是工作进程 worker process。)

2.http
`http`context 是作为虚拟服务器的设定。

> Syntax: server { ... }
> Default: —
> Context: http
> Sets configuration for a virtual server. （略）

---

### 接收 HTTP 请求后的流程

首先如果 HTTP 的请求被发送过来之后, 先查找在`http`context 里的`server`context 里是否有符合条件的。`server`里有`listen`和`server_name`两个选项描述。这就是筛选的条件。

1.首先、先查找`listen`里指定的 IP 地址和番号是否和请求一致。

2.如果一致的情况下、`server_name`和 HTTP 请求头里的 Host 是一样的话,那么就执行 server context 里写好的处理。

这次我们用的是下面这样的简单的 server context.

```js
server {
        listen 80;
        server_name localhost;
        # 处理...
    }
```

在这里`listen`用来判断请求是不是番号 80 来的, server_name 是来判断是否和请求里的 Host 和 localhost 一致。
如果能对上其中一个那么就执行之后写好的处理。

我们来让反向代理运行吧。解析 HTTP 的 URL 的路径,然后路由到别的 Web 服务器上。

server context 里的处理

```
  location /dog {
    proxy_pass http://host.docker.internal:7000/;
    proxy_redirect off;
  }
  location /cat {
    proxy_pass http://host.docker.internal:7001/;
    proxy_redirect off;
  }
```

也就是在 location context 里寻找一致的路径。 /dog 的路径的话 就自动路由到`proxy_pass`里写好的 URL 里。

如果路径再长一点的话, 比如`localhost/dog/list`,就会路由到`http://host.docker.internal:7000/list`上。

### proxy_pass 的注意点

当 proxy_pass 是 URL 和不是 URL 的场合, 路由的路径是不一样的！

### Docker Container 看来的 Host 番号

`http://host.docker.internal`是指从 Docker Container 看来的番号。从 Container 内连接 Host 的番号`7000`的时候这样写就可以解决命名的问题,从而裂解上 Host 的番号。
Docker Container 内如果写了 localhost 的话指的不是 Docker 启动的 host,二手 Container 自身。当然如果不使用 Docker 而是直接启动 Nginx 的话可以直接置换 localhost。

也就是说当`localhost/dog`的时候, 向 Docker 启动的 host 的番号为 7000 的服务器发送请求。(/cat 的情况下也是一样, 也就是 7001)

### 结果

让我们用`docker-compose up`来启动服务器, 并向`localhost/dog`发送请求,然后验证一下返回的响应。

```
curl --verbose localhost/dog
```

HTTP 请求头

```
> GET /dog HTTP/1.1
> Host: localhost
> User-Agent: curl/7.54.0
> Accept: */*
```

HTTP 响应头

```
< HTTP/1.1 200 OK
< Server: nginx/1.17.0
< Date: Mon, 01 Jul 2019 13:38:14 GMT
< Content-Type: text/html
< Content-Length: 264
< Connection: keep-alive
< Last-Modified: Sun, 30 Jun 2019 13:43:59 GMT
< ETag: "5d18bc9f-108"
< Accept-Ranges: bytes
```

HTTP 响应体

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1"/>
    <title>汪星人的页面</title>
  </head>
  <body>
    <h1>汪星人的页面</h1>
  </body>
* Connection #0 to host localhost left intact
</html>%
```

各个 container

```
reverse-proxy_1  | 172.31.0.1 - - [01/Jul/2019:13:38:14 +0000] "GET /dog HTTP/1.1" 200 264 "-" "curl/7.54.0"
dog-container    | 172.31.0.1 - - [01/Jul/2019:13:38:14 +0000] "GET / HTTP/1.0" 200 264 "-" "curl/7.54.0" "-"
```

大功告成！
