---
title: About VSCode Extension （Plus Tips）
date: 2019-10-05 19:53:42
tags:
- setting
- extension
- VSCode
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: System Setting
---

关于VSCode常用的扩展插件（以及设定技巧）
<!--more-->
### 1. [vscode-icons](https://marketplace.visualstudio.com/items?itemName=vscode-icons-team.vscode-icons)

给项目文件添加icon.

<img src="./1.gif">

***

### 2. [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)

能够以commit为单位在文件内将修改标示出来.甩锅必备.

<img src="./2.gif">

***

### 3. [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

自动调整code的format，默认快捷键是alt（option） + shift + f

<img src="./3.jpg">

***

### 4.[Git History](https://marketplace.visualstudio.com/items?itemName=donjayamanne.githistory)
方便看git log

<img src="./4.gif">

***

### 5.[Bracket Pair Colorizer](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer)

能够为括号标注出不同的颜色！

<img src="./5.jpg">

***

### 6. [TODO Highlight](https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight)

为你的TODO和FIXME之类的comment添加高亮，系统只能帮你到这份上了，记得以后要修复啊！

<img src="./6.png">

***

### 7. [ Path Autocomplete](https://marketplace.visualstudio.com/items?itemName=ionutvmi.path-autocomplete)

为你补全path
<img src="./7.gif">

***

### 8. [Trailing Spaces](https://marketplace.visualstudio.com/items?itemName=shardulm94.trailing-spaces)

强调行内不要的space.

<img src="./8.jpg">

***

### 9. [indent-rainbow](https://marketplace.visualstudio.com/items?itemName=oderwat.indent-rainbow)

 给你的缩进添加颜色！

 <img src="./9.png">

***

### 其他设定

[Code] => [Preferences] => [Settings] ,直接编辑json 或者从GUI搜索

##### 1.最终行的自动改行

settings.json
```JS
"files.insertFinalNewline": true
```

***

##### 2.特殊字符的显示

用于防止向github提交README时产生乱码的现象

settings.json
```JS
"editor.renderControlCharacters": true
```

***

##### 3.关于自动换行

过长的时候进行自动换行显示

settings.json
```JS
"editor.wordWrap": "on"
```
***

##### 4.自动删除不需要的行

settings.json
```JS
"files.trimFinalNewlines": true
```
