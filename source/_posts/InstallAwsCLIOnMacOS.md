---
layout: post
title: Install AWS CLI On macOS
date: 2020-02-08 20:54:37
tags:
- awsCLI
- install
clearReading: true
thumbnailImage: 20200208.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Back-end Knowledge
---
在 macOS 上安装 AWS CLI 版本 1
<!--more-->
1.先决条件
Python 2 版本 2.7+ 或 Python 3 版本 3.4+

```
$ python --version
```
***
2.使用捆绑安装程序安装 AWS CLI 版本 1
如果没有 unzip，请使用您喜欢的程序包管理器或等效工具进行安装。
```
curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
unzip awscli-bundle.zip
sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
```
***
3.运行安装程序。该命令将 AWS CLI 安装到 /usr/local/aws，
并在 /usr/local/bin 目录中创建符号链接 aws。使用 -b 选项创建符号链接将免除在用户的 $PATH 变量中指定安装目录的需要。这应该能让所有用户在任何目录下通过键入 aws 来调用 AWS CLI。
```
sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
```
***
4.查看是否安装成功
```
aws --version
```
***

参考
[AWS官方文档](https://docs.aws.amazon.com/zh_cn/cli/latest/userguide/install-macos.html#awscli-install-osx-pip)