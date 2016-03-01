title: JavaScript 学习小结（二）
date: 2015-06-01 22:47:55
tags:
---
## JavaScript 错误 - throw、try 和 catch
try 语句测试代码块的错误。（运行代码）

catch 语句处理错误。（发生错误时运行的代码）

throw 语句创建自定义错误。（创建或者抛出异常）

如果把 throw 与 try 和 catch 一起使用，就能够控制程序流，并生成自定义的错误消息。
```
<script>
function myFunction()
{
try
  { 
  var x=document.getElementById("demo").value;
  if(x=="")    throw "empty";
  if(isNaN(x)) throw "not a number";
  if(x>10)     throw "too high";
  if(x<5)      throw "too low";
  }
catch(err)
  {
  var y=document.getElementById("mess");
  y.innerHTML="Error: " + err + ".";
  }
}
</script>

<h1>My First JavaScript</h1>
<p>Please input a number between 5 and 10:</p>
<input id="demo" type="text">
<button type="button" onclick="myFunction()">Test Input</button>
<p id="mess"></p>
```
## 表单验证

```
<script>
function validateForm()
{
var x=document.forms["myForm"]["fname"].value;  //选择forms对象的[form的"name"][input的"name"]
if (x==null || x=="")
  {
  alert("姓必须填写");
  return false;
  }
}
</script>
</head>

<body>
<form name="myForm" action="demo-form.php" onsubmit="return validateForm()" method="post">
姓: <input type="text" name="fname">
<input type="submit" value="提交">
</form>
```

<!--more-->

## E-mail 验证
输入的数据必须包含 @ 符号和点号(.)。同时，@ 不可以是邮件地址的首字符，并且 @ 之后需有至少一个点号：

```
<script>
function validateForm()
{
var x=document.forms["myForm"]["email"].value;
var atpos=x.indexOf("@");
var dotpos=x.lastIndexOf(".");
if (atpos<1 || dotpos<atpos+2 || dotpos+2>=x.length)
  {
  alert("Not a valid e-mail address");
  return false;
  }
}
</script>
</head>

<body>
<form name="myForm" action="demo-form.php" onsubmit="return validateForm();" method="post">
Email: <input type="text" name="email">
<input type="submit" value="Submit">
</form>
```
## JSON
JSON 是用于存储和传输数据的格式，通常用于服务端向网页传递数据 。

JSON 格式在语法上与创建 JavaScript 对象代码是相同的。由于它们很相似，所以 JavaScript 程序可以很容易的将 JSON 数据转换为 JavaScript 对象。
### JSON 语法规则
- 数据为 键/值 对。
- 数据由逗号分隔。
- 大括号保存对象
- 方括号保存数组

```
<p id="demo"></p>

<script>
var text = '{"employees":[' +      //创建 JavaScript 字符串，字符传为 JSON 格式的数据(不知道"+"有什么用)
'{"firstName":"John","lastName":"Doe" },' +
'{"firstName":"Anna","lastName":"Smith" },' +
'{"firstName":"Peter","lastName":"Jones" }]}';

obj = JSON.parse(text);    //JSON.parse() 将字符串转换为 JavaScript 对象
document.getElementById("demo").innerHTML =
obj.employees[1].firstName + " " + obj.employees[1].lastName;
</script>
```
eval() 函数使用的是 JavaScript 编译器，可解析 JSON 文本，然后生成 JavaScript 对象。必须把文本包围在括号中，这样才能避免语法错误：
```
var object1 = eval("{}");        //抛出错误
var object2 = eval(" ({}) ");        // 没问题
var object3 = eval( "(" + jsonText + ")" );        // 通用的解决方案
```
