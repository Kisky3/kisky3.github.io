---
title: Git Push Error 403
date: 2020-06-05 00:47:21
tags:
- git
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
---
Git Push时返回403的异常
<!--more-->
## 错误内容
```
$ git push origin master
remote: Permission to アカウント1/リポジトリ名.git denied to アカウント2.
fatal: unable to access 'https://アカウント1＠github.com/アカウント1/リポジトリ名.git' : The requested URL returned error: 403
```

## 解决方法
```
$ git remote set-url origin https://アカウント1＠github.com/アカウント1/リポジトリ名.git
```

确认远程URL是否已经改变
```
$ git remote -v
origin: https://アカウント1＠github.com/アカウント1/リポジトリ名.git (fetch)
origin: https://アカウント1＠github.com/アカウント1/リポジトリ名.git (push)
```

再次Push
```
$ git push origin master
  ~一部省略~
To https://github.com/アカウント1/リポジトリ名.git
 * [new branch]      master -> master
 ```
