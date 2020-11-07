---
title: Add a live2d Character To Your Hexo Blog
date: 2020-07-01 18:13:22
tags:
- hexo
- blog
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
给你的Hexo博客加一只会动的live2d看板娘
<!--more-->
实现效果:

先看一下实现效果，右下角的小可爱就是添加的live2d卡通人物，而且她还会眨眼睛，头会随着鼠标的移动而转动。
<img src="./1.png" style="width:600px">

#### 1. 安装hexo-helper-live2d
```
npm install --save hexo-helper-live2d
```
***
##### 2.安装live2d

其中<live2d-widget-model>替换成想要的，比如我安装的的是live2d-widget-model-hijiki(小黑猫)

当然，还有很多的model可供选择:
```
live2d-widget-model-chitose
live2d-widget-model-epsilon2_1
live2d-widget-model-gf
live2d-widget-model-haru/01 (use npm install --save live2d-widget-model-haru)
live2d-widget-model-haru/02 (use npm install --save live2d-widget-model-haru)
live2d-widget-model-haruto
live2d-widget-model-hibiki
live2d-widget-model-hijiki
live2d-widget-model-izumi
live2d-widget-model-koharu
live2d-widget-model-miku
live2d-widget-model-ni-j
live2d-widget-model-nico
live2d-widget-model-nietzsche
live2d-widget-model-nipsilon
live2d-widget-model-nito
live2d-widget-model-shizuku
live2d-widget-model-tororo
live2d-widget-model-tsumiki
live2d-widget-model-unitychan
live2d-widget-model-wanko
live2d-widget-model-z16
```

live2d所有模型展示图：
https://www.cnblogs.com/strengthen/p/11112215.html

```
// $ npm install <live2d-widget-model>
// 例: 安装live2d-widget-model-hijiki
$ npm install live2d-widget-model-hijiki
```
***

#### 3.配置
在Hexo站点配置文件_config.yml，或者主题配置文件_config.yml中添加如下配置
至于每个配置项的作用看名字就很清楚，也可以修改值然后部署看下效果.

```js
live2d:
  enable: true
  scriptFrom: local
  pluginRootPath: live2dw/
  pluginJsPath: lib/
  pluginModelPath: assets/
  tagMode: false
  log: false
  model:
    use: live2d-widget-model-hijiki
  display:
    position: right
    width: 150
    height: 300
  mobile:
    show: true
  react:
    opacity: 0.7
```
如果是在主题下的话有时可能无法生效。
下载完之后，在Hexo根目录中新建文件夹`live2d_models`，然后在node_modules文件夹中找到刚刚下载的live2d模型，将其复制到live2d_models中，然后编辑配置文件中的model.use项，将其修改为live2d_models文件夹中的模型文件夹名称。
```
model:
    use: live2d-widget-model-hijiki
```

一切就绪之后，用hexo server命令启动服务器，稍等一下就可以看到右下角出现了你设置的看板娘了。

***
#### deploy
预览完成以后没问题的话就可以发布了。
```
hexo d -g
```
