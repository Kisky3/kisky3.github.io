---
title: AWS Lambda Dynamodb Access Error AccessDeniedException
date: 2020-09-15 23:55:41
tags:
- AWS
- Lambda
clearReading: true
autoThumbnailImage: yes
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
AWS Lambda和Dynamodb连接时的错误 AccessDeniedException
<!--more-->
当想在lambda里通过特定条件读取Dynamodb里的数据时，发生了以下的错误
```
2017-04-03T12:11:12.144Z    ******  Unable to delete item. Error JSON:
{
    "message": "User: arn:aws:sts::******:assumed-role/******/****** is not authorized to perform: dynamodb:DeleteItem on resource: arn:aws:dynamodb:us-east-1:******:table/******",
    "code": "AccessDeniedException",
    "time": "2017-04-03T12:11:12.131Z",
    "requestId": "******",
    "statusCode": 400,
    "retryable": false,
    "retryDelay": 0
}
```
### 解决方法：
lambda的policy里、添加可以允许Dynamodb连接的权限。

首先确认lambda的policy：
<img src="./1.png" style="width: 500px">

***
然后添加policy
<img src="./2.png" style="width: 500px">

***
这时候ARN这个修饰名是需要的，可以从dynamodb的overview里看到。
<img src="./3.png" style="width: 500px">
