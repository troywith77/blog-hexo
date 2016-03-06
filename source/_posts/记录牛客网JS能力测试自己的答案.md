title: 牛客网JS能力测试自己的答案
date: 2016-01-15 22:50:30
tags:
---
- 查找数组元素位置

题目描述:

找出元素 item 在给定数组 arr 中的位置

输出描述:

> 如果数组中存在 item，则返回元素在数组中的位置，否则返回 -1

输入例子:

indexOf([ 1, 2, 3, 4 ], 3)

输出例子:

2

```
function indexOf(arr, item) {
    var index = arr.indexOf(item);
	if(index > -1) {
        return index;
    } else {
        return -1;
    }
}
```

<!--more-->

- 数组求和

题目描述:

计算给定数组 arr 中所有元素的总和

输入描述:

> 数组中的元素均为 Number 类型

输入例子:

sum([ 1, 2, 3, 4 ])

输出例子:

10

```
function sum(arr) {
    var sum = 0;
	arr.map(function(item) {
        sum += item;
    })
    return sum;
}
```

- 计时器

题目描述:

实现一个打点计时器，要求:

1、从 start 到 end（包含 start 和 end），每隔 100 毫秒 console.log 一个数字，每次数字增幅为 1

2、返回的对象中需要包含一个 cancel 方法，用于停止定时操作

3、第一个数需要立即输出

```
function count(start, end) {
    console.log(start);
    var temp = start + 1;
	var timer = setInterval(function() {
        if(temp >= end) {clearInterval(timer)}
        console.log(temp);
        temp += 1;
    }, 100)
    return {
        cancel: function() {
            clearInterval(timer)
        }
    }
}
```