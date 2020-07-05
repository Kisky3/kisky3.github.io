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
```
To let the Serverless Framework access your AWS account, we're going to create an IAM User with Admin access, which can configure the services in your AWS account. This IAM User will have its own set of AWS Access Keys.
```
<img src="./1.png" style="width:600px;margin:40px 0">

输入适当的用户名，在「アクセス種類」的地方选择「プログラムによるアクセス」
<img src="./2.png" style="width:600px;margin:40px 0">

在权限设置的地方选择「既存のポリシーを直接アタッチ」,然后搜索选择你所需要的ポリシー.
AdministratorAccess是已存在的权限最强的ポリシー。
<img src="./3.png" style="width:600px;margin:40px 0">

你也可以自己制作你所需要的ポリシー,来限制你所能使用到的服务和权限范围。
比如说下面这个就是连接到S3的，并且只有读取权限的ポリシー
<img src="./4.png" style="width:600px;margin:40px 0">

当用户生成之后你会得到一对key.
点击表示并下载csv文档.

之后就可以利用制作好的IAM User来设置Serverless Framework了。
用下面的 `aws configure`命令行。
```
sls config credentials --provider aws --key XXXXXXXXXXXXEXAMPLE --secret XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXEXAMPLEKEY
Serverless: Setting up AWS...
Serverless: Saving your AWS profile in "~/.aws/credentials"...
Serverless: Success! Your AWS access keys were stored under the "default" profile.
```

如果已经有defualt的`aws configure`的时候，加上选项 --profile就好了。

### 生成Service并deploy
一行的命令行就行，这就是Serverless Framework的魅力。
首先生成新的service
```
$ sls create -t aws-python -p slstest
Serverless: Generating boilerplate...
Serverless: Generating boilerplate in "/your/folder/slstest"
 _______ __
| _ .-----.----.--.--.-----.----| .-----.-----.-----.
| |___| -__| _| | | -__| _| | -__|__ --|__ --|
|____ |_____|__| \___/|_____|__| |__|_____|_____|_____|
| | | The Serverless Application Framework
| | serverless.com, v1.7.0
 -------'

Serverless: Successfully generated boilerplate for template: "aws-python"
$ ls slstest/
handler.py serverless.yml
```
这是用python function的template.你还可以选择其他的语言.

其中包括：
- handler.py
lambda function的template,里面已经定义好了测试用的hello function.

- serverless.yml
用于记录Serverless Framework的设定
`sls create` 命令行的选项
```
$ sls create --help
Plugin: Create
create ........................ Create new Serverless service
--template / -t (required) ......... Template for the service. Available templates: "aws-nodejs", "aws-python", "aws-java-maven", "aws-java-gradle", "aws-scala-sbt", "aws-csharp", "openwhisk-nodejs" and "plugin"
--path / -p ........................ The path where the service should be created (e.g. --path my-service)
--name / -n ........................ Name for the service. Overwrites the default name of the created service.
```

这次选择的是python,所以用aws-python.
另外，如果是新建的script的话要指定PATH.
slstest文件夹就是用于指定PATH.

如果是已经存在的话，script的文件夹内 使用`sls create`就好.

详细设定可以之后再编辑`serverless.yml`,现在首先要确认能否跑起来,
那么先移动到slstest文件夹并deploy.
```
$ cd slstest/
$ sls deploy
Serverless: Creating Stack...
Serverless: Checking Stack create progress...
.....
Serverless: Stack create finished...
Serverless: Packaging service...
Serverless: Uploading CloudFormation file to S3...
Serverless: Uploading service .zip file to S3 (639 B)...
Serverless: Updating Stack...
Serverless: Checking Stack update progress...
..................
Serverless: Stack update finished...
Service Information
service: slstest
stage: dev
region: us-east-1
api keys:
None
endpoints:
None
functions:
slstest-dev-hello
```

这样就deploy好了,里面自动设定好了CloudFormation,IAM,Lambda等.

### 测试一下
deploy之后测试一下
```
$ sls invoke -f hello
{
"body": "{\"input\": {}, \"message\": \"Go Serverless v1.0! Your function executed successfully!\"}",
"statusCode": 200
}
```
测试也非常简单.`sls invoke`的命令行：
```
$ sls invoke --help
Plugin: Invoke
invoke ........................ Invoke a deployed function
invoke local .................. Invoke function locally
--function / -f (required) ......... The function name
--stage / -s ....................... Stage of the service
--region / -r ...................... Region of the service
--path / -p ........................ Path to JSON or YAML file holding input data
--type / -t ........................ Type of invocation
--log / -l ......................... Trigger logging data output
--data / -d ........................ input data
```

刚刚的测试是什么都没有传递，要传递数据的时候加上`-d`
```
$ sls invoke -f hello -d '{"key":"value"}'
{
"body": "{\"input\": {\"key\": \"value\"}, \"message\": \"Go Serverless v1.0! Your function executed successfully!\"}",
"statusCode": 200
}
```

### 编辑serveless.yml
region之类的设定都在生成的serveless.yml里进行编辑/
```
# you can overwrite defaults here
# stage: dev
region: ap-northeast-1  # 在这里编辑
```

保存设定并且再实行一次，顺便使用`-v`选项进行动作的确认
```
$ sls deploy -v
Serverless: Creating Stack...
Serverless: Checking Stack create progress...
CloudFormation - CREATE_IN_PROGRESS - AWS::CloudFormation::Stack - slstest-dev
CloudFormation - CREATE_IN_PROGRESS - AWS::S3::Bucket - ServerlessDeploymentBucket
CloudFormation - CREATE_IN_PROGRESS - AWS::S3::Bucket - ServerlessDeploymentBucket
CloudFormation - CREATE_COMPLETE - AWS::S3::Bucket - ServerlessDeploymentBucket
CloudFormation - CREATE_COMPLETE - AWS::CloudFormation::Stack - slstest-dev
Serverless: Stack create finished...
Serverless: Packaging service...
Serverless: Uploading CloudFormation file to S3...
Serverless: Uploading service .zip file to S3 (639 B)...
Serverless: Updating Stack...
Serverless: Checking Stack update progress...
CloudFormation - CREATE_COMPLETE - AWS::S3::Bucket - ServerlessDeploymentBucket
CloudFormation - CREATE_COMPLETE - AWS::CloudFormation::Stack - slstest-dev
CloudFormation - UPDATE_IN_PROGRESS - AWS::CloudFormation::Stack - slstest-dev
CloudFormation - CREATE_IN_PROGRESS - AWS::IAM::Role - IamRoleLambdaExecution
CloudFormation - CREATE_IN_PROGRESS - AWS::Logs::LogGroup - HelloLogGroup
CloudFormation - CREATE_IN_PROGRESS - AWS::Logs::LogGroup - HelloLogGroup
CloudFormation - CREATE_IN_PROGRESS - AWS::IAM::Role - IamRoleLambdaExecution
CloudFormation - CREATE_COMPLETE - AWS::Logs::LogGroup - HelloLogGroup
CloudFormation - CREATE_COMPLETE - AWS::IAM::Role - IamRoleLambdaExecution
CloudFormation - CREATE_IN_PROGRESS - AWS::IAM::Policy - IamPolicyLambdaExecution
CloudFormation - CREATE_IN_PROGRESS - AWS::IAM::Policy - IamPolicyLambdaExecution
CloudFormation - CREATE_COMPLETE - AWS::IAM::Policy - IamPolicyLambdaExecution
CloudFormation - CREATE_IN_PROGRESS - AWS::Lambda::Function - HelloLambdaFunction
CloudFormation - CREATE_IN_PROGRESS - AWS::Lambda::Function - HelloLambdaFunction
CloudFormation - CREATE_COMPLETE - AWS::Lambda::Function - HelloLambdaFunction
CloudFormation - CREATE_IN_PROGRESS - AWS::Lambda::Version - HelloLambdaVersionnjv8umhHX8AljD0VKQUqojKkzU2huW1AkaSample
CloudFormation - CREATE_IN_PROGRESS - AWS::Lambda::Version - HelloLambdaVersionnjv8umhHX8AljD0VKQUqojKkzU2huW1AkaSample
CloudFormation - CREATE_COMPLETE - AWS::Lambda::Version - HelloLambdaVersionnjv8umhHX8AljD0VKQUqojKkzU2huW1AkaSample
CloudFormation - UPDATE_COMPLETE_CLEANUP_IN_PROGRESS - AWS::CloudFormation::Stack - slstest-dev
CloudFormation - UPDATE_COMPLETE - AWS::CloudFormation::Stack - slstest-dev
Serverless: Stack update finished...
Service Information
service: slstest
stage: dev
region: ap-northeast-1
api keys:
None
endpoints:
None
functions:
slstest-dev-hello

Stack Outputs
HelloLambdaFunctionQualifiedArn: arn:aws:lambda:ap-northeast-1:000000000000:function:slstest-dev-hello:1
ServerlessDeploymentBucketName: slstest-dev-serverlessdeploymentbucket-1s9cy3sample
```
`region: ap-northeast-1`是东京，可以看到这个已经被deploy了