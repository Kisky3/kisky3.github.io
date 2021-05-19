---
title: CentOS6 Error:All mirror URLs are not using ftp, http[s] or file.
date: 2021-02-23 01:20:34
tags:
- CentOS6
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Back-end Knowledge
---
CentOS6 Error: All mirror URLs are not using ftp, http[s] or file.
<!--more-->
2020-11-30开始CentOS 6已经不支持了！

如果`yum install`和`yum update`的时候出现以下的错误的话要考虑看是不是CentOS6版本不支持了的缘故.

error:
```css
Loaded plugins: fastestmirror, ovl
Setting up Install Process
Error: Cannot retrieve repository metadata (repomd.xml) for repository: base. Please verify its path and try again
YumRepo Error: All mirror URLs are not using ftp, http[s] or file.
Eg. Invalid release/repo/arch combination/
removing mirrorlist with no valid mirrors: /var/cache/yum/x86_64/6/base/mirrorlist.txt
```
自己是因为docker的关系一直用着旧的image和container,所以一直没有发觉. 一旦清空重新`docker compose up`的时候就因为CentOS6的关系当掉了.

解决方法是搜索最新所支持的包的url并替换掉...
或者将docker file里的CentOS版本更新.
