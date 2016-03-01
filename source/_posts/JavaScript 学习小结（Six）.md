title: JavaScript 学习小结（Six）
date: 2015-06-20 20:18:45
tags:
---
## Number 对象
JavaScript 不是类型语言。与许多其他编程语言不同，JavaScript 不定义不同类型的数字，比如整数、短、长、浮点等等。

在JavaScript中，数字不分为整数类型和浮点型类型，所有的数字都是由浮点型类型。JavaScript采用IEEE754标准定义的64位浮点格式表示数字，它能表示最大值为±1.7976931348623157 x 10308，最小值为±5 x 10 -324可以使用也可以不使用小数点来书写数字。
### 精度
整数（不使用小数点或指数计数法）最多为 15 位。小数的最大位数是 17，但是浮点运算并不总是 100% 准确：

```
var x = 0.2+0.1; //  0.30000000000000004
//解决办法是
var x = (0.2 * 10 + 0.1 * 10) / 10         //0.3
```
### 八进制和十六进制
如果前缀为 0，则 JavaScript 会把数值常量解释为八进制数，如果前缀为 0 和 "x"，则解释为十六进制数。

默认情况下，JavaScript 数字为十进制显示。但是你可以使用 toString() 方法 输出16进制、8进制、2进制。

```
var myNumber=128;
myNumber.toString(16);   // returns 80
myNumber.toString(8);    // returns 200
myNumber.toString(2);    // returns 10000000
```
### 无穷大（Infinity）
当数字运算结果超过了JavaScript所能表示的数字上限（溢出），结果为一个特殊的无穷大（infinity）值，在JavaScript中以Infinity表示。同样地，当负数的值超过了JavaScript所能表示的负数范围，结果为负无穷大，在JavaScript中以-Infinity表示。无穷大值的行为特性和我们所期望的是一致的：基于它们的加、减、乘和除运算结果还是无穷大（当然还保留它们的正负号）。

```
myNumber=2;
while (myNumber!=Infinity)
{
myNumber=myNumber*myNumber; // Calculate until Infinity
}
```
除以0也产生了无限:

```
var x = 2/0;
var y = -2/0;
```
### NaN - (Not a number)非数字值
NaN 属性是代表非数字值的特殊值。该属性用于指示某个值不是数字。可以把 Number 对象设置为该值，来指示其不是数字值。可以使用 isNaN() 全局函数来判断一个值是否是 NaN 值。

```
var x = 1000 / "Apple";
isNaN(x); // returns true
var y = 100 / "1000";      //0.1
isNaN(y); // returns false
```
除以0是无穷大，无穷大是一个数字:
### 数字可以是数字或者对象
数字可以私有数据进行初始化，就像 x = 123;JavaScript 数字对象初始化数据， var y = new Number(123);

```
var x = 123;
var y = new Number(123);
typeof(x) // returns Number
typeof(y) // returns Object

var x = 123;              
var y = new Number(123);
(x === y) // is false because x is a number and y is an object.
```

<!--more-->

## 字符串（String） 对象
字符串可以使用单引号或双引号：

可以使用索引访问字符串中任何的字符：

字符串的索引从零开始, 所以字符串第一字符为 [0],第二个字符为 [1]

```
var carname="Volvo XC60";
var character=carname[7];  // C
```
### 在字符串中查找字符串
使用 indexOf() 可以来定位字符串中某一个指定的字符首次出现的位置：

```
var str="Hello world, welcome to the universe.";
var n=str.indexOf("welcome");   // 13
```
如果没找到对应的字符函数返回-1。lastIndexOf() 方法在字符串末尾开始查找字符串出现的位置。

### 内容匹配
match()函数用来查找字符串中特定的字符，并且如果找到的话，则返回这个字符。

```
var str="Hello world!";
document.write(str.match("world") + "<br>");  //world
document.write(str.match("World") + "<br>");  //null
document.write(str.match("world!"));    //world!
```
### 替换内容
replace() 方法在字符串中用某些字符替换另一些字符。

```
str="Please visit Microsoft!"
var n=str.replace("Microsoft","w3cschool");
```
### 字符串大小写转换
字符串大小写转换使用函数 toUpperCase() / toLowerCase():

```
var txt="Hello World!";       // String
var txt1=txt.toUpperCase();   // txt1 is txt converted to upper
var txt2=txt.toLowerCase();   // txt2 is txt converted to lower
```
使用这两个方法会返回新的字符串，旧字符串不会改变
### 字符串转为数组
字符串使用strong>split()函数转为数组:

```
txt="a,b,c,d"   // String
txt.split(",");   // Split on commas   ["a","b","c","d"]
txt.split(" ");   // Split on spaces   ["a,b,c,d"]  //没有空格就将整个字符串转为数组里的一个值
txt.split("|");   // Split on pipe     ["a,b,c,d"]  //没有 | 就将整个字符串转为数组里的一个值
txt.split("");    //                   ["a", ",", "b", ",", "c", ",", "d"]
```

### 数组连接成字符串
join() 方法用于把数组中的所有元素放入一个字符串。元素是通过指定的分隔符进行分隔的。

```
arrayObject.join(separator)
```

此方法返回一个字符串。该字符串是通过把 arrayObject 的每个元素转换为字符串，然后把这些字符串连接起来，在两个元素之间插入 separator 字符串而生成的。

```
var arr = new Array(3);
arr[0] = "George";
arr[1] = "John";
arr[2] = "Thomas";
var arr2 = arr.join();
typeof arr; // "object"
typeof arr2;  // "string"
```

### 特殊字符

Javascript 中可以使用反斜线（\）插入特殊符号，如：撇号,引号等其他特殊符号。

代码 | 输出
---|---
\'	| 单引号
\"	| 双引号
\\\	| 斜杆
\n	| 换行
\r	| 回车
\t	| tab 制表符（四个空格）
\b	| 空格
\f	| 换页

## Date（日期） 对象

```
var time=new Date();
time;  //获得当前日期
time.getFullYear(); //年
time.getHours(); //时
time.getMinutes(); //分
time.getSeconds(); //秒 
time.getMonth(); //月
time.getDate();  //日
```

### 创建日期
Date 对象用于处理日期和时间。 可以通过 new 关键词来定义 Date 对象。以下代码定义了名为 myDate 的 Date 对象：

有四种方式初始化日期:

```
new Date() // 当前日期和时间
new Date(milliseconds) //返回从 1970 年 1 月 1 日至今的毫秒数
new Date(dateString)
new Date(year, month, day, hours, minutes, seconds, milliseconds)
var time  = new Date(1995,05,24,12,30,30);  //Sat Jun 24 1995 12:30:40 GMT+0800 (CST)
```

### 设置日期

```
var myDate=new Date();
myDate.setDate(myDate.getDate()+5); //将日期推迟五天
```

如果增加天数会改变月份或者年份，那么日期对象会自动完成这种转换。
### 两个日期比较
日期对象也可用于比较两个日期。

```
var x=new Date();
x.setFullYear(2100,0,14);
var today = new Date();

if (x>today)
  {
  alert("Today is before 14th January 2100");
  }
else
  {
  alert("Today is after 14th January 2100");
  }
```

## Array（数组） 对象
数组对象是使用单独的变量名来存储一系列的值。
### 创建一个数组
创建一个数组，有三种方法。

```
//1: 常规方式:
var myCars=new Array(); 
myCars[0]="Saab";       
myCars[1]="Volvo";
myCars[2]="BMW";
//2: 简洁方式:
var myCars=new Array("Saab","Volvo","BMW");
//3: 字面:
var myCars=["Saab","Volvo","BMW"];
```

### 访问数组
通过指定数组名以及索引可以访问某个特定的元素。

```
var name=myCars[0];

myCars[0]="Opel"; //修改了数组 myCars 的第一个元素
```

### 在一个数组中你可以有不同的对象
所有的JavaScript变量都是对象。数组元素是对象。函数是对象。因此，在数组中可以有不同的变量类型。

```
myArray[0]=Date.now;
myArray[1]=myFunction;
myArray[2]=myCars;
```

### 数组方法和属性
使用数组对象预定义属性和方法：

```
var x=myCars.length             // the number of elements in myCars
var y=myCars.indexOf("Volvo")   // the index position of "Volvo"
```

### Prototype 原型创建新方法
原型是JavaScript全局构造函数。它可以构建新Javascript对象的属性和方法。
Array.prototype.myUcase=function()

```
Array.prototype.myUcase=function()
{
for (i=0;i<this.length;i++)
  {
  this[i]=this[i].toUpperCase();
  }
}

function myFunction()
{
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.myUcase();
var x=document.getElementById("demo");
x.innerHTML=fruits;
}
```

### Array 对象方法
方法 | 描述
---|---
concat()	|连接两个或更多的数组，并返回结果。
indexOf()	|返回某个指定的字符串值在字符串中首次出现的位置。
join()	|把数组的所有元素放入一个字符串。元素通过指定的分隔符进行分隔。
lastIndexOf()	|返回一个指定的字符串值最后出现的位置，在一个字符串中的指定位置从后向前搜索。
pop()	|删除数组的最后一个元素并返回删除的元素
push()	|向数组的末尾添加一个或更多元素，并返回新的长度。
reverse()	|颠倒数组中元素的顺序。
shift()|	删除并返回数组的第一个元素
slice()	|从某个已有的数组返回选定的元素
sort()	|对数组的元素进行排序
splice()	|删除元素，并向数组添加新元素。
toString()	|把数组转换为字符串，并返回结果。
unshift()|	向数组的开头添加一个或更多元素，并返回新的长度。
valueOf()|	返回数组对象的原始值

### Boolean（布尔） 对象
如果布尔对象无初始值或者其值为:

- 0
- -0
- null
- ""
- false
- undefined
- NaN

那么对象的值为 false。否则，其值为 true（即使当自变量为字符串 "false" 时）！

### Math（算数） 对象
使用Math的属性/方法的语法：

```
var x=Math.PI;
var y=Math.sqrt(16);
```

Math对象无需在使用这个对象之前对它进行定义。

```
Math.round()  //四舍五入
Math.ceil()  //向上舍入
Math.floor()  //向下舍入
Math.random()  //返回 0 ~ 1 之间的随机数
Math.sqrt()  //返回数的平方根
```

## RegExp 对象
语法

```
var patt=new RegExp(pattern,modifiers);

or more simply:

var patt=/pattern/modifiers;
```

- 模式描述了一个表达式模型。
- 修饰符描述了检索是否是全局，区分大小写等。

### RegExp 修饰符
修饰符用于执行不区分大小写和全文的搜索。

i - 修饰符是用来执行不区分大小写的匹配。

g - 修饰符是用于执行全文的搜索（而不是在找到第一个就停止查找,而是找到所有的匹配）。

### test()
test()方法搜索字符串指定的值，根据结果并返回真或假。

```
var patt1=new RegExp("e");
document.write(patt1.test("The best things in life are free"));  //true
```

### exec()
exec() 方法检索字符串中的指定值。返回值是被找到的值。如果没有发现匹配，则返回 null。

```
var patt1=new RegExp("e");
document.write(patt1.exec("The best things in life are free")); //e
```
