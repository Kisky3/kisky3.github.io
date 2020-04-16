---
title: Use ServerlessFramework To Create REST API
date: 2020-02-10 21:03:52
tags:
- serverlessframework
- aws
- restAPI
clearReading: true
thumbnailImage: 20200210.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Back-end Knowledge
---
使用serverless框架创建REST API
<!--more-->
### 安装环境
- node.js (npm) v4以上
- awsアカウント
- awscli

使用下面的command确认环境
```
$ node -v
v4.3.2
$ aws --version
aws-cli/1.11.44 Python/2.7.10 Darwin/16.4.0 botocore/1.5.7
```

***
### 安装Serverless Framework
首先从npm开始安装
```
$ npm install -g serverless
```

安装完成后进行版本确认
```
serverless -v
Framework Core: 1.63.0
Plugin: 3.3.0
SDK: 2.3.0
Components Core: 1.1.2
Components CLI: 1.4.0
```

### 关于Serveless Application的构成
Serveless Application主要分为**有各种方法(你所想运行的代码块)的文件**和**云端(AWS)设定文件**
***
### 设置AWS Credentials
首先要设置能使用Serveless Framework的IAM
