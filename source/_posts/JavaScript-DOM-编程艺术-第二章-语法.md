title: JavaScript DOM 编程艺术 第二章 语法
date: 2015-03-06 23:26:31
tags:
---
网上有很多人都推荐看这本书来入门JavaScript，正好当当打折就买了一本这个和一本高级程序设计。倒是这本比高程要薄好多啊啊啊，先看完这本，跟着把实例做完之后再去看"红宝书"吧~

### 语句（statement）

```
first statement;
second statement;
```
建议每条语句独占一行，每条语句末尾加上分号。

### 注释

```
//注释能吃吗
```
注释可以使用两个斜杠开始，后面的内容就是注释，会被JavaScript解释器忽略。
```
//注释能吃吗
  我想吃哇
```
这样是不行的，多行的话每行都需要加两个斜杠。
```
/*  不管能不能吃
    我都要吃掉   */
```
如果要插入大段注释，建议使用这种方法。

<!--more-->

### 变量
提前声明变量是一种良好的编程习惯

```
var mood = "happy"; age = 33;
```
注意：在JavaScript中，变量和其他语法元素都是区分大小写的。<br><br>
命名规则：
- 变量名中不允许包括空格或标点符号（美元符号"$"除外）
- 变量名允许包含字母、数字、美元符号和下划线（但第一个字符不允许是数字）
- 函数名、方法名和对象属性名命名的首选格式是*驼峰格式*（第一个单词小写，后面的单词首字母大写）。

```
var my mood = "happy";    // 语法错误
var my_mood = "happy";    // 下划线连接单词
var myMood  = "happy";    // 驼峰格式
```
### 数据类型
JavaScript是弱类型（weakly typed）语言，意味着可以在任何阶段改变变量的数据类型。
1. 字符串
2. 数值
3. 布尔值

字符串、数值、布尔值都是标量，它们在任意时刻都只能有一个值。

### 数组
数组是指用一个变量表示一个值的集合，集合中的每个值都是这个数组的一个*元素*。

```
var beatles = Array();
```
使用关键字Array声明数组。

```
array[index] = element;
```
向数组中添加元素的操作成为填充。填充时，需要给出新元素的值和它在数组中的存放位置，这个位置就是这个元素的索引。索引从 0 开始。

```
var beatles = Array(4);
beatles[0] = "John";
beatles[1] = "Paul";
beatles[2] = "George";
beatles[3] = "Ringo";
```

下面是相对简单的一种方式：在声明数组的同时对它进行填充。
```
var beatles = Array("John", "Paul", "George", "Ringo");
```
数组元素可以是字符串、布尔值、数值、变量，甚至还可以包括其他数组。
#### 关联数组

```
var lennon = Array();
lennon["name"] = "John";
lennon["year"] = 1940;
lennon["living"] = false;
```
关联数组使用字符串代替数字索引，更有可读性，但是不建议这样使用。上面的例子中实际上给了lennon数组添加了三个属性，但是理想情况下不应该修改Array对象的属性，而应该使用对象（Object）。
### 对象
```
var lennon = Object();
lennon.name = "John";
lennon.year = 1940;
lennon.living = false;
```
与数组类似，对象也使用一个名字表示一组值。对象的每个值都是对象的一个属性，使用点号获取属性。

```
 { propertyName:value, propertyName:value };

 var lennon = { name:"John", year:1940, living:false };
```
另一种创建对象的方法。

### 运算
- 加减乘除(+、-、*、/)
- 求模(%)
- 取余(%)


字符串相加会将其拼接起来，字符串加数字会先将数字转换为字符串然后拼接。

### 条件语句

```
if (condition){
    statements;
}
else｛
    statements；
｝
```
if 语句条件放在圆括号中，可以是表达式或布尔值，只有当条件的求值结果是 true 时花括号里的语句才会执行。

### 比较操作符
- 大于、小于(>、<)
- 大于等于、小于等于(>=、<=)
- 相等、不等(==、!=)
- 全等、全不等(===、!==)


```
var a = false;
var b = "";
if (a == b){
    alert("a equals b");
}                              //这个条件语句的求值结果是 true

var a = false;
var b = "";
if (a === b){
    alert("a equals b");
}                              //这个条件语句的求值结果则是 false
```

相等操作符 == 不同数据类型的值比较时会将它们转换为同一类型然后比较，全等操作符 === 会执行严格的比较。

### 逻辑操作符
- 逻辑与( && ),两个布尔值都为真时才是真
- 逻辑或( || ),有一个布尔值为真时就是真
- 逻辑非( ! ),用于耽搁逻辑操作，取反布尔值

### 循环语句
工作原理：只要给定条件得到满足，循环语句里的代码就会重复执行；直到条件的求值结果不再是 true，循环结束。
#### while循环

```
while (condition){
    statements;
}
```
##### do...while循环
因为while循环中有可能代码一次也不执行，所以当我们希望代码至少执行一次的话，可以使用do...while循环。

```
do {
    statements;
}   while (condition);
```
do...while循环对循环控制条件的求值发生在每次循环结束之后，所以即使条件不为真，也会先执行一次代码。

#### for循环

```
for (initial condition; test condition; alter condition){
    statements;
}

for (var i = 1; i < 11; i ++){
    alert(i);
}
```
for循环的基本形式

```
var beatles = Array("John", "Paul", "George", "Ringo");
for (var i = 0; i < beatles.length; i ++){
    alert(beatles[i]);
}
```
for循环最常用的用途是对某个数组里的全体元素进行遍历处理，这往往需要使用数组的array.length属性，这个属性可以告诉我们指定数组的元素个数。

### 函数

```
function shout(){
    var beatles = Array("John", "Paul", "George", "Ringo");
    for (var i = 0; i < beatles.length; i ++){
    alert(beatles[i]);
}

shout();            //使用这个语句来调用函数
```
如果需要多次使用同一段代码，可以把它们封装成一个函数。函数实际上就是一个小脚本。作为一种良好的编程习惯，应该先对函数做出定义再调用它们。

```
function name(arguments){
    statements;
}
```
定义函数的语法。传递给函数的数据称为参数，可以把不同的数据传递给它们。定义函数时，可以声明多个参数，只需用逗号分隔开。<br>
JavaScript提供许多内建函数，alert()就是一例。

```
function multiply(num1, num2){
    var total = num1 + num2;
    alert(total);
}

multiply(10, 2);
```
传递两个参数的函数，将会弹出显示结果警告框。如果想要将结果返回给调用这个函数的语句，可以使用 return：

```
function multiply(num1, num2){
    var total = num1 + num2;
    return total;
}

function convertToCelsius(temp){
    var result = temp - 32;
    result = result / 1.8;
    return result;
}                   //转换成摄氏温度
```
函数的真正价值体现在，我们可以把它当作一种数据类型来使用，这意味这可以把一个函数的调用结果赋给一个变量。

```
var temp_fahrenheit = 95;
var temp_celsius = convertToCelsius(temp_fahrenheit);
alert(temp_fahrenheit);
```
在这个例子里，变量 temp_celsius 的值将是35，这个数值有 convertToCelsius 函数返回。<br>
命名函数通常使用驼峰格式，变量则使用下划线。
#### 变量的作用域
- 全局变量 可以在脚本中的任何位置被引用，包括函数内部。全局变量的作用域是整个脚本。
- 局部变量 只存在于声明它的函数内部，在那个函数外部无法被引用。局部变量的作用域仅限于某个特定的函数。

可以使用 var 关键字明确地为函数设定作用域。<br>
如果在某个函数中使用了 var，那个变量就被视为一个局部变量，它只存在于这个函数上下文中；反之，如果没有使用 var，那个变量就被视为一个全局变量，如果脚本中已经存在一个与之同名的全局变量，那个函数就会改变那个全局变量的值。

```
function square(num){
    total = num * num;
    return total;
}
var total = 50;
var number = square(20);
alert(total);
```
这些代码将不可避免地导致全局变量 total 的值发生变化。

```
function square(num){
    var total = num * num;
    return total;
}
```

这样才是正确声明局部变量的方法。
### 对象
对象 是一种非常重要的数据类型。对象是自包含的数据集合，包含在对象里的数据可以通过两种形式访问——属性和方法：
- 属性 是隶属于某个特定对象的变量
- 方法 是只有某个特定函数才能调用的函数

对象就是由一些属性和方法组合在一起而构成的一个数据实体。在JavaScript里，属性和方法都使用“点”语法来访问。