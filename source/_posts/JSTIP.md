---
title: JS/ES TIP
date: 2018-06-25 18:46:41
tags:
- Js
- ES
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
categories: Front-end Knowledge
---
About common js/es tips. Continuously update here.
<!--more-->
{% alert success no-icon %}
#### 过滤唯一值
{% endalert %}
ES6 引入了 Set 对象和延展（spread）语法…，我们可以用它们来创建一个只包含唯一值的数组。
```js
const array = [1, 1, 2, 3, 5, 5, 1]
const uniqueArray = [...new Set(array)];
console.log(uniqueArray); // Result: [1, 2, 3, 5]
```
在 ES6 之前，获得同样的数组需要更多的代码！

这个技巧可以支持包含原始类型的数组：undefined、null、boolean、string 和 number。但如果你的数组包含了对象、函数或其他嵌套数组，就不能使用这种方法了。

{% alert success no-icon %}
#### 二分法
{% endalert %}
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

{% alert success no-icon %}
### 冒泡法
{% endalert %}
进行乱数数组排列的时候可以用冒泡法。

```js
arr = arr.sort((a, b) => {
	return a - b
});
console.log(arr);
```

{% alert success no-icon %}
### 取数列中的最大值
{% endalert %}
```js
let piles = [2,5,4,8,6,2,4];
let max = Math.max(...piles);
console.log(max)
// 8
```

{% alert success no-icon %}
### 在循环中缓存数组长度
{% endalert %}
在我们学习使用 for 循环时，一般建议使用这种结构：
```js
for (let i = 0; i < array.length; i++){
  console.log(i);
}
```
在使用这种方式时，for 循环的每次迭代都会重复计算数组长度。

有时候这个会很有用，但在大多数情况下，如果能够缓存数组的长度会更好，这样只需要计算一次就够了。我们可以把数组长度复制给一个叫作 length 的变量，例如：
```js
for (let i = 0, length = array.length; i < length; i++){
  console.log(i);
}
```
这段代码和上面的差不多，但从性能方面来看，即使数组变得很大，也不需要花费额外的运行时重复计算 array.length。


{% alert success no-icon %}
### 短路求值(Short-Circuit Evaluation)
{% endalert %}
`||` 运算符可用于返回表达式中的第一个真值，而` && `运算符可用于阻止执行后续代码。

其实有时也可以使用三项演算符.

#### 使用 && 运算符
例:
下面是一个 if-else 语句转换为短路求值的示例。在本例中，我们希望获得用户的显示名。如果 user 对象有 name 属性，我们将使用它作为显示名。否则，我们只会称我们的用户为 Guest。

normal
```js
const user = {}

let displayName

if (user.name) {
  displayName = user.name
} else {
  displayName = 'Guest'
}
console.log(displayName) // "Guest"
```

短路求值:
```js
const user = {}
const displayName = user.name || 'Guest'
console.log(displayName) // "Guest"
```

在多个条件判断下也同样适用:
normal
```js
const user = { name: 'IU' }
let displayName

if (user.nickname) {
  displayName = user.nickname
} else if (user.name) {
  displayName = user.name
} else {
  displayName = 'Guest'
}

console.log(displayName) // "IU"
```

短路求值:
```js
const user = { name: 'IU' }
const displayName = user.nickname || user.name || 'Guest'
console.log(displayName) // "IU"
```

你是否曾经在访问嵌套对象属性时遇到过问题？你可能不知道对象或某个子属性是否存在，所以经常会碰到让你头疼的错误。

假设我们想要访问 this.state 中的一个叫作 data 的属性，但 data 却是 undefined 的。在某些情况下调用 this.state.data 会导致 App 无法运行。为了解决这个问题，我们可以使用条件语句：
normal
```js
if (this.state.data) {
  return this.state.data;
} else {
  return 'Fetching Data';
}
```

短路求值:
```js
return (this.state.data || 'Fetching Data');
```

#### 使用 && 运算符
&& 短路的工作原理有点不同。使用 && 运算符，我们可以确保只有在前面的表达式是真实的情况下才运行后续表达式。这到底是什么意思？让我们看另一个例子！在本例中，如果服务器响应不成功，我们希望在控制台中显示一个错误。首先，我们可以使用 if 语句来实现这一点。

normal:
```js
const response = { success: false }
if (!response.success) {
  console.error('ERROR')
}
```

短路求值:
```js
const response = { success: false }
!response.success && console.error('ERROR')
```

{% alert success no-icon %}
### 转换成布尔值
{% endalert %}

除了标准的布尔值 true 和 false，在 JavaScript 中，所有的值要么是“真值”要么是“假值”。

在 JavaScript 中，除了 0、“”、null、undefined、NaN 和 false 是假值之外，其他的都是真值。

我们可以使用!云算法来切换 true 和 false。

```js
const isTrue  = !0;
const isFalse = !1;
const alsoFalse = !!0;
console.log(true); // Result: true
console.log(typeof true); // Result: "boolean"
```

{% alert success no-icon %}
### 快速幂运算
{% endalert %}
从 ES7 开始，可以使用**进行幂运算，比使用 Math.power(2,3)要快得多。
```js
console.log(2 ** 3); // Result: 8
```


{% alert success no-icon %}
### 快速取整
{% endalert %}
我们可以使用 Math.floor()、Math.ceil()或 Math.round()将浮点数转换成整数，但有另一种更快的方式，即使用位或运算符`|`
```js
console.log(23.9 | 0);  // Result: 23
console.log(-23.9 | 0); // Result: -23
```
如果 n 是正数，那么 n|0 向下取整，否则就是向上取整。它会移除小数部分，也可以使用~~达到同样的效果。

{% alert success no-icon %}
### 获取数组最后的元素
{% endalert %}
数组的 slice()方法可以接受负整数，并从数组的尾部开始获取元素。
```js
let array = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
console.log(array.slice(-1)); // Result: [9]
console.log(array.slice(-2)); // Result: [8, 9]
console.log(array.slice(-3)); // Result: [7, 8, 9]
```

{% alert success no-icon %}
### 声明和初始化数组
{% endalert %}
我们可以使用特定的大小来初始化数组，也可以通过指定值来初始化数组内容，大家可能用的是一组数组，其实二维数组也可以这样做，如下所示：

```js
const array = Array(5).fill('');
// 输出
(5) ["", "", "", "", ""]

const matrix = Array(5).fill(0).map(() => Array(5).fill(0))
// 输出
(5) [Array(5), Array(5), Array(5), Array(5), Array(5)]
0: (5) [0, 0, 0, 0, 0]
1: (5) [0, 0, 0, 0, 0]
2: (5) [0, 0, 0, 0, 0]
3: (5) [0, 0, 0, 0, 0]
4: (5) [0, 0, 0, 0, 0]
length: 5
```

{% alert success no-icon %}
### 求和，最小值和最大值
{% endalert %}
`reduce()`方法接收一个函数作为累加器，数组中的每个值（从左到右）开始缩减，最终计算为一个值。
```js
const array  = [5,4,7,8,9,2];
```

求和:
```js
array.reduce((a,b) => a+b);
```

最大值:
```js
array.reduce((a,b) => a>b?a:b);
```

最小值:
```js
array.reduce((a,b) => a<b?a:b);
// 输出: 2
```

{% alert success no-icon %}
### 排序字符串，数字或对象等数组
{% endalert %}
我们有内置的方法sort()和reverse()来排序字符串，但是如果是数字或对象数组呢

字符串数组排序:
```js
const stringArr = ["Joe", "Kapil", "Steve", "Musk"]
stringArr.sort();
// 输出
(4) ["Joe", "Kapil", "Musk", "Steve"]

stringArr.reverse();
// 输出
(4) ["Steve", "Musk", "Kapil", "Joe"]
```

数字数组排序:
```js
const array  = [40, 100, 1, 5, 25, 10];
array.sort((a,b) => a-b);
// 输出
(6) [1, 5, 10, 25, 40, 100]

array.sort((a,b) => b-a);
// 输出
(6) [100, 40, 25, 10, 5, 1]
```

对象数组排序:
```js
const objectArr = [
    { first_name: 'Lazslo', last_name: 'Jamf'     },
    { first_name: 'Pig',    last_name: 'Bodine'   },
    { first_name: 'Pirate', last_name: 'Prentice' }
];
objectArr.sort((a, b) => a.last_name.localeCompare(b.last_name));
// 输出
(3) [{…}, {…}, {…}]
0: {first_name: "Pig", last_name: "Bodine"}
1: {first_name: "Lazslo", last_name: "Jamf"}
2: {first_name: "Pirate", last_name: "Prentice"}
length: 3

```

{% alert success no-icon %}
JS中去除数组中的假值（0, 空，undefined, null, false）
{% endalert %}
```js
arr.filter(Boolean)
```





