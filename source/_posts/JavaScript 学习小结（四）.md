title: JavaScript 学习小结（Four）
date: 2015-06-10 12:20:40
tags:
---
# HTML DOM
## 改变 HTML 内容

```
document.getElementById(id).innerHTML=new HTML
```
## 改变 HTML 属性

```
//HTML DOM方法是
document.getElementById(id).attribute=new value
//DOM方法是
document.getElementById(id);
setAttribute(attribute, value);
```
## 改变 HTML 样式

```
document.getElementById(id).style.property=new style
```
## 对事件做出反应
### onclick事件

```
onclick = JavaScript     //添加事件

<button onclick="displayDate()">Try it</button>       //直接在标签内分配事件

document.getElementById("myBtn").onclick=function(){displayDate()};     //使用DOM来分配事件
```
### onload 和 onunload 事件
onload 和 onunload 事件会在用户进入或离开页面时被触发。

onload 事件可用于检测访问者的浏览器类型和浏览器版本，并基于这些信息来加载网页的正确版本。

onload 和 onunload 事件可用于处理 cookie。

```
<body onload="checkCookies()">      //弹窗-提示浏览器cookie是否可用。
```
### onchange 事件
onchange 事件常结合对输入字段的验证来使用。

```
<input type="text" id="fname" onchange="upperCase()">     
//当用户改变输入字段的内容时，会调用 upperCase() 函数
```
### onmouseover 和 onmouseout 事件
onmouseover 和 onmouseout 事件可用于在用户的鼠标移至 HTML 元素上方或移出元素时触发函数。

### onmousedown、onmouseup 以及 onclick 事件
onmousedown, onmouseup 以及 onclick 构成了鼠标点击事件的所有部分。首先当点击鼠标按钮时，会触发 onmousedown 事件，当释放鼠标按钮时，会触发 onmouseup 事件，最后，当完成鼠标点击时，会触发 onclick 事件。

<!--more-->

## HTML DOM EventListener
addEventListener() 方法用于向指定元素添加事件句柄。addEventListener() 方法添加的事件句柄不会覆盖已存在的事件句柄。

```
element.addEventListener(event, function, useCapture);
```
第一个参数是事件的类型 (如 "click" 或 "mousedown").

第二个参数是事件触发后调用的函数。

第三个参数是个布尔值用于描述事件是冒泡还是捕获。该参数是可选的。
### 向原元素添加事件句柄

```
element.addEventListener("click", function(){ alert("Hello World!"); });
```
### 向同一个元素中添加多个事件句柄

addEventListener() 方法允许向同个元素添加多个事件，且不会覆盖已存在的事件：
```
element.addEventListener("click", myFunction);
element.addEventListener("click", mySecondFunction);
```
可以向同个元素添加不同类型的事件：

```
element.addEventListener("mouseover", myFunction);
element.addEventListener("click", mySecondFunction);
element.addEventListener("mouseout", myThirdFunction);
```
### 向 Window 对象添加事件句柄
当用户重置窗口大小时添加事件监听：

```
window.addEventListener("resize", function(){
    document.getElementById("demo").innerHTML = sometext;
});
```
### 传递参数
当传递参数值时，使用"匿名函数"调用带参数的函数：

```
element.addEventListener("click", function(){ myFunction(p1, p2); });
```
### 事件冒泡与事件捕获
事件传递有两种方式：冒泡与捕获。

事件传递定义了元素事件触发的顺序。 如果你将 < p > 元素插入到 < div > 元素中，用户点击 < p > 元素, 哪个元素的 "click" 事件先被触发呢？

在 冒泡 中，内部元素的事件会先被触发，然后再触发外部元素，即： < p > 元素的点击事件先触发，然后会触发 < div > 元素的点击事件。

在 捕获 中，外部元素的事件会先被触发，然后才会触发内部元素的事件，即： < div > 元素的点击事件先触发 ，然后再触发 < p > 元素的点击事件。

addEventListener() 方法可以指定 "useCapture" 参数来设置传递类型：

```
addEventListener(event, function, useCapture);
```
默认值为 false, 即冒泡传递，当值为 true 时, 事件使用捕获传递。

```
document.getElementById("myDiv").addEventListener("click", myFunction, true);
```
内部外部的元素需要同时设置监听
### removeEventListener() 方法
removeEventListener() 方法移除由 addEventListener() 方法添加的事件句柄:

```
element.removeEventListener("mousemove", myFunction);
```
## HTML DOM 元素的操作
### 添加 HTML 元素

```
<p id="p1">This is a paragraph.</p>
<p id="p2">This is another paragraph.</p>

<script>
var para=document.createElement("p");      //创建一个p元素
var node=document.createTextNode("This is new.");        //创建一个新的文本节点
para.appendChild(node);         //将这个节点追加到p元素上

var element=document.getElementById("p1");       //获取id为p1的元素
element.appendChild(para);          //将创建的p元素追加到id为p1的元素之后
</script>
```
### 删除已有的 HTML 元素

```
<div id="div1">
<p id="p1">This is a paragraph.</p>
<p id="p2">This is another paragraph.</p>
</div>
<script>
var parent=document.getElementById("div1");     //获取div1
var child=document.getElementById("p1");        //获取p1
parent.removeChild(child);                      //将子节点p1从div1中移除
</script>
```
如果要不知道父元素的情况下删除节点，常用的方法是这样的：

```
child.parentNode.removeChild(child);
```
