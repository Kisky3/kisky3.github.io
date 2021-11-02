---
title: Use Jira To Track Story Points Per Developer On My Team
date: 2021-10-09 23:34:10
tags:
  - jira
clearReading: true
coverCaption: 'Hello World, Hello Programming'
coverSize: partial
comments: false
categories: System Setting
---

使用 jira 来追踪小组组员的 story point

<!--more-->

使用 jira 工具分析小组组员各自完成的 story point 的时候, 发现这个功能是需要付费的。查了一下发现可以使用 jira API 来分析获取数据,从而获取到小组成员个人的分析数据。

试了一下结果是可行的, 取得的数据经过整理可以像下面这样:

```js
{
  "A": 8,
  "B": 18,
  "C": 8,
  "null": 0
}
```

### API token 的发行

首先你需要申请一个 jira 的 API token 来保证你可以使用它。申请详情在这里就不贴了。

## 为了取得 sprint 里的 storypoint 表而准备好 JQL

之后我们为了使用 Issue Search API 而需要准备好相应的 [JQL](https://www.atlassian.com/ja/software/jira/guides/expand-jira/jql)是 Jira 内使用的检索 SQL 语言。执行 JQL 的话可以在 Jira 里的 Filters 的页面里确认结果。

这次我们就在还在进行中的 sprint 里试着取得 storypoint 的列表。

```js
project = XXX AND issuetype = story AND sprint in openSprints()
```

`project = XXX`是 Jira 的项目的 alias
`issuetype = story` 这里是指定 issue 的属性为 story
`sprint in openSprints()` 这个是指定正在进行中的 sprint

## 利用 Isuue Search API 来取得 story 列表

Jira API 里的 Issue Search API 可以帮助我们取得 story 列表。
Issue Search API 能够向 JQL 发送请求, 那么用 curl 来敲一下我们刚刚写下的 JQL

```
curl https://xxx.atlassian.net/rest/api/2/search?jql="project%20%3D%20XXXX%20AND%20issuetype%20%3D%20%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AA%E3%83%BC%20AND%20sprint%20in%20openSprints()"
   --user xxx@fuga.com:HOgehoGeFugaFuGA
```

在这里需要注意的是`jql`参数里需要指定 encode 之后的 URL 地址。
`--user`参数是指[Jira 的登陆邮箱地址] : [API 键]

## 指定 Jira 里使用的 field

API curl 一下的话能得到返回来的 json 形势的 story 列表。
这次利用的是担当者和 storypoint 两个 field。

担当者=`.fields.assignee.displayNam`
StoryPoint=`.fields.customfield_10030`

重点要注意的是`customfield_xxxxx`的`xxxxx`部分是要根据自己的 Jira 来设定的。你需要自定义 Filter,然后查找相应的 customfield_id。

## 使用 JQ 命令行来解析 json

因为返回回来的 json 数据的话饱含着很多的 field,所以我们需要从 json 数据里抽出我们需要的两个数据。
整形用的 jq 代码如下：

```
curl https://xxx.atlassian.net/rest/api/2/search?jql=(省略)
 | jq '.issues | map({ name: .fields.assignee.displayName, storypoint: .fields.customfield_10030})'
 | sed s/null,/\"null\",/ | sed s/null/0/
 | jq 'reduce .[] as $row ({}; .[$row.name] += $row.storypoint)'
```

这样我们就可以得到以下整形成功的数据了！

{
"A": 8,
"B": 18,
"C ": 8,
"null": 0
}

## 指定 sprint 的 storypoint 数据的取得

如果你不是想取得正在进行中的 sprint 的数据而是指定 sprint 的话,你可以将下面的 openSprints 改成 sprint (同样需要进行 encode 变换)

```
sprint in openSprints()
↓
sprint = XXXX
```
