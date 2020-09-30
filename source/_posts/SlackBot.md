---
title: Easy Slack bot from Google Apps Script
date: 2019-12-25 23:50:23
tags:
- setting
- slack bot
- slack
- Google Apps Script
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: System Setting
---
如何利用GAS做一个简单的机器人
<!--more-->

如何利用GAS(Google Apps Script)来做一个可以定时提醒的bot呢.
首先功能是这样，每天早上（除了周末和节假日）的8:59分和下午17:59的时候，会自动发消息圈到当天应该做值日的人，并提醒他做值日。
<img src="./1.png" style="width: 400px">
***

### 创建频道

在slack上创建一个用于发送和管理消息的频道。名字和说明任意即可。
如果要往已存在的频道发送消息则可以忽略这一步。
<img src="./2.png" style="width: 400px">

***

### 制作Bot
点击https://api.slack.com/start/overview,　然后在跳转页面点击「Create a Slack App」

输入你的bot名字和workplace,然后点击「Create App」。
<img src="./3.png" style="width: 400px">
***
### 设置Incoming Webhooks

在bot设定的左侧介面点击features>Incoming Webhooks、将「Activate Incoming Webhooks」 设置为On。
然后在页面下部会出现「Add New Webhook to WorkSpace 」的选项，点击并许可。
<img src="./4.png" style="width: 400px">
<img src="./5.png" style="width: 400px">

在页面的下方会显示「Webhook URL」，记得要将它复制下来。
之后GAS里的Script代码需要用它来进行连接并取得你在GAS里创建的数据。

***
### 制作保存数据的GAS
这里需要三列，一是用于显示的值日生名字，一个是对应值日日期，第三列是slack上该用户的对应用户名（这里建议用用户ID，因为当用户名中间有空格时，有时不能很好地圈到slack用户）
<img src="./6.png" style="width: 400px">

***
### 制作GAS的script
从菜单点击「ツール」「スクリプトエディタ」，然后在里面输入你的script代码。
<img src="./7.png" style="width: 400px">
<img src="./8.png" style="width: 400px">

这次使用的代码如下：

```JS
// set timer 08:59
function setTrigger08() {
  var triggerDay = new Date();
  triggerDay.setHours(08);
  triggerDay.setMinutes(59);
  ScriptApp.newTrigger("main").timeBased().at(triggerDay).create();
}

// set timer 17:59
function setTrigger17() {
  var triggerDay = new Date();
  triggerDay.setHours(17);
  triggerDay.setMinutes(59);
  ScriptApp.newTrigger("main").timeBased().at(triggerDay).create();
}

// delete Trigger
function deleteTrigger() {
  var triggers = ScriptApp.getProjectTriggers();
  for(var i=0; i < triggers.length; i++) {
    if (triggers[i].getHandlerFunction() == "main") {
      ScriptApp.deleteTrigger(triggers[i]);
    }
  }
}

// main method
function main() {
  deleteTrigger();
  core_function();
  Logger.log('main');
}

function core_function() {
 
    // Get script sheet URL
    var spreadSheet = SpreadsheetApp.openByUrl('https://docs.google.com/spreadsheets/xxxxxx');
 
    // Get the first sheet
    var sheet = spreadSheet.getSheets()[0];
 
    // Get Rows
    var lastrow = sheet.getLastRow();
 
    // Get current date
    var currentDay = new Date();
   
   // To determine if a current date is a workday or not
　　function isWeekDay(currentDate) {
  　　var weekday = currentDate.getDay();
  　　if (weekday == 0 || weekday == 6) {
    　　return false;
  　　}
  　　return true;
　　}
   
   // To determine if a current date is a holiday or not
　　function isEventsForDay(currentDate) {
  　　var calendar = CalendarApp.getCalendarById('ja.japanese#holiday@group.v.calendar.google.com');
  　　if (calendar.getEventsForDay(currentDate, {max: 1}).length > 0) {
    　　return true;
  　　}
  　　return false;
　　}
 
    // Get name ,date and metion name array
    var name_array = sheet.getSheetValues(2, 1, lastrow, 1);
    var date_array = sheet.getSheetValues(2, 2, lastrow, 1);
    var mention_array = sheet.getSheetValues(2, 3, lastrow, 1);
 
    // Set date format for today
    var today = Utilities.formatDate(new Date(), "Asia/Tokyo", "yyyy/MM/dd");
    var date = new Array();
 
    // Roop to get date_number
    var date_num = date_array.length;
    for(var i = 0; i < date_num - 1; i++) {
 
        // Set date format for date
        date.push(Utilities.formatDate(new Date(date_array[i]), "Asia/Tokyo", "yyyy/MM/dd"));
 
        // Get index if today = date
        var num = date.indexOf(today);
    }
 
    // Get name through index we got
    var member = name_array[num];
    var mention = mention_array[num];
 
    // if it is workday and not holiday then set message
    var message;
 
    if(member && isWeekDay(currentDay) && !isEventsForDay(currentDay)) {
 
        message = 'Hey〜！:sunny:\n 今日の当番は　*' + member + '* です！ <@' + mention + '>: \n 本日「加湿器ON/OFF」、「終礼」、「ゴミ捨て」をあなたに任せるよ ！！ \n ※都合が悪い時にみんなに言ってくださいね';
       
       // Send message to slack
       Logger.log(message);
       postSlack(message)
    } 
 
  function postSlack(message) {
    // Webhook URL you copied when you created a app
    var url = "https://hooks.slack.com/xxxxxxxx";
    var options = {
      "method" : "POST",
      "headers": {"Content-type": "application/json"},
      "payload" : '{"text":"' + message + '"}'
    };
     // Run here
     UrlFetchApp.fetch(url, options);
  }
}
```
***
### 将GAS　Script设置为公开Application
从菜单的「公開」点击「ウェブアプリケーションとして導入」
<img src="./9.png" style="width: 400px">

<img src="./10.png" style="width: 400px">

点击更新按钮后Web　Application的URL就会被更新，然后同样将它copy下来。
***
### 设置SlackApi的Outgoing Webhooks
连接下面的URL
```
https://(your workplace name).slack.com/apps/manage
```

OutgoingWebhooks为关键词搜索，并点击出现的Popup,跳转之后点击设定。
<img src="./11.png" style="width: 400px">

在Outgoing Webhooks的Integration Settings里设定你的频道名，并将刚刚复制的GAS的URL粘贴并保存。
<img src="./12.png" style="width: 400px">

***
### Slack里发送消息的设定
在GAS的编辑页面，点击菜单「編集」「現在のプロジェクトのトリガー」。

在GAS管理页面打开后点击右下的「トリガーを追加」。
<img src="./15.png" style="width: 400px">
<img src="./14.png" style="width: 400px">

完成！

***
### 踩坑
##### 1.GAS只能在一个小时内进行执行，如何尽量准确地指定时间呢。可以在scirpt里写入下面的代码：
然后トリガー的执行时间设定选择为08~09点，这样就可以在08:59的准确时间进行Slack的消息发送了。

```JS
// set timer 08:59
function setTrigger08() {
  var triggerDay = new Date();
  triggerDay.setHours(08);
  triggerDay.setMinutes(59);
  ScriptApp.newTrigger("main").timeBased().at(triggerDay).create();
}
```

参考：
[Google Apps Scriptの日毎のトリガーで時間をもっと細かく設定する](https://qiita.com/sumi-engraphia/items/465dd027e17f44da4d6a)

***

##### 2.在Slack想圈出用户时，如果用户名中间有空格则会失效
这时不使用<@username>而是使用<@userID>，就可以解决这个问题了。
类似这样的
<img src="./16.png" style="width: 400px">

参考：
[Slack APIでユーザー宛のメンションができなくなったので対策した](https://qiita.com/tomoeine/items/ee7bae955efec9db1a96)
