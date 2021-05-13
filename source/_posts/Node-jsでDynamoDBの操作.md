---
title: Basic Operations on DynamoDB Tables by node.js
date: 2021-04-08 00:04:54
tags:
- AWS
- DynamoDB
- Node.js
- DocumentClient
---
用node.js对DynamoDB进行的基本操作
<!--more-->
用js对DynamoDB进行数据操作的话有两种方法,
一种是使用AWS.DynamoDB,另一种是使用AWS.DynamoDB.DocumentClient(docClient)

例：AWS.DynamoDB
```js
var dynamodb = new AWS.DynamoDB();
var params = {
    TableName: 'DogTable',
    Item:{
        'dogId':{N: '12'}, // 数字的情况下需要指定N
        'name':{S: '项羽'} // 字符串的情况下要指定S
    }
};
dynamodb.putItem(params, callback);
```


例：AWS.DynamoDB.DocumentClient
```js
var docClient = new AWS.DynamoDB.DocumentClient();
var params = {
    TableName: 'DogTable',
    Item:{
         dogId: 12,
         name: '项羽'
    }
};
docClient.put(params, callback);
```

使用docClient的话,能够直接把原生的js代码转换成DynamoDB上的数据类型,代码相对简洁。
因此在此只讨论docClient的情况。

***
### PUT (插入数据)
往SuperCarTable表里的主键(carId)里插入12这个项目。
```js
var params = {
    TableName: 'SuperCarTable',
    Item:{                               // 主键是必须的（如果有排序键的话写上排序键）
         carId: 12,                      // 主键
         name: 'フェラーリ458',
         price: 28300000,
         engine: {
             type: 'V型8気筒',
             power: 578
         },
         color:['red', 'black', 'white']
    }
};
docClient.put(params, callback);
```

***
### GET（获取数据）
```js
var params = {
    TableName: 'SuperCarTable',
    Key:{ // 指定你想取得数据的主键(或者排序键)
         carId: 12
    }
};
docClient.get(params, function(err, data){
    if(err){
        console.log(err);
    }else{
        console.log(data.Item.name);       //'宝马458'
        console.log(data.Item.engine.type);//'V型8号引擎'
        console.log(data.Item.color[2]);   //'白色'
    }
});
```
***
### UPDATE(更新)
更新SuperCarTable表里主键(carId)为12的数据的其他属性。
```js
var params = {
    TableName: 'SuperCarTable',
    Key:{  // 指定你想取得数据的主键(或者排序键)
         carId: 12
    },
    // 指定UpdateExpression的更新名
    ExpressionAttributeNames: {
        '#n': 'name',
        '#d': 'designer'
        '#e': 'engine',
        '#t': 'type',
        '#p': 'power',
        '#c': 'color'
    },
    // 更新各属性的值
    ExpressionAttributeValues: {
        ':newName': 'フェラーリ488GTB',
        ':newdesigner': 'フラビオ・マンツォーニ',
        ':newType': 'V型8気筒ツインターボ',
        ':addPower': 92,
        ':newColor': ['yellow']
    },
    UpdateExpression: 'SET #n = :newName, #d = :newDesigner, #e.#t = :newType, #e.#p = #e.#p + :addPower, #c = list_append(#c, :addPower)'
};
docClient.update(params, callback);

```
#### 关于UpdateExpression
UpdateExpression是根据定义好的String的定义式来指定更新内容的。
必须要在UpdateExpression的最开始使用指定的action keyword,用逗号隔开的话可以同时执行多个指令。但是每个指定只能在一个UpdateExpression里使用一次。

例:
```js
UpdateExpression: 'SET #a = :aval, #b = :bval REMOVE #c'
```

#### SET ACTION:
用于属性的追加和加工。比如对数值进行处理后追加,还可以在数列里添加数据。(数列时要使用[]将数据框起来)

#### REMOVE ACTION:
删除指定项目的指定属性, 也可以只删除指定数列内的数据。

例:
```js
ExpressionAttributeNames:{
    '#pr': 'price',
    '#c': 'color'
}
UpdateExpression: 'REMOVE #pr, #c[1]' // 删除price属性和color数列的第二个值
```
***
#### ADD ACTION:
用于数值属性的加法处理,追加SET类型的值。(推荐用SET ACTION进行代替)
另外,SET类型是原生Javascirpt不存在的类型。

```js
ExpressionAttributeNames:{
    '#pr': 'price',
    '#c': 'color'// color属性为String Set型
},
ExpressionAttributeValues:{
    ':addPrice': 7400000,  // 为price属性加上7400000
    ':addColor': docClient.createSet(['Yellow', 'Gray']) // 为color属性添加Yellow和Gray的值
},
UpdateExpression: 'ADD #pr :addPrice, #c :addColor'
```

***
#### DELETE ACTION:
可以从SET型的属性删除指定的值。

```js
ExpressionAttributeNames:{
    '#c': 'color' // color属性为String Set型
},
ExpressionAttributeValues:{
    ':deleteColor': docClient.createSet(['Red', 'Blue'])  // 从color属性删除Red和Blue的值
},
UpdateExpression: 'DELETE #pr :deletePrice'

```
如果是删除指定的一列数据的话,可以直接使用delete。

```js
var params = {
    TableName: 'SuperCarTable',
    Key:{   // 指定想删除项目的主键(或排序键)
         carId: 12
    }
};
docClient.delete(params, callback);
```
***
#### QUERY(检索)和SCAN(取得所有)
QUERY:
基本情况下,表的主键和排序键(或者两者同时)的情况下才能进行query。
但是如果使用了*GSI*的情况下,也可以利用其他的属性来进行检索。

GSI:

|  name(pk)  |  year(sk)  |	category| nation|
| ---- | ---- | ---- |---- |
|  |  |   |

例:
#### 对1980年前获奖的获奖者进行query。
这时query的对象是sort key,所以直接可以进行query
```js
var params = {
    TableName: 'NovelPrizeTable',
    ExpressionAttributeNames:{'#y': 'year'},
    ExpressionAttributeValues:{':val': 1980},
    KeyConditionExpression: '#y <= :val'//検索対象が満たすべき条件を指定
};
docClient.query(params, function(err, data){
    if(err){
        console.log(err);
    }else{
       data.Items.forEach(function(person, index){
           console.log(person.name);//1980年以前の受賞者の名前
       });
    }
});
```
***
#### 对获得经济学奖的获奖人进行query
这时进行query的对象是category。所以要设置一个将category作为主键的GSI(Global secondary index)
1. 打开DynamoDB的控制台里的index tab.
2. 生成index
3. 主键里输入「category」,将数据类型选择为字符串.
4. sort key 可以不用填写.
5. index名可以设置为「category-index」
6. 点击生成index.

代码如下:
```js
var params = {
    TableName: 'NovelPrizeTable',
    IndexName: 'category-index',//インデックス名を指定
    ExpressionAttributeNames:{'#c': 'category'},
    ExpressionAttributeValues:{':val': 'economics'},
    KeyConditionExpression: '#c = :val'//検索対象が満たすべき条件を指定
};
docClient.query(params, function(err, data){
    if(err){
        console.log(err);
    }else{
       data.Items.forEach(function(person, index){
           console.log(person.name);//経済学賞受賞者の名前
       });
    }
});
```
***
### GSI query的时候对IAM Role的设置
为了获得对`NovelPrizeTable`的操作权限, `NovelPrizeTable`的ARN操作时要分配到可以拥有操作权限的IAMRole.
但是NovelPrizeTable的GSI进行query的时候，必须要追加对当前的GSI的ARN的操作权限的Resource.
这时IAM Role的设置如下(表名字的后面添加/index(index名))
```js
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "dynamodb:Query"
            ],
            "Resource": [
                "arn:aws:dynamodb:ap-northeast-1:********:table/NovelPrizeTable",
                "arn:aws:dynamodb:ap-northeast-1:********:table/NovelPrizeTable/index/category-index"
            ]
        }
    ]
}

```