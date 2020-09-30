---
title: Connecting to GitHub with SSH
date: 2019-12-21 17:40:36
tags:
- Git
- SSH
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
利用ssh连接Github
<!--more-->
虽然已经用ssh连接Github好几次了，但是一直以来对公钥和密钥的加密过程不清不楚，所以在这里总结以下

### 什么是公钥和私钥
公钥（Public Key）与私钥（Private Key）是通过一种算法得到的一个密钥对。
公钥是密钥对中公开的部分。

公钥加密法的步骤如下：

1. 首先接受方生成一对密钥，即私钥和公钥。
2. 然后接收方将公钥法送给发送方。
3. 发送方用收到的公钥对数据加密,再发送给接收方。
4. 接收方收到数据后，使用自己的私钥解密。

由于再非对称算法中，公钥加密的数据必须使用对应的私钥才能解密，而私钥又只有接受方自己知道，
这样就保证了数据传输的安全性。

***

### 公钥和密钥的生成
连接Github的时候，需要再本地生成公钥和密钥，密钥自己保存而公钥需要发送给Github。

首先移动到保存钥的文件夹

```
$cd ~/.ssh
```
第一次生成钥匙的时候这个文件夹里应该是空的啥都没有。
不管有没有先生成钥,也可以自行加上option，但是一般情况下这样就可以了

```
$ssh-keygen -t rsa
```

一般来说不用在意问题，连续三次按下回车键就可以看到生成了id_rsa(私钥)和id_rsa.pub(公钥)公钥对。
如果你已经生成过私钥(id_rsa)的话在这里会被覆盖。

```
$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/(username)/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```

<img src="./1.png" style="width: 500px">

<br>
当你想修改钥名的时候，在第一个问题里输入你想要生成的钥名。比如在这里就指定了id_git_rsa,
然后持续按下回车键就可以生成id_git_rsa和id_git_rsa.pub两个公钥对了。

```
$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/(username)/.ssh/id_rsa):id_git_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```
***

### 将公钥上传到Github
点击下面的网页进行公钥的设置：（前提是你已经拥有了自己的Github账号）
https://github.com/settings/ssh

在画面上点击「Add SSH key」,然后在title的地方输入公钥名（任意），「key」里输入你生成公钥的内容。

<img src="./2.png" style="width: 500px">

利用下面的命令行进行公钥内容的copy
```
$ pbcopy < ~/.ssh/id_rsa.pub (Mac)
$ clip < ~/.ssh/id_rsa.pub (Windows)
```
* 钥名为你自己生成的钥名，默认为id_rsa.pub

***

### 确认连接
```
$ ssh -T git@github.com
```
如果返回以下的文字则说明已经连接成功了。
```
Hi (account名)! You've successfully authenticated, but GitHub does not provide shell access.
```

但是注意如果生成钥的时候你指定了自己的钥名，有可能会连接不成功。
这是因为ssh连接的时候自动默认参照的是「~/.ssh/id_rsa」、「~/.ssh/id_dsa」、「~/.ssh/identity」的钥名文件。
你需要自己在~/.ssh/config中进行定义。

```
Host github github.com
  HostName github.com
  IdentityFile ~/.ssh/id_git_rsa # 在这里写下自己设置的私匙文件名
  User git
```

然后再连接一次应该就能连接上了。

```
ssh -T github
```
***
### 与GitHub的互动
实际上在与Github互动的时候,比如pull或push的时候还需要确认用户名和密码，那就说明SSH并没有很好地运作。
在~/.gitconfig文件里进行设定后就可以解决了。
```
[url "github:"]
    InsteadOf = https://github.com/
    InsteadOf = git@github.com:
```
