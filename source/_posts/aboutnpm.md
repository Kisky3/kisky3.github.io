---
title: Npm And Yarn
date: 2019-11-24 17:01:22
tags:
- Npm
- Yarn
clearReading: true
thumbnailImage: 20191124.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: System Setting
---
Npm和Yarn
<!--more-->
### npm概念
Npm就是Node Package Manager，也就是Node包管理工具。

***
### npm用途
如果不使用包管理器，有什么麻烦的呢？

这是源于代码包分享的理念，包和依赖越来越多，每个包都有自己的版本和发展，而包与包之间也有依赖。版本管理就成了一件令人头痛的事。

比如jQuery插件A(版本1)，依赖于jQuery(版本1)，当你把jQuery版本更新为2的时候，A很可能就挂掉了。各个插件间也有依赖，插件A挂掉了，可能B也挂掉了，
连接挂掉是噩梦一般，你要一个个去查文档，看每个包要求依赖的版本，然后自己协调。
这时候npm就应运而生了，不仅可以帮你把各种包从网上download下来，最主要的是还可以帮你管理不同包之间的关系，
比如你当前的版本号是多少，你依赖着谁，你依赖的版本号是多少都一目了然。

***

### npm实现具体步骤
NPM 的思路大概是这样的：

1. 买个服务器作为代码仓库（registry），在里面放所有需要被共享的代码

2. 发邮件通知 jQuery、Bootstrap、Underscore 作者使用 npm publish 把代码提交到 registry 上，分别取名 jquery、bootstrap 和 underscore（注意大小写）

3. 社区里的其他人如果想使用这些代码，就把 jquery、bootstrap 和 underscore 写到 package.json 里，然后运行 npm install ，npm 就会帮他们下载代码

4. 下载完的代码出现在 node_modules 目录里，可以随意使用了。

这些可以被使用的代码被叫做「包」（package），这就是 NPM 名字的由来：Node Package(包) Manager(管理器)。

***

### npm的发展
那么 npm 是怎么火的呢？

npm 的发展是跟 Node.js 的发展相辅相成的。

Node.js 是由一个在德国工作的美国程序员 Ryan Dahl 写的。他写了 Node.js，但是 Node.js 缺少一个包管理器，于是他和 npm 的作者一拍即合、抱团取暖，最终 Node.js 内置了 npm。

后来的事情大家都知道，Node.js 火了。
随着 Node.js 的火爆，大家开始用 npm 来共享 JS 代码了，于是 jQuery 作者也将 jQuery 发布到 npm 了。

所以现在，你可以使用 npm install jquery 来下载 jQuery 代码。

现在用 npm 来分享代码已经成了前端的标配。

***
### npm使用方法
进入新建的工作目录，然后进行npm的初始化。
```
npm init -t
```
<img src="./1.png" style="width:500px ">

在项目目录下生成了一个**package.json**文件。
然后安装jQuery包。然后npm就会帮你看有没有jQuery这个包。如果存在的话就帮你安装到根目录下**node_modules**这个文件夹下。
```
npm i jQuery
```
<img src="./2.png" style="width:500px ">

并且你安装的包的版本信息还会被自动记录到**package.json**的**dependencies**里面。是一对一的。即使之后你把node_modules文件夹全部删除了，
在你重新初始化的时候，它会读取dependencies里的信息，然后再重新生成对应的node_modules文件夹。
<img src="./3.png" style="width:500px ">

***
### npm常用配置
**package.json**的**script**
在script里会执行你指定的命令。
比如写一个yo，输出yo的指令，然后**npm run yo**,就会看到命令行输出了yo
<img src="./4.png" style="width:500px ">

区别生产环境和开发环境用下面的命令
```
npm i webpack --save-dev
```
安装成功之后可以看到package.json里多了一个devDependencies。也就是用于生产环境下的依赖。
<img src="./5.png" style="width:500px ">
