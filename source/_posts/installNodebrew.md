---
layout: post
title: Install nodebrew
date: 2019-12-08 00:24:44
tags:
- node.js
- nodebrew
clearReading: true
thumbnailImage: 20191208.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: System Setting
---
安装nodebrew
<!--more-->
### 什么是nodebrew
nodebrew是Node.js版本管理工具。
可以安装指定版本的Node.js，并进行版本间切换。

***
### nodebrew安装
1.如果安装来Homebrew则可以用homebrew来安装。
首先安装Homebrew。
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

确认Homebrew安装成功。
```
brew help
```

2.安装nodebrew
```
$ brew install nodebrew
Updating Homebrew...
==> Auto-updated Homebrew!
Updated 1 tap (homebrew/core).
==> Updated Formulae
pulledpork

==> Downloading https://github.com/hokaccha/nodebrew/archive/v1.0.1.tar.gz
==> Downloading from https://codeload.github.com/hokaccha/nodebrew/tar.gz/v1.0.1
######################################################################## 100.0%
==> Caveats
You need to manually run setup_dirs to create directories required by nodebrew:
  /usr/local/opt/nodebrew/bin/nodebrew setup_dirs

Add path:
  export PATH=$HOME/.nodebrew/current/bin:$PATH

To use Homebrew's directories rather than ~/.nodebrew add to your profile:
  export NODEBREW_ROOT=/usr/local/var/nodebrew

Bash completion has been installed to:
  /usr/local/etc/bash_completion.d

zsh completions have been installed to:
  /usr/local/share/zsh/site-functions
==> Summary
  /usr/local/Cellar/nodebrew/1.0.1: 8 files, 38.6KB, built in 5 seconds
$
```

3.设置环境path
```
$ echo 'export PATH=$HOME/.nodebrew/current/bin:$PATH' >> ~/.bash_profile
$ source ~/.bash_profile
```

4.确认nodebrew是否安装成功
```
$ nodebrew -v
nodebrew 1.0.1

Usage:
    nodebrew help                         Show this message
    nodebrew install <version>            Download and install <version> (from binary)
    nodebrew compile <version>            Download and install <version> (from source)
    nodebrew install-binary <version>     Alias of `install` (For backword compatibility)
    nodebrew uninstall <version>          Uninstall <version>
    nodebrew use <version>                Use <version>
    nodebrew list                         List installed versions
    nodebrew ls                           Alias for `list`
    nodebrew ls-remote                    List remote versions
    nodebrew ls-all                       List remote and installed versions
    nodebrew alias <key> <value>          Set alias
    nodebrew unalias <key>                Remove alias
    nodebrew clean <version> | all        Remove source file
    nodebrew selfupdate                   Update nodebrew
    nodebrew migrate-package <version>    Install global NPM packages contained in <version> to current version
    nodebrew exec <version> -- <command>  Execute <command> using specified <version>

Example:
    # install
    nodebrew install v8.9.4

    # use a specific version number
    nodebrew use v8.9.4
$
```
***
### 安装Node.js
接下来可以用nodebrew来安装Node.js了。
1.首先确认可以安装的版本号。
```
$ nodebrew ls-remote
v0.0.1    v0.0.2    v0.0.3    v0.0.4    v0.0.5    v0.0.6    

v0.1.0    v0.1.1    v0.1.2    v0.1.3    v0.1.4    v0.1.5    v0.1.6    v0.1.7
v0.1.8    v0.1.9    v0.1.10   v0.1.11   v0.1.12   v0.1.13   v0.1.14   v0.1.15
v0.1.16   v0.1.17   v0.1.18   v0.1.19   v0.1.20   v0.1.21   v0.1.22   v0.1.23
v0.1.24   v0.1.25   v0.1.26   v0.1.27   v0.1.28   v0.1.29   v0.1.30   v0.1.31
v0.1.32   v0.1.33   v0.1.90   v0.1.91   v0.1.92   v0.1.93   v0.1.94   v0.1.95
v0.1.96   v0.1.97   v0.1.98   v0.1.99   v0.1.100  v0.1.101  v0.1.102  v0.1.103
v0.1.104  

︙
︙

v11.0.0   v11.1.0   v11.2.0   v11.3.0   v11.4.0   v11.5.0   v11.6.0   v11.7.0
v11.8.0   v11.9.0   v11.10.0  v11.10.1  v11.11.0  v11.12.0  

io@v1.0.0 io@v1.0.1 io@v1.0.2 io@v1.0.3 io@v1.0.4 io@v1.1.0 io@v1.2.0 io@v1.3.0
io@v1.4.1 io@v1.4.2 io@v1.4.3 io@v1.5.0 io@v1.5.1 io@v1.6.0 io@v1.6.1 io@v1.6.2
io@v1.6.3 io@v1.6.4 io@v1.7.1 io@v1.8.1 io@v1.8.2 io@v1.8.3 io@v1.8.4 

io@v2.0.0 io@v2.0.1 io@v2.0.2 io@v2.1.0 io@v2.2.0 io@v2.2.1 io@v2.3.0 io@v2.3.1
io@v2.3.2 io@v2.3.3 io@v2.3.4 io@v2.4.0 io@v2.5.0 

io@v3.0.0 io@v3.1.0 io@v3.2.0 io@v3.3.0 io@v3.3.1 
$
```

2.指定版本号的安装
```
$ nodebrew install-binary {version} //指定版本号
$ nodebrew install-binary latest    //安装最新版本
```

如果出现错误的话就新建以下路径
错误：
```
$ nodebrew install-binary latest
Fetching: https://nodejs.org/dist/v11.12.0/node-v11.12.0-darwin-x64.tar.gz
Warning: Failed to create the file 
Warning: /Users/fukatsu/.nodebrew/src/v11.12.0/node-v11.12.0-darwin-x64.tar.gz:
Warning:  No such file or directory
                                                                           0.0%
curl: (23) Failed writing body (0 != 1057)
download failed: https://nodejs.org/dist/v11.12.0/node-v11.12.0-darwin-x64.tar.gz
```

解决方法：
```
$ mkdir -p ~/.nodebrew/src
$ nodebrew install-binary latest
Fetching: https://nodejs.org/dist/v11.12.0/node-v11.12.0-darwin-x64.tar.gz
######################################################################## 100.0%
Installed successfully
```

3.确认安装的Node.js版本
```
$ nodebrew ls
v11.12.0

current: none
```
4.使用安装好的Node.js版本,
```
$ nodebrew use v11.12.0
use v11.12.0
```
5.再次确认
```
$ nodebrew ls
v11.12.0

current: v11.12.0
```

或者用Node.js版本确认命令
```
$ node -v
v11.12.0
```

确认完毕！ 安装成功！
