---
title: Create Json By GAS and Deploy to S3
date: 2020-03-12 17:13:07
tags:
- google app script
- json
- s3
- deploy
clearReading: true
thumbnailImage: 20200312.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
用GAS造json数据并自动发布到S3上
<!--more-->
最近在开发中需要造一堆相似的阶层似的json数据。
实在不想手动写，所以研究了一下如何用已经存在的文本文件来造出所需要的json数据，并能够自动deploy到S3上保存起来。

### 准备工作
首先需要在S3上生成上传所需要的user,policy和保存数据用的bucket，
这能保证你在GAS中上传的时候能够使用改user，并且该user拥有上传json数据的权限和连接到你保存数据用的bucket.

#### 1. user和policy的生成：
参照之前的文章[IAM用户的生成](https://kisky3.github.io/2020/02/10/UseServerlessFrameworkToCreateRESTAPI/)
在这里给user添加的policy要稍微修改一下，生成一个只能上传的policy
例：
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "s3:PutObject",
            "Resource": "*"
        }
    ]
}
```
#### 2. bucket的生成：
bucket就直接在S3新建就好了，但是在access权限上，最好是设置成只能取得这个bucket数据的权限。
例：
```
{
    "Version": "2012-10-17",
    "Id": "Policy1454907251774",
    "Statement": [
        {
            "Sid": "Stmt1454907246553",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject", // 只能取得数据的Action
            "Resource": "arn:aws:s3:::your_bucket_name/*"
        }
    ]
}
```
并在Bucket Property的地方选择并如下图般设置Static website hosting,并保存
<img src="./1.png" style="width:400px;margin:40px 0">

***
### GAS的生成
在准备工作做好之后就可以开始生成json数据的工作了。
首先整理出你所需要的文字版数据。
<img src="./2.png" style="width:400px;margin:40px 0">

然后利用GAS的API来根据你文字版数据的sheet和内容生成你所需要的json。
写法基本和普通的js差不多，取得数据后编辑成你所需要的形式，
然后利用你刚刚生成的S3的user和bucket情报来进行数据的自动上传。

在这里GAS的内容就不说明了，大概在方法里都写了注释。
生成的是一个比较复杂的阶层式的json，写的比较粗糙。(sorry)

GAS例：
```JS
/* domainによってjsonを生成してS3上にUPする ***********************************************************/
// 個別のjsonを生成してS3上にUPする
function CreateAndUploadJson() {
  const domain = "xxxxxxxx"
  createCategoryData(domain)
  UploadJson(domain)
}

// 全てのjsonを一気に生成してUPする(重いので、こげる可能性がある)
function CreateAndUploadAll() {
  const domain = ["audio-takakuureru", "beauty-takakuureru", "bicycle-takakuureru", "brand-takakuureru", "camera-takakuureru", "delivery-kaitori", "figure-takakuureru", "fishing-takakuureru", "g-takakuureru", "gakki-takakuureru", "guitar-kaitori", "gun-takakuureru", "kaden-takakuureru", "kagu-takakuureru", "kenki-takakuureru", "kougu-takakuureru", "navi-takakuureru", "noukigu-takakuureru", "pc-takakuureru", "sax-takakuureru", "sports-takakuureru", "syuttyou-kaitori", "tokei-takakuureru", "train-takakuureru", "tv-takakuureru"]　
  domain.forEach(json_domain => {
    createCategoryData(json_domain)
    UploadJson(json_domain)
  })
}

/* S3上Upload用 ***********************************************************/
function UploadJson(domain) {
  var accessKey = 'xxxxxxxxxx';
  var secretKey = "xxxxxxxxxx"
  var bucketName = "xxxxxxxxx"
  var spreadsheetId = 'xxxxxxxxxxxx'
  var sheetName = domain;
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  var sheet = ss.getSheetByName(sheetName);
  // 生成されたシートからデータを取得
  var data = String(sheet.getRange('A:A').getValues());
  // もし文字列の最後「,」が存在する場合削除する
  while (data.charAt(data.length - 1) == ',') {
    data = data.substr(0, data.length - 1);
  }
  json = Utilities.newBlob(data, "application/json");
  var s3 = S3.getInstance(accessKey, secretKey);
  var response = s3.putObject(bucketName, sheetName + '.json', json, {
    logRequests: true
  });
  Logger.log(response);
}

/* サイト毎のメイン処理 **********************************************/
function createCategoryData(sheetName) {
  var sheet = SpreadsheetApp.getActive().getSheetByName(sheetName + '-data');
  //dataへ変換
  var dataArray = convSheet(sheet);
  //dataをJsonに変換し、新規シート出力
  creatNewSheet(dataArray, sheetName);
}

//data（配列）へ変換関数
function convSheet(sheet) {
  let categoryJson = []
  Logger.log(sheet)
  //領域
  var jsonRange = sheet.getRange(2, 2, sheet.getLastRow() - 1, sheet.getLastColumn() - 1);
  // Logger.log('jsonRange:'+JSON.stringify(jsonRange));
  var jsonRowValues = jsonRange.getValues();
  // Logger.log('jsonRowValues:'+JSON.stringify(jsonRowValues));
  for (let i = 0; i < jsonRowValues.length; i++) {
    const catLvlObj = categoryJson.filter(cat0 => cat0.title === jsonRowValues[i][0])
    // 大カテゴリーは存在する場合、被るかどうかをチェック
    if (!!Object.keys(catLvlObj).length) {　
      const cat1LvlObj = catLvlObj[0].options.filter(cat1 => cat1.title === jsonRowValues[i][1])
      const att = CreateJson(jsonRowValues[i][5])
      // 中カテゴリーは存在する場合、被るかどうかをチェック
      　　
      if (!!Object.keys(cat1LvlObj).length) {　　　　
        cat1LvlObj[0].options.push({
          id: jsonRowValues[i][5],
          title: jsonRowValues[i][2],
          category: jsonRowValues[i][6],
          attributes: att
        })　　　　
      } else {
        // 中カテゴリーは値がない場合　追加
        catLvlObj[0].options.push({
          id: jsonRowValues[i][4],
          title: jsonRowValues[i][1],
          options: [{
            id: jsonRowValues[i][5],
            title: jsonRowValues[i][2],
            category: jsonRowValues[i][6],
            attributes: att
          }]
        })
      }
      // 大カテゴリーは存在しない場合新規追加
    } else {
      let catNew = new Object
      catNew.id = jsonRowValues[i][3]
      catNew.title = jsonRowValues[i][0]
      const att = CreateJson(jsonRowValues[i][5])
      catNew.options = [{
        id: jsonRowValues[i][4],
        title: jsonRowValues[i][1],
        options: [{
          id: jsonRowValues[i][5],
          title: jsonRowValues[i][2],
          category: jsonRowValues[i][6],
          attributes: att
        }]
      }]
      categoryJson.push(catNew)
    }
  }
  return categoryJson
}

//新規シート作成関数
function creatNewSheet(dataArray, sheetName) {
  //data(配列)よりJsonへ変換
  var json = JSON.stringify(dataArray);
  //出力
  var newSheetName = sheetName;
  //新規シート初期化
  var newSheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(newSheetName);
  //存在有無確認
  //存在しない場合
  if (!newSheet) {
    //シート生成
    newSheet = SpreadsheetApp.getActiveSpreadsheet();
    newSheet.insertSheet(newSheetName);
    //データ入力
    newSheet.getRange("A1").setValue(json);
    //存在する場合
  } else {
    //データの書換え
    newSheet.getRange("A1").setValue(String(json));
  }
}

// 追加項目リスト作成関数
function CreateJson(cat2_id) {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const fieldSheet = ss.getSheetByName("抽出_format");
  const elementSheet = ss.getSheetByName("m_format_element");
  const fieldData = fieldSheet.getDataRange().getValues();
  const elementData = elementSheet.getDataRange().getValues();
  let value = []
  for (let i = 2; i < fieldData.length; i++) {
    const row = fieldData[i]
    const outputFlg = row[0]
    if (outputFlg === 0) continue;
    let inputHint = row[2];
    const sort = row[4];
    const categoryId = row[6];
    const formatId = row[7];
    const name = row[9];
    const formatType = row[13];
    const fieldKey = row[14];
    // validate rule
    const rules = []
    const required = row[3];
    const maxLength = row[10];
    if (required === 1) {
      rules.push("required")
    }
    if (maxLength !== "") {
      rules.push(`max:${maxLength}`)
    }
    // select, radioのoptions
    let elements;
    if (formatType === "select" || formatType === "radio") {
      elements = elementData.filter(row => row[0] === formatId).map(row => row[2])
      if (formatType === "radio") {
        const radioObj = []
        elements.map(el => radioObj.push({
          label: el,
          value: el
        }))
        elements = radioObj
      }
    }
    if (row[6] === Number(cat2_id)) {
      value.push({
        "field_name": name,
        "field_key": fieldKey,
        "type": formatType,
        "required": `${required === 1}`,
        "rules": `${rules.length > 0 ? rules.join("|") : ""}`,
        "options": elements === null ? "" : elements,
        "hint": `${inputHint === null ? "" : inputHint}`,
        "sort": sort,
        "format_id": formatId
      })
    }
  }
  return value
}
```
***
### 生成结果
执行以上GAS的CreateAndUploadJson方法，就可以看到json数据被生成了sheet,并上传到了你指定的S3 Bucket上。
<img src="./3.png" style="width:400px;margin:40px 0">

以上

***
# 参考
[多分保存版？S3とSpreadSheet間でのCSVとJSONのデータ読み書きを簡単なサンプルで網羅してみた](https://dev.classmethod.jp/articles/csv-and-json-convert-between-s3-and-spreadsheet/)

[Google Apps Scriptを利用してGoogleスプレッドシートのデータをS3へJSONとして保存する](https://dev.classmethod.jp/articles/google-apps-script-gss-to-s3-json/)