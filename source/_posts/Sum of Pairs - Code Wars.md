---
title: Sum of Pairs - Code Wars
date: 2018-10-23 13:26:45
tags:
---

今天在code wars上遇到一道题，卡了蛮久，特地记录一下。

### introductions

Given a list of integers and a single sum value, return the first two values (parse from the left please) in order of appearance that add up to form the sum.

<!--more-->

```javascript
sum_pairs([11, 3, 7, 5],         10)
#              ^--^      3 + 7 = 10
== [3, 7]

sum_pairs([4, 3, 2, 3, 4],         6)
#          ^-----^         4 + 2 = 6, indices: 0, 2 *
#             ^-----^      3 + 3 = 6, indices: 1, 3
#                ^-----^   2 + 4 = 6, indices: 2, 4
#  * entire pair is earlier, and therefore is the correct answer
== [4, 2]

sum_pairs([0, 0, -2, 3], 2)
#  there are no pairs of values that can be added to produce 2.
== None/nil/undefined (Based on the language)

sum_pairs([10, 5, 2, 3, 7, 5],         10)
#              ^-----------^   5 + 5 = 10, indices: 1, 5
#                    ^--^      3 + 7 = 10, indices: 3, 4 *
#  * entire pair is earlier, and therefore is the correct answer
== [3, 7]
```

Negative numbers and duplicate numbers can and will appear.

NOTE: There will also be lists tested of lengths upwards of 10,000,000 elements. Be sure your code doesn't time out.

### 思路

最初的想法是遍历两次数组，先找出所有符合的数字，然后再在这些符合的数字里看哪个index更小，再返回那两个index对应的数字组成的数组，代码：

```javascript
var sum_pairs = function (ints, s) {
  let pairs = []
  for (let i = 0; i < ints.length; i++) {
    for (let j = i + 1; j < ints.length; j++) {
      if (ints[i] + ints[j] === s) pairs.push([i, j])
    }
  }
  if (!pairs.length) return
  return pairs.reduce((prev, next) => prev[1] > next[1] ? next : prev, [Infinity, Infinity])
  .reduce((acc, idx) => [...acc, ints[idx]], [])
}
```

跑了一下测试案例，顺利通过了，然后一提交就报超时了，然后回去仔细看了下introduction，发现这句话`NOTE: There will also be lists tested of lengths upwards of 10,000,000 elements. Be sure your code doesn't time out.`。回头再看下我的代码，一千万条数据先来个双重遍历，后面又是一个reduce遍历，不超时才怪啊。由此开始了苦恼的想简化复杂度的过程，想了至少半个小时怎么想都没想到怎么处理才能把开始的双重遍历改成一次遍历。

后来又开始仔细地看introduction，里面写的是找出index更靠前的相加等于参数的两个数字，如果换一种方式思考的话，也就是说一次遍历中每一次遍历的元素如果发现前面已经有出现过的元素和它匹配，那这个时候这两个数肯定是index最小的两个数，比如`sum_pairs(([10, 5, 2, 3, 7, 5], 10)`，第一个元素是10，它需要一个0组队才能想加等于10，第二个元素是5，它需要一个5，第三个是2需要一个8，第四个是3需要一个7，第五个是7需要一个3，这个时候我们发现前面正好已经出现过一个3了，我们第一次发现了符合条件的两个数，而且比数组里另外一组符合条件的数`[5, 5]`更靠前，这个时候我们就可以直接返回这两个数而不用继续遍历数组了，更不用做双重遍历了。所以我们要做的就是把每次遍历过的元素记住，以便后面的查找。

```javascript
// 277ms on average
var sum_pairs=function(ints, s){
  const mem = new Map()
  for (let i = 0; i < ints.length; i++) {
    // num 是当前数字
    const num = ints[i]
    // numNeeded 是和当前数字匹配的数字
    const numNeeded = s - num
    // 如果当前数字被记忆过，也就是说有之前出现过的数字和当前数字匹配
    if (mem.has(num)) {
      // 直接返回
      return [
        mem.get(num),
        num
      ]
    }
    // 否则将被需要的数字记忆一下
    mem.set(numNeeded, num)
  }
}
```

```javascript
// 455ms on average
var sum_pairs = function (ints, s) {
  // 用一个对象来记忆出现过数字
  const mem = new Map()
  for (let i of ints) {
    // for of 遍历数组，i是当前数字
    // 如果记忆力有和i相加等于s的数字就直接返回
    if (mem.get(s - i)) return [s - i, +i]
    // 否则把当前数字记忆一下
    mem.set(i, 1)
  }
}
```

一开始的版本测试了几次平均时间为277ms，后来用`for of`改写了一下，平均时间变成了455ms，应该是`for of`的效率稍微比`for in`慢一点的原因。

就这样简单地四行代码就解决了，和一开始的多重遍历对比，不知道高到哪里去了。主要是一开始的思路就是要找到所有符合条件的匹配数字，然后再去找其中的index最小的数，这种做法再数据量小的时候还没关系，当出现测试中这样千万条数据的时候效率就完全没法看了。