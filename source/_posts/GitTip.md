---
title: GIT TIPS
date: 2018-12-06 18:28:52
tags:
- Git
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverImage: cover.jpg
coverSize: partial
comments: false
categories: Front-end Knowledge
top: true
---

About common git tips. Continuously update here.

<!--more-->
{% alert success no-icon %}
#### 1. git pull时 「Error：The following untracked working tree files would be overwritten by merge:」
{% endalert %}
输入以下代码后再进行修改，然后再push就好 （本地做的修改会不见 修改多的时候不建议！)

```
git fetch origin
git reset --hard FETCH_HEAD
```
***
<br>
{% alert success no-icon %}
#### 2. git本地版本回退与远端版本回退
{% endalert %}

##### 本地回滚
1.在Github上或者下面的命令行查看想回退版本的版本号。
 
 ```
 git reflog
 ```

2.接着回退版本
```
git reset --hard *****(版本号)
```

##### 远程回滚
###### 方法一：
如果错误提交已经推送到自己的远程分支了，那么就需要回滚远程分支了。 以下方法只能在自己一人的branch下使用。
强制回滚会消除别人提交的修改。慎用！

1. 首先回退本地分支步骤参见本地回滚

2. 强制推送到远程分支

```
git push -f *****(你的分支名)
```

###### 方法二：
git revert 命令意思是撤销某次提交。
它会产生一个新的提交，虽然代码回退了，但是版本依然是向前的，所以，当你用revert回退之后，所有人pull之后，他们的代码也自动的回退了。
如果使用 revert 撤销的不是最近一次提交，那么一定会有代码冲突，需要你合并代码，合并代码只需要把当前的代码全部去掉，保留之前版本的代码就可以了.
撤销最近一次提交

```
git revert HEAD
```

撤销上上次提交
```
git revert HEAD～1
```

撤销这次提交
```
git revert *****(版本号)
```
***
<br>
{% alert success no-icon %}
#### 3. git 将当前branch1的一部分抽出merge 剩余部分在另一个branch2上开发
{% endalert %}

1. 切换回master
2. 在master上创建一个新的branch2作为继续开发的branch
3. 在branch2上merge branch1
4. 将branch1 返回到想merge的范围，然后强制push回滚 之后merge
5. 在branch2 上进行后续开发

***
<br>
{% alert success no-icon %}
### 4. Git 撤销修改
{% endalert %}
1.本地修改了一堆文件(并没有使用git add到暂存区)，想放弃修改。
```
git checkout -- filename
```

2.撤销所有文件/文件夹的修改
```
git checkout .
```

3.本地新增了一堆文件(并没有git add到暂存区)，想放弃修改。
【单个文件】
```
rm filename / rm dir -rf
```

【所有文件/文件夹：】
```
git clean -xdf
```

4.本地修改/新增了一堆文件，已经git add到暂存区，想放弃修改。
【单个文件/文件夹：】
```
git reset HEAD filename
```

【所有文件/文件夹：】
```
git reset HEAD .
```

5.本地通过git add & git commit 之后，想要撤销此次commit
```
git reset commit_id
```
***
<br>
{% alert success no-icon %}
#### 5. git 将当前的branch的commit移动到另一个新的branch
{% endalert %}

git cherry-pick可以理解为”挑拣”提交，它会获取某一个分支的单笔提交，并作为一个新的提交引入到你当前分支上。 当我们需要在本地合入其他分支的提交时，如果我们不想对整个分支进行合并，而是只想将某一次提交合入到本地当前分支上，那么就要使用git cherry-pick了。

查看你需要的commit，然后切换到master上建立新分支。

###### 一个commit的情况下
```
git cherry-pick 版本号
```

###### 复数commit的情况下
```
git cherry-pick [起点版本号]..[终点版本号]
```

###### 终止cherry-pick
```
git cherry-pick --abort
```

如果发生conflict则需要解决冲突并commit。复数的情况下利用下面的comment查看状态并继续cheery-pick
```
git status
git cherry-pick --continue
```

最后成功后git push到新分支便可以获得所需commit（不是新分支的情况下也适用。）

***
<br>
{% alert success no-icon %}
#### 6. git删除分支
{% endalert %}

###### 删除已经push的远程分支
```
git branch -r -d origin/branch-name
git push origin :branch-name
```

但是本地查看branch还是能看到删除的分支。利用下面的命令行可删除远程仓库不存在的分支。

```
git remote prune origin
```

###### 删除本地分支
```
git branch -d
```
***
<br>
{% alert success no-icon %}
#### 7. 出现错误 fatal: remote origin already exists
{% endalert %}

当要把本地文件夹上传到git时要执行下面的代码：

```
git remote add origin 〜
```

但是有时会出现fatal: remote origin already exists.的错误信息，
此时需要使用git remote rm origin删除origin，然后再次上传即可

```
git remote rm origin
git remote add origin git@github.com:user_name/repository_name.git
git push -u origin master
```
***
<br>
{% alert success no-icon %}
#### 8.修改git的commit的用户名
{% endalert %}
commit的时候Author和Committer的本地Policy是被保存在.git/config里的。

查看设定
```
git config --list
```

显示个别项目
```
git config user.name
git config user.email
```

修改commit时的用户名和邮箱
```
git config user.name "hogehoge"
git config user.email hoge@fuga.com
```