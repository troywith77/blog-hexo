title: JavaScript 学习小结（Five）
date: 2015-06-18 11:14:15
tags:
---
## JavaScript 对象
### 所有事物都是对象
JavaScript 提供多个内建对象，比如 String、Date、Array 等等。此外，JavaScript 允许自定义对象。 对象只是带有属性和方法的特殊数据类型。

 - 布尔型可以是一个对象。
 - 数字型可以是一个对象。
 - 字符串也可以是一个对象
 - 日期是一个对象
 - 数学和正则表达式也是对象
 - 数组是一个对象
 - 甚至函数也可以是对象

### 访问对象的属性
访问对象属性的语法是：

```
objectName.propertyName
```

```
var message="Hello World!";
var x=message.length;      //12
//使用了 String 对象的 length 属性来获取字符串的长度
```
### 访问对象的方法
可以通过以下语法来调用方法：

```
objectName.methodName()
```

```
var message="Hello world!";
var x=message.toUpperCase();   //HELLO WORLD!  
//使用了 String 对象的 toUpperCase() 方法将字符串转换成大写形式
```
### 创建 JavaScript 对象
创建新对象有两种不同的方法：

- 定义并创建对象的实例
- 使用函数来定义对象，然后创建新的对象实例

<!--more-->

#### 创建直接的实例

```
person=new Object();
person.firstname="Rui";
person.lastname="Tang";
person.age=20;
person.eyecolor="black";
```

```
//或者：!!!直接声明，不需要var或者new之类的关键字
person={firstname:"Rui",
lastname:"Tang",
age:20,
eyecolor:"black"};
```
#### 使用对象构造器

```
function person(firstname,lastname,age,eyecolor)
{
this.firstname=firstname;
this.lastname=lastname;
this.age=age;
this.eyecolor=eyecolor;
}
```
在JavaScript中，this通常指向的是我们正在执行的函数本身，或者是指向该函数所属的对象（运行时）

### 创建 JavaScript 对象实例
一旦有了对象构造器，就可以创建新的对象实例，就像这样：

!!!一定要创建新的实例才可以，不能直接person("Rui","Tang",20),这样是没用的!!!

```
var me = new person("Rui","Tang",20);
var myFather=new person("John","Doe",50,"blue");
var myMother=new person("Sally","Rally",48,"green");
```
### 把属性添加到 JavaScript 对象
可以通过为对象赋值，向已有对象添加新属性：

假设 person 对象已存在 - 可以为其添加这些新属性：firstname、lastname、age 以及 eyecolor：

```
person.firstname="John";
person.lastname="Doe";
person.age=30;
person.eyecolor="blue";

x=person.firstname;    //John
```
### 把方法添加到 JavaScript 对象

```
function person(firstname,lastname,age,eyecolor)
{
this.firstname=firstname;
this.lastname=lastname;
this.age=age;
this.eyecolor=eyecolor;

this.changeName=function (name)
{
this.lastname=name;
}
}
```
### JavaScript 类
JavaScript 是面向对象的语言，但 JavaScript 不使用类。

在 JavaScript 中，不会创建类，也不会通过类来创建对象（就像在其他面向对象的语言中那样）。

JavaScript 基于 prototype，而不是基于类的。

### JavaScript for...in 循环
JavaScript for...in 语句循环遍历对象的属性。 for...in 循环中的代码块将针对每个属性执行一次。

语法：

```
for (variable in object)
  {
  code to be executed
  }
```

循环遍历对象的属性：
```
var person={fname:"John",lname:"Doe",age:25}; 
var x,txt = "";

for (x in person)
  {
  txt=txt + person[x];
  }
```
