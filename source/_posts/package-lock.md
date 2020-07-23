---
title: About package-lock.json
date: 2020-04-20 23:26:09
tags:
- npm
- package-lock
clearReading: true
thumbnailImage: 20200420.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: System Setting
---
关于package-lock.json
<!--more-->
当你进行`npm install`的时候，会自动生成package.json和package-lock.json.
这两个到底有什么区别呢。

***
# 什么是 Semantic Versioning
Semantic Versioning就是版本的记录样式。比如像下面这样的形式的版本记录的话大家就很熟悉了。
```
"2.3.2"
```

这种写法就是Semantic Versioning了。
X.Y.Z

X: ‎Major => 这个版本号变化了表示有了一个不可以和上个版本兼容的大更改。
Y: Minor => 这个版本号变化了表示有了增加了新的功能，并且可以向后兼容。
Z: Patch => 这个版本号变化了表示修复了bug，并且可以向后兼容。

因为major version变化表示可能会影响之前版本的兼容性，所以无论是波浪符号还是插入符号都不会自动去修改major version，因为这可能导致程序crush，可能需要手动修改代码。
根据这样的Semantic Versioning写法我们就可以更好地了解到更新的版本情报了。

# 什么是(^)、（~）
当我们查看package.json中已安装的库的时候，会发现他们的版本号之前都会加一个符号，有的是插入符号（^），有的是波浪符号（~）。那么他们到底有什么区别呢？

例：
```

"dependencies": {
    "bluebird": "^3.3.4",
    "body-parser": "~1.15.2"
  }
```

也就是：
```
bluebird的版本号：^3.3.4
body-parse的版本号：~1.15.2
```


当我们使用最新的Node运行`npm instal --save xxx`，的时候，他会优先考虑使用插入符号（^）而不是波浪符号（~）了。
这两个符号有什么分别呢？然后我试着整理了一下：

### 波浪符号（~）：
他会更新到当前minor version（也就是中间的那位数字）中最新的版本。放到我们的例子中就是：body-parser:~1.15.2，这个库会去匹配更新到1.15.x的最新版本，如果出了一个新的版本为1.16.0，则不会自动升级。波浪符号是曾经npm安装时候的默认符号，现在已经变为了插入符号。

### 插入符号（^）：
这个符号就显得非常的灵活了，他将会把当前库的版本更新到当前major version（也就是第一位数字）中最新的版本。放到我们的例子中就是：bluebird:^3.3.4，这个库会去匹配3.x.x中最新的版本，但是他不会自动更新到4.0.0。

也就是说：
```
~1.15.2 :=  >=1.15.2 <1.16.0

^3.3.4 := >=3.3.4 <4.0.0
```

# package-lock.json的作用
其实用一句话来概括很简单，就是锁定安装时的包的版本号，并且需要上传到git，以保证其他人在npm install时大家的依赖能保证一致。

因为npm是开源世界，各库包的版本语义可能并不相同，有的库包开发者并不遵守严格这一原则：相同大版本号的同一个库包，其接口符合兼容要求。这时候用户就很头疼了：在完全相同的一个nodejs的代码库，在不同时间或者不同npm下载源之下，下到的各依赖库包版本可能有所不同，因此其依赖库包行为特征也不同有时候甚至完全不兼容。

原来package.json文件只能锁定大版本，也就是版本号的第一位，并不能锁定后面的小版本，你每次npm install都是拉取的该大版本下的最新的版本，为了稳定性考虑我们几乎是不敢随意升级依赖包的，这将导致多出来很多工作量，测试/适配等，所以package-lock.json文件出来了，当你每次安装一个依赖的时候就锁定在你安装的这个版本。

因此npm最新的版本就开始提供自动生成package-lock.json功能，为的是让开发者知道只要你保存了源文件，到一个新的机器上、或者新的下载源，只要按照这个package-lock.json所标示的具体版本下载依赖库包，就能确保所有库包与你上次安装的完全一样。

那如果我们安装时的包有bug，后面需要更新怎么办？

在以前可能就是直接改package.json里面的版本，然后再npm install了，但是5版本后就不支持这样做了，因为版本已经锁定在package-lock.json里了，所以我们只能npm install xxx@x.x.x  这样去更新我们的依赖，然后package-lock.json也能随之更新。

假如我已经安装了jquery 2.1.4这个版本，从git更新了package.json和package-lock.json，我npm install能覆盖掉node_modules里面的依赖吗?

其实我也有这个疑问，所以做了测试，在直接更新package.json和package-loc.json这两个文件后，npm install是可以直接覆盖掉原先的版本的，所以在协作开发时，这两个文件如果有更新，你的开发环境应该npm install一下才对。