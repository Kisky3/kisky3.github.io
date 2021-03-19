---
layout: post
title: Git Pull Error【Please commit your changes or stash them before you merge】
date: 2021-01-08 15:06:03
tags:
- GitErrors
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
Git pull 时的错误【Please commit your changes or stash them before you merge】
<!--more-->
当`git pull origin master`当时候发生以下的错误：
```
$ git pull origin master
From https://github.com/user-name/app-name
     * branch　     master       ->   FETCH_HEAD
Updating e05c05f..050505
error: Your local changes to the following files would be overwritten by merge: 
         Gemfile.lock
         config/initializers/devise.rb
Please commit your changes or stash them before you merge.
Aborting
$ 
```

### 解决方法1: 删除文件
```
# ファイルのあるディレクトリへ移動
$ rm Gemfile.lock
$ git pull origin master
```
***

### 解决方法2: stash回避
```
# 1) コンフり起こしてるファイルを一時退避
$ git stash
# 2) 退避したコミットしていないものが表示されるのでpullする
$ git pull origin master
# 3) スタッシュを戻す
$ git stash pop
```

***
### 解决方法3: 强制push
```
# 1) リモートの最新を取ってきておいて・・
$ git fetch origin master

# 2) masterを、リモート追跡のmasterに強制的に合わせる
$ git reset --hard origin/master
```
