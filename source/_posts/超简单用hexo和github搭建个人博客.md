---
title: So Easy! Let's Start Your Blog With Hexo And Github From Zero
date: 2019-05-28 20:30:05
tags:
- hexo
- blog
- github
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---

超简单! 教你从零用Hexo和Github搭建个人博客
<!--more-->

### 前言
Hexo是高效的静态站点生成框架，它基于Node.js搭建博客，并可以托管于github服务器上。
之后便可以用markdown语法进行你的博客记录了。
生成上传命令行简便快捷，值得推荐！

### 准备工作
#### 1. 下载node.js

 [点击下载安装Node.js](https://nodejs.org/ja/download/)

 无特殊要求可以一路默认点击Next直到安装完成。

***

#### 2. 安装Git

[点击下载Git](https://git-scm.com/download/win)

以上两步完成后可以在命令行输入以下命令来确认node.js和git安装是否成功

```
node -v
npm -v
git --version
```

***

#### 3. 在github上新建项目

<img src="./1.png" style="width:600px">

输入你的Github用户名+.github.io，例子：Kisky3.github.io

注意勾选下面的生成README选项

<img src="./2.png" style="width:600px">

在新项目的setting里，添加生成可视化page

<img src="./3.png" style="width:600px">

***

#### 4. 安装Hexo
新建文件夹用来存放博客文章。比如MyBlog

然后在该文件夹下执行一下命令行安装Hexo
 ```
$ npm install hexo-cli -g  
$ hexo init 
$ npm install  
$ hexo server  
 ```

 当启动hexo server后，打开 http://localhost:4000 就可以看到生成的默认页面了！

 <img src="./5.png" style="width:600px">

#### 5.推送至Github

在博客文件夹MyBlog下的_config.yml配置文件的url换成你的项目主URL¥，否则后续图片的显示会出问题
```HTML
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://kisky3.github.io/
```

之后在deploy里修改type为git,并且写入你在Github生成的项目地址
```HTML
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: https://github.com/Kisky3/Kisky3.github.io
  branch: master
```

并在MyBlog文件夹下运行以下命令行

```HTML
npm install hexo-deployer-git –save //下载hexo-deployer-git，否则deploy会出现error
hexo g // 生成本地静态文件
hexo d // 将本地文件deploy到Github上
```

此时访问项目的主页http://你的Github名.github.io，就可以看到初始页面了

***

#### 6. 更新博文
并在MyBlog文件夹下运行以下命令行来写博文

```
hexo new 你的博文题目 // 生成博文
```

用markdown语法完成博客记录后

```
hexo g // 生成本地静态文件
hexo d // 将本地文件deploy到Github上
```

#### 7. 更换主题
默认主题太没个性了，可以在网上下载自己喜欢的主题

例：
<img src="./4.png" style="width:600px">

- [下载地址](https://github.com/LouisBarranqueiro/hexo-theme-tranquilpeak)
- [参考文档](https://github.com/LouisBarranqueiro/hexo-theme-tranquilpeak/blob/master/DOCUMENTATION.md)

#### 8. 部分翻译
##### 主题安装
1. 下载最新版本
2. 重命名下载文件夹为 tranquilpeak,并将其放置于你MyBlog/theme文件夹下
3. 修改_config.yml文件夹的theme为tranquilpeak
4. 执行hexo clean删除public文件夹、并再次执行hexo generate重新生成。

##### 博文内配置解释
例子
```HTML
---
title: 超简单! 用Hexo和Github搭建个人博客
date: 2019-05-28 20:30:05
tags:
- hexo
- blog
- github
clearReading: true
thumbnailImage: 20190528.jpg
thumbnailImagePosition: left
coverImage: 20190528.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
这里是文章的概览，显示在主页缩略内容上面
<!-- more -->

这里是自动生成的文章目录
<!-- toc -->

# 这是大标题

## 这是二级标题

## 这里有个本地图片
<!-- 图片需要放置于你生成博文名字的文件夹里面 -->
<img src="./1.png">


```
博文内常用配置设定说明:
```
・ tags:
   定义该文章的标签，定义之后可以在分类里面查看自动建立的索引

・ thumbnailImage:
   首页的文章标题旁边图片

・ thumbnailImagePosition:
   首页的文章图片位置

・ coverImage:
   文章打开时顶部的封面图片

・ <!-- more -->
   这个标志之前的内容将会自动生成首页的概览,如果不写thumbnailImagePosition的设置将不起作用

・ <!-- toc -->
   这个标志的位置将会自动生成文章目录
```

***
#### 修改博客的样式
当你想修改该主题自带的样式的时候，你需要进行下面的操作:
在hexo根目录下启动博客预览模式
```
hexo s
```

同时在themes/tranquilpeak/下运行
```
npm install
npm run start
```
修改样式以后如果满意就可以, run prod来deploy.
```
npm run prod
```

***
##### 文章置顶
修改themes/tranquilpeak/layout/_partial/index.ejs

 ```HTML
                <h1 class="postShorten-title">
                    <% if (post.link) { %>
                    <a class="link-unstyled" href="<%- url_for(post.link) %>">
                        <%= post.title || '(' + __('post.no_title') + ')' %>
                    </a>
                    <% } else { %>
                        <% if (post.top) { %>
                            <i class="fa fa-thumbtack" style="color:gray;font-size:20px"></i>
                            <font style="color:gray;font-size:20px">pinned</font>
                            <span class="post-meta-divider">|</span>
                            <% } %>
                    <a class="link-unstyled" href="<%- url_for(post.path) %>">
                        <%= post.title || '(' + __('post.no_title') + ')' %>
                    </a>
                    <% } %>
                </h1>
```
在文章Front-matter中添加top值，数值越大文章越靠前，如：
```HTML
---
title: Hexo 
date: 2019-05-28 21:49:33
tags:
- Hexo
categories: Front-end Knowledge
top: true
---
```

***
##### 添加访客数和访问量
lauout/_footer.scss给页脚添加访客数和访问量。
```HTML
<footer id="footer" class="main-content-wrap">
        <script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
        <span id="busuanzi_value_site_pv"></span><span id="busuanzi_container_site_pv">Pageviews</span>
        <span class="post-meta-divider">|</span>
        <span id="busuanzi_value_site_uv"></span><span id="busuanzi_container_site_uv">Visitors</span>
        <br>
    <span class="copyrights">
        Copyrights &copy; <%= date(new Date(), 'YYYY') + ' ' + config.author || config.title %>. All Rights Reserved.
    </span>
</footer>
```
注意:

这里的github推送地址和当前Hexo项目地址是分开的，也就是说，github.io的地址上面是没有hexo源码的，只有生成的静态页面。

所以最好将源文件夹做一个备份，以防更换机子或者文件丢失时无法维护博客。
