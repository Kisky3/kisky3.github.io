---
title: Git Error 「fatal cannot lock ref ...」
date: 2020-08-05 00:47:21
tags:
- git
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
---
Git Error: 「fatal: cannot lock ref ...」
<!--more-->
### Error详情：
在Git新建branch的时候出现了error,如果在别人创建的`project/feature_name`下面创建了`project/feature_name/add_hoge`branch的话会出现下面的error:

```
$ git branch
master
* project/feature_name

$ git checkout -b project/feature_name/add_hoge
fatal: cannot lock ref 'project/feature_name/add_hoge':
'project/feature_name' exists; cannot create 'project/feature_name/add_hoge'
```
***
### Error原因：
如果在Git上试图创建`foo/bar`这样的分支的话,就会在`.git`路径的下面创建一个`foo`的路径。
而如果别人已经创建过`foo`分支的话说明`foo`路径已经存在,就会导致刚刚这样的错误。

```
$ git co -b hoge
Switched to a new branch 'hoge'
$ git co -b hoge/fuga
fatal: cannot lock ref 'refs/heads/hoge/fuga': 'refs/heads/hoge' exists; cannot create 'refs/heads/hoge/fuga'
```
***
### 解决方法：
改变根分支的分支名:
```
NG: project/feature_name
OK: project/feature_name/top
```

不使用`/`:
```
例） project/feature_name_add_hoge
例） project/feature_name--add_hoge
例） project/feature_name__add_hoge
```

或者干脆换一个不冲突的分支名就好了。
