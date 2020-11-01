---
title: Basic Velocity
date: 2020-06-10 23:28:46
tags:
- Velocity
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Back-end Knowledge
---
关于Velocity的基础语法
<!--more-->
Velocity是一个基于java的模板引擎（template engine），它允许任何人仅仅简单的使用模板语言（template language）来引用由java代码定义的对象。作为一个比较完善的模板引擎，Velocity的功能是比较强大的，但强大的同时也增加了应用复杂 性。这里简单Velocity脚本的基本语法：

### 1、参数的设置
可以自定义参数。参数开始于$
```js
 写法	       当值为空值当时候	   使用方法
 $name	      显示参数名	     普通
 ${name}	  显示参数名	     在英文语义暧昧的时候
 $!name	      不显示	         当参数为空值的时候不显示
```
参数所能使用的文字,参数只能使用下面的文字:
```
・a-z A-Z
・0-9
・-
・_
```
参数的话就可以单纯用`$name`和使用花括号圈起来的写法`${name}`,如果在只是英文数字的文字列中间插入参数的话，必须使用花括号。

例：abc${D}efg

***
### 2.注释
一行的注释：使用##
```
##comment
Hello $value.

Hello $value.##comment
```

多行的注释：使用`#*`开始, 使用`#*`结束。
```
#*
comment
comment
*#

hello #*comment*# $value.
```
***
### 3.读取别的文件
1. 读取VM文件
在头部或尾部分别读取文件的话，使用`#parse`.
```
#parse("body_header.vm.html")
$data
#parse("body_footer.vm.html")
```
文件的位置是与该文件的相对位置。如果不是直接放置在根目录而是全部放置在template文件夹里的情况下，需要在Velocity初始化之前指定路径，否则可能会出现错误信息。
```
Velocity.setProperty("file.resource.loader.path","template" );
```

错误信息
```
org.apache.velocity.exception.ResourceNotFoundException: Unable to find resource
```
***
### 3.读取一般的TEXT
如果是读取一般的普通文档的话，使用`#include`.
```
#include("test.vm")
```

***
### 4.循环
循环使用`foreach`,值 in list名.在循环的最后必须要使用`#end`
```
#foreach ($data in $list)
$data.name
#end
```

关于循环中的index号码，的话可以使用`velocityCount`来计算进入循环的次数.
```
#foreach( $title in ["a","b","c"] )
    $title $velocityCount $velocityHasNext
#end
```

运行结果:
```
a 1 true b 2 true c 3 false
```
关于多重数列的index
```
 <table border=1>
 #set($i=0)
 #foreach( $cells in [["a","b"],["c","d"]])
 <tr><td>$velocityCount</td>
 #set($j=0)
 #foreach($item in $cells)
 <td>${i}-$j $item</td>
 #set($j=$j + 1)
 #end
 </tr>
  #set($i=$i + 1)
 #end
 </table>
```

运行结果
```
1 0-0 a 0-1 b
2 1-0 c 1-1 d
```

在循环的最初和途中和最后的分支.
```
 #foreach( $value in ["a","b","c","d"])
 #if($velocityCount==1)
 最初 $value
 #elseif($velocityHasNext)
 途中 $value
 #else
 最後 $value
 #end
 #end
```

运行结果
```
最初 a 途中 b 途中 c 最後 d
```

从loop开始的break

Velocity1.6以后的版本允许在loop中使用break，来进跳出循环.
```
#foreach( $title in $titles )
    #if( $velocityCount > 1 )
        #break
    #end
    $title
#end
```
***
### 4. 条件文
if条件, 最后一定要写`#end`:
```
#if($value > 3)
$value
#end
```

else if 和 else
```
#if( $age < 20 )
    十代
#elseif( $age <30 )
   二十台
#elseif( $age <40 )
    三十台
#else
   その他
#end
```

null的条件文
```
#if($value)
nullでない時
#end
```