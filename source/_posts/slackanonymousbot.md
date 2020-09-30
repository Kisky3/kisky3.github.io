---
title: Create a Anonymous Bot for Slack
date: 2020-02-28 23:50:23
tags:
- setting
- slack bot
- slack
- AnonymousBot
- BotKit
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: System Setting
---
做一个可以在slack匿名发言的机器人
<!--more-->
运行环境
- Ubuntu16.04
- node.js

完成后的示意图
<img src="./1.png" style="width:400px;margin:40px 0">

### 添加Bot
首先点击下面的link在slack里添加bot

https://my.slack.com/services/new/bot

这里用了`anonymous`的bot名字，也可以输入其他的，然后点击「Add bot integration」按钮。

<img src="./2.png" style="width:500px;margin:40px 0">

然后记下画面上显示的 「API Token」

### 获取频道ID
https://slack.com/api/conversations.list?token=刚刚记录下的API_Token

还可以取得用户ID和非公开频道ID
***
### 安装Botkit
安装并使用Slack官方文档上的Bot框架[Botkit](https://github.com/howdyai/botkit)

```
//创建路径
$ cd ~
$ mkdir slack_anonymous_bot 
$ cd slack_anonymous_bot

//botkit clone
$ git clone https://github.com/howdyai/botkit.git
$ cd botkit

//branch 移动
$ git checkout origin/legacy 

//安装
$ npm install

//如果安装失败・・・
$ npm audit fix
$ npm install
```

如果生成了~/botkit/example/slack_bot.js就说明成功了

***

### 编码
```
// 移动到你生成的路径
$ cd ~
$ cd slack_anonymous_bot/botkit/

//制作anonymous_bot.js
$ touch anonymous_bot.js
```

在anonymous_bot.js里粘贴下面的代码

```
var Botkit = require("./lib/Botkit.js"); // 注意path
var os = require("os");

var controller = Botkit.slackbot({
    debug: true,
});

var bot = controller.spawn({
    token: "刚刚记录下的API_TOKEN"
}).startRTM();

controller.on("direct_message", (bot, message) => {
    var now = new Date(); //获取时间
    var user_name = "名無しさん: "+ now.getFullYear()+"/"+(now.getMonth()+1)+"/"+now.getDate()+"/ "+now.getHours()+":"+now.getMinutes()+":"+now.getSeconds();

    bot.reply(message, "匿名で投稿しました．");

    bot.startConversation({  channel : "刚刚取得的频道ID" }, (err, convo) => {
        var send_message = {
          type: "message",
          channel: "刚刚取得的频道ID",
          text: message.text,
          username: user_name,
          thread_ts: null,
          reply_broadcast: null,
          parse: null,
          link_names: null,
          attachments: null,
          unfurl_links: null,
          unfurl_media: null,
          icon_url: null,
          icon_emoji: ":robot_face:",
          as_user: true
        }
        convo.say(send_message);
    });
});
```
***
### 运行
```
//debug环境
$ cd ~/slack_anonymous_bot/botkit
$ node anonymous_bot.js

//通常一般环境
$ cd ~/slack_anonymous_bot/botkit
$ forever start slack_bot.js
$ forever stop slack_bot.js // 停止bot
```
***
### 运行结果
如果bot运行成功了，bot前的灯会变绿色。
<img src="./4.png" style="width:300px;margin:40px 0">

投稿时候首先会给你回信
<img src="./5.png" style="width:300px;margin:40px 0">

然后你就可以看到匿名bot在你登陆的频道里代表你发言了
<img src="./6.png" style="width:300px;margin:40px 0">