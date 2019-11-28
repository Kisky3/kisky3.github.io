---
title: Docker Basic and Commands
date: 2019-11-07 00:10:07
tags:
- Docker
- Command
clearReading: true
thumbnailImage: 20191107.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Back-end Knowledge
---
docker的基础知识以及常用命令行
<!--more-->
### 1.Docker 简介
Docker 两个主要部件：

- Docker: 开源的容器虚拟化平台
- Docker Hub: 用于分享、管理 Docker 容器的 Docker SaaS 平台 -- [Docker Hub](https://link.jianshu.com/?t=https://registry.hub.docker.com/search?q=library)

*** 
### 2.Docker内部
要理解 Docker 内部构建，需要理解以下三种部件：

- Docker 镜像 - Docker images
- Docker 仓库 - Docker registeries
- Docker 容器 - Docker containers

***

###### Docker 镜像
Docker 镜像是 Docker 容器运行时的只读模板。每一个镜像由一系列的层 (layers) 组成。
如果我们想要在本地运行容器，就必须保证本地存在对应的镜像。所以，第一步，我们需要下载镜像。当我们尝试下载镜像时，Docker 会尝试先从默认的镜像仓库（默认使用 Docker Hub 公共仓库）去下载，当然了，用户也可以自定义配置想要下载的镜像仓库。

***
###### Docker 仓库
Docker 仓库用来保存镜像，可以理解为代码控制中的代码仓库。同样的，Docker 仓库也有公有和私有的概念。公有的 Docker 仓库名字是 Docker Hub。Docker Hub 提供了庞大的镜像集合供使用。这些镜像可以是自己创建，或者在别人的镜像基础上创建。Docker 仓库是 Docker 的分发部分。
***
###### Docker 容器
Docker 容器和文件夹很类似，一个Docker容器包含了所有的某个应用运行所需要的环境。每一个 Docker 容器都是从 Docker 镜像创建的。Docker 容器可以运行、开始、停止、移动和删除。每一个 Docker 容器都是独立和安全的应用平台，Docker 容器是 Docker 的运行部分。
***
###### Docker File 
当获得docker镜像时，还不能就这样使用，大多数情况下我们要根据自身所需要的条件（比如版本号啊，端口号啊之类的）来将docker镜像设置成我们所需要的开发环境。
实际上，docker file一般写成下面这样。
```
FROM centos:7               # ①
RUN yum install -y java     # ②
ADD files/apache-tomcat-9.0.6.tar.gz /opt/  # ③
CMD [ "/opt/apache-tomcat-9.0.6/bin/catalina.sh", "run" ]  # ④
```
① FROM是指定你所需要的docker镜像（作为最基层）。比如这里就是将centos:7的docker镜像为基础来执行之后的指令。
在执行时哪怕没有事先通过docker pull获取镜像，它也能自动获取相应的镜像供你使用。

② RUN是指在OS命令行执行时使用。在这里是指下载java，并加上了-y 的选项默认确认安装

③ ADD是指将 tar.gz文件的容器copy到你的指定的路径内，并同时解压展开tar文件。
写成这样：「ADD <copy源文件> <复制到Docker镜像内・并展开
这里就是在与docker file同阶层的地方生成一个「file」的路径，并将tomcat放置其中

④ CMD是指容器启动时所执行的命令行

我们现在利用做好的Dockerfile来生成Docker镜像。移动到Docker所在的路径，然后使用生成Docker镜像的命令：build
```
# cd <Dockerfile所在路径>
# docker build -t tomcat:1 .
　(docker build -t <Docker镜像名> <Dockerfile所在路径>)
# docker images
REPOSITORY   TAG   IMAGE ID       CREATED         SIZE
tomcat       1     10af894cf09a   1 minutes ago   456MB
```

通过这样我们就生成了自己的Docker镜像。然后我们使用Docker镜像来启动docker容器。
```
# docker run -it -d --name tomcat-1 -p 8081:8080 tomcat:1
```
然后打开「http://<IP地址>:8081/」就能看到启动成功的画面了。

可以使用下面的命令来查看log
```
# docker logs -f tomcat-1
```
***
###### Docker Compose
就是对于有好几个容器构成的服务来说，可以使用Docker Compose来控制多个Docker镜像的生成（build）和多个容器的启动和停止

上面几个概念的整体关系如图所示：
<img src="./1.png">

***
### Docker基础用法
[Docker Hub](https://link.jianshu.com/?t=https://registry.hub.docker.com/search?q=library): Docker镜像首页，包括官方镜像和其它公开镜像

镜像搜索
```
sudo docker search ubuntu # 搜索ubuntu官方镜像
```

获取镜像
```
$ sudo docker pull ubuntu # 获取ubuntu官方镜像
$ sudo docker images # 查看当前镜像列表
```

删除镜像
```
docker rmi IMAGE [IMAGE...]
```

运行容器
・docker run - 运行一个容器
・-t - 分配一个（伪）tty (link is external)
・-i - 交互模式 (so we can interact with it)
・ubuntu:14.04 - 使用 ubuntu 基础镜像 14.04
・/bin/bash - 运行命令 bash shell

```
sudo docker run -i -t ubuntu:14.04 /bin/bash
```

查看当前所有容器
```
$ sudo docker ps # 查看当前运行的容器, ps -a 列出当前系统所有的容器
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
6c9129e9df10        ubuntu:14.04        /bin/bash           6 minutes ago       Up 6 minutes                            cranky_babbage
```

进入指定容器（运行中）
```
docker attach CONTAINER
docker exec -it CONTAINER /bin/bash

```

删除所有容器
```
docker rm $(docker ps -aq)
```
***
##### Docker-compose
启动所有的容器
```
docker-compose up -d
```

进入启动中的某个容器内
```
docker-compose + exec ( or run )  + サービス名 + 実行したいコマンド
```

停止全部容器或某个容器的运行
```
docker-compose stop
docker-compose stop nginx
```
