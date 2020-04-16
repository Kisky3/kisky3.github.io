---
layout: post
title: Create a Git Commit Template
date: 2019-12-21 23:27:00
tags:
- Git
- Template
clearReading: true
thumbnailImage: 20191221.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
创建Git Commit的Template
<!--more-->
我们可以在commit的时候进行commit内容的统一。

### 生成commit message template
commit message template 本身可以指定路径并且让Git读取，可以在任意路径进行。

今天我们在my-cheatsheet的project的根目录进行配置。
并且设定只针对这个project有效。

```
# 任意的文件名都可以
$ vi .gitmessage
```

然后可以自行设置commit内容，包括颜文字
```
[branchname]
# ==== Emojis ====
# ✏️  :pencil2: Writing docs
# 🐛  :bug: Fix bugs
# 👍  :+1: Feature improvements
# ✨  :sparkles: Additional partial feature
# 🎉  :tada: Grand major features added to celebrate
# ♻️  :recycle :Refactoring
# 🚿  :shower: Removal of obsolete functions or features.
# 💚  :green_heart: Modification and improvement of testing and CI
# 👕  :shirt: Fixing of errors found by Lint.
# 🚀  :rocket: Performance improvement
# 🆙  :up: Updates, such as dependent package
# 🔒  :lock: Limit of the range of new features
# 👮  :cop: Security-related improvements
# ==== Format ====
# :emoji: Subject #issue No.
#
# Commit body...
# ==== The Seven Rules ====
# 1. Separate subject from body with a blank line
# 2. Limit the subject line to 50 characters
# 3. Capitalize the subject line
# 4. Do not end the subject line with a period
# 5. Use the imperative mood in the subject line
# 6. Wrap the body at 72 characters
# 7. Use the body to explain what and why vs. how
```
***
### 注册你的commit message template
在git config里注册你生成的Template。

```
$ git config commit.template .gitmessage
```

如果你想电脑里存在所有Git管理的项目都使用这个Template的话。要加上global option。
这个例子没有加option，说明只是针对当前的项目使用这个Template。

***
### 使用Template
设置完成后在你commit的时候Template就会自动显示了，在commit的时候输入相应的emoji的编码就可以显示了。
```
:bug: correct a bug
```


```
$ git commit
```

```
[branchname]
# ==== Emojis ====
# ✏️  :pencil2: Writing docs
# 🐛  :bug: Fix bugs
# 👍  :+1: Feature improvements
//　省略
```

以上！！
