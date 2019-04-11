title: 一步一步实现一个 Function.prototype.bind polyfill
date: 2019-04-11 10:14:02
tags:
---

这篇文章的目的是模拟一个 bind 函数，我们知道 JavaScript 中有 call 和 apply 可以用来执行函数并且改变函数内 this 的指向，bind 函数的作用也是类似的，但是 bind 并不会立即执行函数，而是会返回一个新的函数，今天我们就来自己一步一步来实现一个 bind 函数。由于是 polyfill，语法全部使用 ES5。

首先最简单的，实现一下接收一个对象并且返回一个新的函数把 this 指向这个对象。

```javascript
Function.prototype.bind = function (targetThis) {
  // 保存原始函数到 func 变量
  var func = this
  return function () {
    // 通过 apply 改变原始函数的 this 指向为传入的 targetThis
    return func.apply(targetThis)
  }
}

var a = 1
var obj = { a: 100 }
function foo () {
  return this.a
}
foo() // 1
var bar = foo.bind(obj)
bar() // 100
```

这里我们简单实现了 bind，然后接下来我们要让在调用 bind 时能够接受参数，并且 bind 返回的函数还能继续接受参数，最终把所有参数拼接在一起执行。

```javascript
Function.prototype.bind = function (targetThis) {
  // 保存原始函数到 func 变量
  var func = this
  var slice = Array.prototype.slice
  var concat = Array.prototype.concat
  var baseArgs = slice.call(arguments, 1)
  return function () {
    // 通过 apply 改变原始函数的 this 指向为传入的 targetThis
    return func.apply(
      targetThis,
      // 把 bind 时传入的参数和返回函数的参数拼接在一起
      baseArgs.concat(slice.call(arguments))
    )
  }
}

function foo(a, b) {
  console.log(a, b)
}
var bar = foo.bind(null, 1)
bar(2) // 1, 2
```

到这里我们的 bind 函数就实现得差不多了，为了严谨一点，我们需要再加上一些边界条件。

```javascript
Function.prototype.bind = function (targetThis) {
  // 保存原始函数到 func 变量
  var func = this
  var slice = Array.prototype.slice
  var concat = Array.prototype.concat
  var baseArgs = slice.call(arguments, 1)
  if (typeof func !== 'function') {
    // 调用 bind 的原始对象必须是函数，一般情况都是满足条件的，
    // 但是如果某个构造函数继承 new Function，
    // 就可能出现实例非函数的情况
    throw new TypeError('Bind must be called on a function!')
  }
  return function () {
    // 通过 apply 改变原始函数的 this 指向为传入的 targetThis
    return func.apply(
      targetThis,
      // 把 bind 时传入的参数和返回函数的参数拼接在一起
      baseArgs.concat(slice.call(arguments))
    )
  }
}
```

加上条件判断我们的 Function.prototype.bind polyfill 就完成了，在 MDN 上也提供了两种 polyfill，第一种和我们实现的类似，然后还有第二种是可以支持返回的函数当做构造函数使用的，当然这种情况非常少，MDN 上也推荐我们不要这样使用，但是这里我们也可以来实现一下支持 new 关键字。

要支持 new 关键字，主要要实现两个特性：
1. 一是构造函数内的 this 不会受 bind 的 this 影响
2. 二是可以继承原始函数的prototype。

```javascript
Function.prototype.bind = function (targetThis) {
  // 保存原始函数到 func 变量
  var func = this
  var slice = Array.prototype.slice
  var concat = Array.prototype.concat
  var baseArgs = slice.call(arguments, 1)
  // 我们需要一个空函数做中转来让 fBound 继承 func，否则如果直接用 fBound.prototype = func.prototype，
  // 修改 fBound.prototype 的时候会影响到 func.prototype
  var fNOP = function () {}
  if (typeof func !== 'function') {
    // 调用 bind 的原始对象必须是函数，一般情况都是满足条件的，
    // 但是如果某个构造函数继承 new Function，
    // 就可能出现实例非函数的情况
    throw new TypeError('Bind must be called on a function!')
  }
  var fBound = function () {
    // 做构造函数使用时，这里的 this 会指向实例对象，
    // 我们要判断 apply 的参数用 this 或者 targetThis
    return func.apply(
      // 如果 fNOP 在 this 的原型链上，证明 fBound 是做构造函数使用的，应该传递构造函数内的 this
      fNOP.prototype.isPrototypeOf(this) ? this : targetThis,
      // 把 bind 时传入的参数和返回函数的参数拼接在一起
      baseArgs.concat(slice.call(arguments))
    )
  }
  if (this.prototype) {
    // 让 fNOP 可以访问到原始函数的 prototype
    fNOP.prototype = this.prototype;
  }
  // 让 fBound 继承这个空函数，避免修改原型时对原型链造成影响
  fBound.prototype = new fNOP()
  return fBound
}
```