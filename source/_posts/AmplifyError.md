---
title: AWS Amplify Memo
date: 2021-03-24 23:44:34
tags:
- AWS
- Ampliy
clearReading: true
autoThumbnailImage: yes
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Back-end Knowledge
---
AWS Amplify常见的坑
<!--more-->
最近用Amplify踩了一些坑。在这里总结一下错误。

### 1. GraphQLError: Request failed with status code 401
Graphql的API返回401的时候很可能是你设置的API Key过期了。
你最初通过命令行默认设置的API的API Key期限是七天。

AWS AppSync -> 設定 -> 在默认认证模式下添加API Key,然后在aws-exports.js 里更行aws_appsync_apiKey的値。

最长可以设置一年的期限。
其实最好的是设置一个可用的IAM用户,但是我还没有摸索出来。(汗)

***

### 2. amplify codegen
这个命令用于你修改了schema之后更新`/arc/graphql/`目录下的GraphQL。
Graphql文件夹里面的内容会根据schema进行自动的typr配型和生成。
```
amplify add codegen --apiId {appsync_graphql_id}
amplify codegen
```
***

### 3. Validation error of type FieldUndefined
这个情况是出现在先通过scheme生成了src/graphql的query之类的API之后,运行`amplify codegen`进行更行graphql而可能出现的错误。

这是因为你的AppSync API需要使用DataStore category。
所以解决步骤:
```
amplify update api
// 选择GraphQL
// 选择Enable DataStore for entire API
amplify push
```
参考:
https://github.com/aws-amplify/amplify-cli/issues/5339
> Here's the important part of the error: Field 'syncTodos' in type 'Query' is undefined.

> I believe this is because you don't have conflict detection on your AppSync API, which needs to be enabled to using the DataStore category. It should be disabled if you are using the API category. To enable it, run amplify update api, choose "GraphQL", then choose "Enable DataStore for entire API", then run amplify push

> I'm leaving this issue open as an improvement opportunity, so that we can look at making the error message more helpful, as this issue has been raised several times now.

***

#### 4. 什么是AppSync
AppSync就是AWS提供的一个能够灵活地使用GraphQL API的一个管理类服务。
也就是相当于是用AWS API Gateway来提供一个常见的REST API。

AppSync可以直接修改DynamoDB的值,并进行获取/更新/删除等常见操作。
而一般来说API Gateway一般中间要搭配和Lambda进行使用。但是AppSync的话就可以不使用Lambda来进行DynamoDB的直接连接。

Graphql的query类型一般有3种。query:read, mutation:create / update / delete, subscription: get date realtime

#### 5. amplify pull --appId xxxxxxx --envName dev
这个命令行用于在deploy之后，别的测试环境里可以取得同样的后端代码。
前端部署的话首先从Github里clone代码,然后使用这个命令,并进行问答式对话来获取相同AWS region Amplify 的后端部署。(连接数据库之类的)

```
amplify pull --appId xxxxxxx --envName dev
```
***