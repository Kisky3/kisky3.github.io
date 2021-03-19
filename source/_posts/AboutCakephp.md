---
title: How to use var_dump and print_r into Cakephp
date: 2021-02-05 10:14:41
tags:
- php
- cakephp
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Back-end Knowledge
---
如何在Cakephp里使用var_dump和print_r
<!--more-->
### var_dump
```
var_dump(参数)
```
例：
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
  </head>
  <body>
    <?php
      $data = 70;
      var_dump($data);
    ?>
  </body>
</html>
```

结果：
<img src="./1.png" style="width: 500px">

字符串
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
  </head>
  <body>
    <?php
      $data = "abcde";
      var_dump($data);
    ?>
  </body>
</html>
```
结果：
<img src="./2.png" style="width: 500px">

数列：
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
  </head>
  <body>
    <?php
      $data = array("one",2,3,"four");
      var_dump($data);
    ?>
  </body>
</html>
```
<img src="./3.png" style="width: 500px">

# 与print_r的不同
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
  </head>
  <body>
    <?php
      $data = array("one",2,3,"four");
      var_dump($data);
      echo "<br />";
      print_r ($data);
    ?>
  </body>
</html>
```
<img src="./4.png" style="width: 500px">
第一行为var_dump的输出内容、第二行为print_r的输出内容。
print_r一般只打印内容。

***

当是写在view(tpl)里面的时候，可以用下面的写法直接在view里显示数列的内容。
```
{$array|@debug_print_var}
{$array|@print_r}
{$array|@var_dump}
```