---
title: Create a PR Bot
date: 2020-05-10 00:26:11
tags:
- setting
- slack bot
- slack
- PRBot
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: System Setting
---
制造一个github的PR BOT
<!--more-->
简单来说就是利用Github 的Github Hook，选择在PR生成并指定Reviwer的时候进行事件呼出。
而利用GAS的api生成功能接受Hook发来的信息，再往slack的频道里发送消息。

Github webhooks：
Github/Setting/Webhooks/Add Webhook
选择类型为json，url为gas生成的app的url
<img src="./1.png" style="width:500px;margin:40px 0">

GAS：
deploy as a web app
然后选择Anyone,even anonymous can access to the app.
<img src="./2.png" style="width:500px;margin:40px 0">

GAS的内容：
```js
var CHANNEL = "YOUR_CHSNNEL_NAME";

function doPost(e) {
  try {
    recievePayload(e.postData.getDataAsString());
  } catch (ex) {
    notifyToSlack(CHANNEL, ex);
  }
}

function recievePayload(json) {
  var payload = JSON.parse(json);

  if (payload.action === "opened") {
    notifyToSlack(CHANNEL, review_request(payload))
  }

  if (payload.action === "submitted") {
    notifyToSlack(CHANNEL, approve(payload))
  }
}

function approve (payload) {
  var message = "";
  var reviwers = "";
  payload.pull_request.requested_reviewers.forEach( i => {
    reviwers += i.login + "さん、"
  })

  // PR Approvedされた時：
    if (payload.review.state === "approved") {
      message =  convert_user(payload.pull_request.user.login) + "さん、\n下記のPR :lgtm2: をもらいました！ \n"
      + "問題なければマージしてね \n\n"
      + "--------------------------------------------------\n "
      + "■PR TITLE: \n"
      + payload.pull_request.title + "\n\n"
      + "■PR URL: \n"
      + "\n" + payload.pull_request.html_url + "\n\n"
      + "--------------------------------------------------\n "
    } else {
      return;
    }

  return message;

}

function review_request(payload) {
  var message = "";
  var reviwers = "";

  payload.pull_request.requested_reviewers.forEach( i => {
    reviwers += convert_user(i.login) + "さん  "
  })

  // PR新規作成された時：
    if (payload.pull_request.state === "open" && reviwers !== "") {
      message = reviwers + payload.pull_request.user.login + "からのPR依頼がきました！ \n"
      + "手が空いてる時に、下記のPRのレビューをお願い致します〜 \n\n"
      + "--------------------------------------------------\n "
      + "■PR TITLE: \n"
      + payload.pull_request.title + "\n\n"
      + "■PR URL: \n"
      + "\n" + payload.pull_request.html_url + "\n\n"
      + "--------------------------------------------------\n "
    } else {
      return
      //message = payload.pull_request.user.login + "下記のPRにReviewerを指定してください \n" + payload.pull_request.title + "\n" + payload.pull_request.html_url;
    }

  return message;

}

function convert_user(name) {
  switch(name) {
      case "<github-user-name>":
        return "<slack-user-id>"
        break;
      default:
        break;
  }
}

function notifyToSlack(channel, message) {
  var prop = PropertiesService.getScriptProperties().getProperties();
  var slackApp = SlackApp.create('xoxp-xxxxxxxxxxxxxx');
  slackApp.chatPostMessage(channel, message, {　
    username: "PR_娘",
    icon_emoji: ":musume:"
  });
}
```