---
title: Set Repository-specific Username/Email
date: 2020-03-08 23:48:44
tags:
- Git
- GitConfig
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
给特定的Git Repojitory设定用户名和邮箱
<!--more-->
我们知道config是配置的意思，那么git config命令就是对git进行一些配置。
git里面一共有3个配置文件

### 1.仓库级配置文件：
#### 方法1：找到该文件，直接打开：

该文件位于当前仓库下，路径.git/，文件名为config
这个配置中的设置只对当前所在仓库（H:\MyGit目录下的test仓库）有效

#### 方法2：

通过命令查看项目配置（仓库级配置）：git config --local -l
***
### 2.全局级配置文件：
#### 方法1：
以win10个人的PC机为例，在用户目录下，其路径为：C:\Users\Administrator，文件名为 .gitconfig

#### 方法2:
通过命令查看全局级配置：git config --global -l

***
### 3.系统级配置文件：
#### 方法1：

本地git的安装目录下，以我的git安装路径为例：F:\software\Git\mingw64\etc，文件名为：gitconfig

#### 方法2：
通过命令查看系统配置：git config --system -l


***
对于git来说，配置文件的权重是仓库>全局>系统

### 二、 用git config命令查看配置文件
命令参数 –list, 简写 -l
格式：git config [–local|–global|–system] -l
查看仓库级的config，命令：git config –local -l
查看全局级的config，命令：git config –global -l
查看系统级的config，命令：git config –system -l
查看当前生效的配置，命令：git config -l，这个时候会显示最终三个配置文件计算后的配置信息

***
### 三、 使用git config命令编辑配置文件
编辑的英文单词是什么，没错，edit

命令参数 –edit, 简写 -e

格式：git config [–local|–global|–system] -e

编辑仓库级的config，命令：git config –local -e，与–list参数不同的是，git config -e默认是编辑仓库级的配置文件。

编辑全局级的config，命令：git config –global -e

编辑系统级的config，命令：git config –system -e

注：执行这个命令的时候，git会用配置文件中设定的编辑器打开配置文件。

例子：
```
$ git config user.name "hogehoge"
$ git config user.email hoge@gmail.com

$ git config --global user.name "hogehoge"
$ git config --global user.email hoge@gmail.com

$ git config --local user.name "hogehoge"
$ git config --local user.email hoge@gmail.com
```