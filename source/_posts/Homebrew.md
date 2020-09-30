---
title: Install homebrew
date: 2019-09-29 23:03:54
tags:
- tip
- homebrew
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: System Setting
---
安装homebrew
<!--more-->


### 1. AppStore下载Xcode

### 2. global install
```
xcode-select --install
```

### 3.安装Homebrew
```
"$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### 4. 确认
```
brew doctor
```

没有安装 yarn。create-react-app 需要你事先安装好了 yarn，如果你没有安装，那就需要去下载 安装即可。如果你没安装 yarn，会自动降级为 npm.
Mac 安装 yarn 的方式很简单: brew install yarn 即可。
