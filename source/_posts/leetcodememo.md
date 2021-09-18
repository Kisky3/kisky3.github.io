---
title: LeetCode Memo
date: 2021-06-22 22:19:00
tags:
- LeetCode
---
LeetCode笔记
<!--more-->

刷LeetCode啊！！！

### 1. 二分法
二分法最常见的是取到中位数。并且不断地根据中位数更新起点和终点，缩小范围到取得理想值。

一般来说首先设置start为1，end为数组最大位

```js
const start = 0, const end = array.length - 1(or something else)
```

然后取中点：
```js
const mid = Math.floor(start + (end - start) / 2)
```

通过while进行start和end的比较，直到start = end 然后结束处理。
进行中位数相关的条件判断, 将start或end重新代入。
最后在while结束循环的时候返回start。

```js
while (start < end) {
      const mid = Math.floor(start + (end - start) / 2)
      if (进行中位数相关的条件判断) {
        start = mid + 1
      } else {
        end = mid
      }
    }
    return start
```

### 2.冒泡法
进行乱数数组排列的时候可以用冒泡法。(背下来～！)

```js
arr = arr.sort((a, b) => {
	return a - b
});
console.log(arr);
```

### 3.取数列中的最大值
```js
let piles = [2,5,4,8,6,2,4];
let max = Math.max(...piles);
console.log(max)
// 8
```




