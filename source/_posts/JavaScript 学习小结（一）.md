title: JavaScript 学习小结（One）
date: 2015-05-24 21:28:11
tags:
---
- innerHTML获取（改变）的是元素的内容加上标签。不能获取或改变元素最外层的标签;
(不知道是不是我弄错了，感觉应该要可以改才对，或许有别的标签捏？)
```
<h1>I love <em>JavaScript</em></h1>
alert(document.getElementsByTagName("h1")[0].innerHTML) 
//I love <em>JavaScript</em>
```
- 使用childNodes.nodeValue来更改文本节点的值时需要先选择文本节点：
```
<p id="demo">点击<mark>下面</mark>的按钮来显示今天是<em>周几</em></p>
var para = document.getElementById("demo");
document.write(para.firstChild.nodeValue);  //点击
document.write(para.lastChild.nodeValue);  //null
document.write(para.childNodes[2].nodeValue);  //的按钮来显示今天是
```

- 改变CSS的方法是：element.style.property = "value" ;
- isNaN()方法判断一个值是否是数字
```
<input type="text" onclick="what()">
function what(){
    var x = document.getElementById("demo").value
    if(x == ""||isNaN(x)){
        alert("not a numebr");
    }
}
```
- 文档加载完成后再使用document.write整个文档会被覆盖（不建议使用）;
- 数字 + 字符 会将数字转换成字符后连接起来;
```
10 + "10"  //"1010"
```
- 创建元素节点、文本节点并将它们各自插入到父元素中
```
<body>
<h1>我的 Web 页面</h1>
<p id="demo">一个段落。</p>
<div id="myDIV">一个 DIV。</div>
<button onclick="answer()">Answer</button>
<script>
document.getElementById("demo").innerHTML="你好 Troy";
document.getElementById("myDIV").innerHTML="你最近怎么样?";
function answer(){
var pp = document.createElement("p");
var txt = document.createTextNode("I'm fine~~");
pp.appendChild(txt);
document.getElementsByTagName("body")[0].appendChild(pp);
}
</script>
</body>
```
- break 跳出循环， continue 跳出本次循环开始下一次循环；
- JavaScript 代码中字符串内可以使用 / 换行；
- match()方法可以检查是否匹配，可以传入字符串，也可以用正则匹配
- 定义数组是var colors = [ "red", "blue", "maroon" ],定义对象是var person = { firstName: "Rui", lastName: "Tang", age: "20" };调用对象属性是 person.age 或者 person["age"]
```
var person = { fullName: function(){...}}//定义数组的方法
person.fullName() //调用方法，如onclick = person.fullName();
```
- 如果把值赋给尚未声明的变量，该变量将被自动作为全局变量声明。
- 如果在函数内直接给全局变量赋值将会覆盖全局变量的值。
- #####字符串方法

Method | 描述
---|---
charAt()|	返回指定索引位置的字符
charCodeAt()|	返回指定索引位置字符的 Unicode 值
concat()|	连接两个或多个字符串，返回连接后的字符串
fromCharCode()|	将字符转换为 Unicode 值
indexOf()|	返回字符串中检索指定字符第一次出现的位置
lastIndexOf()|	返回字符串中检索指定字符最后一次出现的位置
localeCompare()	|用本地特定的顺序来比较两个字符串
match()|	找到一个或多个正则表达式的匹配
replace()|	替换与正则表达式匹配的子串
search()|	检索与正则表达式相匹配的值
slice()	|提取字符串的片断，并在新的字符串中返回被提取的部分
split()|	把字符串分割为子字符串数组
substr()|	从起始索引号提取字符串中指定数目的字符
substring()	|提取字符串中两个指定的索引号之间的字符
toLocaleLowerCase()	|根据主机的语言环境把字符串转换为小写，只有几种语言（如土耳其语）具有地方特有的大小写映射
toLocaleUpperCase()	|根据主机的语言环境把字符串转换为大写，只有几种语言（如土耳其语）具有地方特有的大小写映射
toLowerCase()|	把字符串转换为小写
toString()|	返回字符串对象值
toUpperCase()|	把字符串转换为大写
trim()|	移除字符串首尾空白
valueOf()|	返回某个字符串对象的原始值

- Switch语句：工作原理，首先设置表达式 n（通常是一个变量）。随后表达式的值会与结构中的每个 case 的值做比较。如果存在匹配，则与该 case 关联的代码块会被执行。请使用 break 来阻止代码自动地向下一个 case 运行。
```
switch(n)
{
case 1:
  执行代码块 1
break;
case 2:
  执行代码块 2
break;
default:
 n 与 case 1 和 case 2 不同时执行的代码
}
```
- 数字使用 .toString( ) 方法需要添加括号； String( )
#####将字符串转换为数字
```
Number("3.14")    // 返回 3.14
Number(" ")       // 返回 0 
Number("")        // 返回 0
Number("99 88")   // 返回 NaN
```
- Date()对象

方法 | 描述
---|---
getDate()|	从 Date 对象返回一个月中的某一天 (1 ~ 31)。
getDay()|	从 Date 对象返回一周中的某一天 (0 ~ 6)。
getFullYear()|	从 Date 对象以四位数字返回年份。
getHours()|	返回 Date 对象的小时 (0 ~ 23)。
getMilliseconds()|	返回 Date 对象的毫秒(0 ~ 999)。
getMinutes()|	返回 Date 对象的分钟 (0 ~ 59)。
getMonth()|	从 Date 对象返回月份 (0 ~ 11)。
getSeconds()	|返回 Date 对象的秒数 (0 ~ 59)。
getTime()|	返回 1970 年 1 月 1 日至今的毫秒数。

- 运算符 + 可以把字符串转换为数字，如果字符串里的内容不是数字的话，那么字符串仍会被转换，但值为NaN
#####自动转换类型 Type Conversion<br>
当 JavaScript 尝试操作一个 "错误" 的数据类型时，会自动转换为 "正确" 的数据类型。
```
5 + null    // 返回 5         because null is converted to 0
"5" + null  // 返回"5null"   because null is converted to "null"
"5" + 1     // 返回 "51"      because 1 is converted to "1"  
"5" - 1     // 返回 4         because "5" is converted to 5
```
