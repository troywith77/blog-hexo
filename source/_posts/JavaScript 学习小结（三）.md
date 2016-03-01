title: JavaScript 学习小结（Three）
date: 2015-06-07 20:38:10
tags:
---
# 函数
## 定义

```
var functionName = function ( argument ){
    ...
}
function functionName ( argument ){
    ...
}
```
## 函数提升（Hoisting）
提升是 JavaScript 默认将当前作用域提升到前面去的的行为。使用表达式定义函数时无法提升。

函数可以在声明之前调用：

```
myFunction(5);

function myFunction(y) {
    return y * y;
}
```
## 自调用函数
函数表达式可以 "自调用"。如果表达式后面紧跟 () ，则会自动调用。

```
(function () {
    var x = "Hello!!";      // 我将调用自己，say"Hello!!"
})();
```
这是一个匿名自我调用的函数 (没有函数名)。
## 函数是对象
在 JavaScript 中使用 typeof 操作符判断函数类型将返回 "function" 。但，JavaScript 函数描述为一个对象更加准确。JavaScript 函数有属性和方法。

arguments.length 属性返回函数调用过程接收到的参数个数：

```
function myFunction(a, b) {
    return arguments.length;
}
```
toString() 方法将函数作为一个字符串返回:

```
function myFunction(a, b) {
    return a * b;
}

var txt = myFunction.toString();   //返回的是整个函数的字符串形式，而不是结果的字符串形式
```
## 默认参数
如果函数在调用时缺少参数，参数会默认设置为： undefined，有时这是可以接受的，但是建议最好为参数设置一个默认值：

```
function myFunction(x, y) {
    if (y === undefined) {
          y = 0;
    } 
}
function myFunction(x, y) {     //更简单的形式
    y = y || 0;                 //定义了y时就返回y，没有定义时y为undefined，为false，返回0
}
```

<!--more-->

## JavaScript 函数调用
JavaScript 函数有 4 种调用方式，每种方式的不同方式在于 this 的初始化。
### this 关键字
this指向函数执行时的当前对象。
### 作为一个函数调用

```
function myFunction(a, b) {
    return a * b;
}
myFunction(10, 2);           // myFunction(10, 2) 返回 20
```
在浏览器中的页面对象是浏览器窗口(window 对象)。以上函数会自动变为 window 对象的函数。myFunction() 和 window.myFunction() 是一样的：
### 全局对象
当函数没有被自身的对象调用是， this 的值就会变成全局对象。在 web 浏览器中全局对象是浏览器窗口（window 对象）。

```
function myFunction() {
    return this;
}
myFunction();                // 返回 window 对象
```
### 作为方法调用

```
var myObject = {
    firstName:"John",
    lastName: "Doe",
    fullName: function () {
        return this.firstName + " " + this.lastName;
    }
}
myObject.fullName();         // 返回 "John Doe"
```
### 使用构造函数调用函数
如果函数调用前使用了 new 关键字, 则是调用了构造函数。这看起来就像创建了新的函数，但实际上 JavaScript 函数是创建了新的对象：

```
// 构造函数:
function myFunction(arg1, arg2) {
    this.firstName = arg1;
    this.lastName  = arg2;
}

// This creates a new object
var x = new myFunction("John","Doe");
x.firstName;                             // 返回 "John"
```
构造函数的调用会创建一个新的对象。新对象会继承构造函数的属性和方法。
### 作为函数方法调用函数
在 JavaScript 中, 函数是对象。JavaScript 函数有它的属性和方法。call() 和 apply() 是预定义的函数方法。 两个方法可用于调用函数，两个方法的第一个参数必须是对象本身。

```
function myFunction(a, b) {
    return a * b;
}
myFunction.call(myObject, 10, 2);      // 返回 20

function myFunction(a, b) {
    return a * b;
}
myArray = [10,2];
myFunction.apply(myObject, myArray);   // 返回 20
```
两个方法都使用了对象本身作为第一个参数。 两者的区别在于第二个参数： apply传入的是一个参数数组，也就是将多个参数组合成为一个数组传入，而call则作为call的参数传入（从第二个参数开始）。

## 函数闭包

```
function myFunction() {
    var a = 4;
    return a * a;         //16
}

var a = 4;
function myFunction() {
    return a * a;         //16
}
```
后面一个实例中， a 是一个 全局 变量。在web页面中全局变量属于 window 对象。全局变量可应用于页面上的所有脚本。

在第一个实例中， a 是一个 局部 变量。局部变量只能用于定义它函数内部。对于其他的函数或脚本代码是不可用的。

全局和局部变量即便名称相同，它们也是两个不同的变量。修改其中一个，不会影响另一个的值。

*注意*：变量声明是如果不使用 var 关键字，那么它就是一个全局变量，即便它在函数内定义。

### 变量生命周期
全局变量的作用域是全局性的，即在整个JavaScript程序中，全局变量处处都在。

而在函数内部声明的变量，只在函数内部起作用。这些变量是局部变量，作用域是局部性的；函数的参数也是局部性的，只在函数内部起作用。

### JavaScript 内嵌函数
所有函数都能访问全局变量。实际上，在 JavaScript 中，所有函数都能访问它们上一层的作用域。JavaScript 支持嵌套函数。嵌套函数可以访问上一层的函数变量。

```
function add() {
    var counter = 0;
    function plus() {counter += 1;}
    plus();                             // plus() 可以访问父函数的 counter 变量：
    return counter; 
}
```
### 闭包

```
<button type="button" onclick="myFunction()">计数!</button>

<p id="demo">0</p>

<script>
var add = (function () {
    var counter = 0;
    return function () {return counter += 1;}  
})();    /*制造一个伪全局变量的意思吗？让匿名函数访问到上一层函数里的counter,并且不会把counter清零*/
         //自调用函数，匿名函数
function myFunction(){
    document.getElementById("demo").innerHTML = add();
}
</script>
```
闭包是可访问上一层函数作用域里变量的函数，即便上一层函数已经关闭。