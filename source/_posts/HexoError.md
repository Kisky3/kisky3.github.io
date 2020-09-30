---
title: 【Hexo ERROR】fatal in unpopulated submodule '.deploy_git'
date: 2020-06-01 00:34:01
tags:
- hexo
- blog
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
【Hexo异常】fatal: in unpopulated submodule '.deploy_git'
<!--more-->
今天又重新搞了下hexo，好久不动它居然报了错。
这种情况可以先安装下相关的依赖：
```
npm install hexo-deployer-git –save
```

实在不行，就把它删掉，然后重新生成和部署。
```
rm -rf .deploy_git
hexo g
hexo d
```

OK DONE!
---
