---
title: Create a Auto Mail Bot
date: 2020-05-05 00:25:40
tags:
- setting
- slack
- Google Apps Script
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: System Setting
---
做一个GAS自动送信功能的bot
<!--more-->
<img src="./1.png" style="width: 400px">
<img src="./2.png" style="width: 400px">
<img src="./3.png" style="width: 400px">

```js
function mailSend() {
  /* 名前を取得 */
  var activeSpreadsheet = SpreadsheetApp.getActiveSpreadsheet();
  var getName = activeSpreadsheet.getRange("D2").getValue().trim();
  var masterName = getName.replace(/　/g, ""); //スペース削除

  /* SD日報設定ファイルから情報を取得 */
  var settingUrl = "YOUR_SHEET";
  var settingSpreadsheet= SpreadsheetApp.openByUrl(settingUrl);
  var settingSheet = settingSpreadsheet.getSheetByName("mail");
  var lastRow = settingSheet.getLastRow() + 1;

  for(var i = 1;i < lastRow;i++){
    var setName = settingSheet.getRange(i,1).getValue();
    if(setName == masterName){
      var strCc = settingSheet.getRange(i,4).getValue(); //宛先
      var strFrom = settingSheet.getRange(i,2).getValue(); //差出アドレス
    }
  }

  /* メールデータの準備 */
  var strUrl = activeSpreadsheet.getUrl();
  var strGid = activeSpreadsheet.getSheetId();
  var strDate = Utilities.formatDate(new Date(),"JST","MM/dd");
  var strSubject = "【業務日報】" + strDate + " " + masterName;
  var strBody = strUrl + "#gid=" + strGid;

  /* メールを送信 */
  GmailApp.sendEmail(
    "",
    strSubject,
    strBody,
    {
      cc:strCc,
      from:strFrom,
      name: masterName
    }
  );
}

```

还有一种是可以将表格的内容直接发送
```js
function mailSend() {
  var ui = SpreadsheetApp.getUi();
  /* 名前を取得 */
  var activeSpreadsheet = SpreadsheetApp.getActiveSpreadsheet();
  var getName = activeSpreadsheet.getRange("C2").getValue().trim();
  var masterName = getName.replace(/　/g, ""); //スペース削除

  /* 内容を取得 */
  var noteCell = activeSpreadsheet.getRange("B5");
  if (noteCell.isBlank()) {
    throw new Error("本日の所感が入力されていません。\n入力後に再度送信ボタンを押下してください。")
  }
  var mainNote = noteCell.getValue();

  /* 設定ファイルから情報を取得 */
  var settingUrl = "https://docs.google.com/spreadsheets/d/xxxxxxxxxx";
  var settingSpreadsheet= SpreadsheetApp.openByUrl(settingUrl);
  var settingSheet = settingSpreadsheet.getSheetByName("mail");
  var lastRow = settingSheet.getLastRow() + 1;

  for(var i = 1;i < lastRow;i++){
    var setName = settingSheet.getRange(i,1).getValue();
    if(setName == masterName){
      var strCc = settingSheet.getRange(i,4).getValue(); //宛先
      var strFrom = settingSheet.getRange(i,2).getValue(); //差出アドレス
    }
  }
  if (!strFrom) {
    throw new Error("C2に正しく氏名を入力してください（姓と名の間に空白は入れない")
  }

  /* メールデータの準備 */
  var strUrl = activeSpreadsheet.getUrl();
  var strGid = activeSpreadsheet.getSheetId();
  var strDate = Utilities.formatDate(new Date(),"JST","MM/dd");
  var strSubject = "【業務内容】" + strDate + " " + masterName;
  var strBody = mainNote + "\n\n" + strUrl + "#gid=" + strGid;

  /* メールを送信 */
  GmailApp.sendEmail(
    "",
    strSubject,
    strBody,
    {
      cc:strCc,
      from:strFrom,
      name: masterName
    }
  );
}

```