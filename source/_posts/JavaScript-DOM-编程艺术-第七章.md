title: JavaScript DOM 编程艺术 第七章 动态创建标记
date: 2015-04-01 20:11:57
tags:
---
### 一些传统方法
- ####document.write
- ####innerHTML

### DOM 方法
- createElement方法：创建一个元素节点

```
document.createElement(nodeName)
document.createElement("p");
```
- appendChild方法：将节点插入另一个节点内

```
parent.appendChild(child)
var para = document.createElement("p");
var testdiv = document.getElementById("testdiv");
testdiv.appendChild(para);
```
- createTextNode：创建一个文本节点

```
document.createTextNode(text)
var txt = document.createTextNode("Hello World");
para.appendChild(txt);
```

- 示例

```
<p>This is <em>my</em> content.</p>
```

<!--more-->

要创建这样的HTML内容，需要：

```
window.onload = function(){
    var para = document.createElement("p");
    var txt1 = document.createTextNode("This is ");
    var emphasis = document.createElement("em");
    var txt2 = document.createTextNode("my");
    var txt3 = document.createTextNode(" content.");
    para.appendChild(txt1);
    emphasis.appendChild(txt2);
    para.appendChild(emphasis);
    para.appendChild(txt3);
    var testdiv ＝ document.getElementById("testdiv");
    testdiv.appendChild(para);
}
```
#### 在已有元素前插入一个新元素
DOM 提供了名为 insertBefore()的方法，这个方法把一个新元素插入到一个现有元素的前面。调用此方法，需要传入：
1. 新元素：你想插入的元素(newElement)
2. 目标元素：你想把这个新元素插入到哪个元素(targetElement)之前
3. 父元素：目标元素的父元素(parentElement)

```
parentElement.insertBefore(newElement, targetElement)
```

我们不必知道父元素到底是哪个，因为targetElement 的parentNode 属性值就是它。在DOM里，元素节点的父元素必须是另一个元素节点(属性节点和文本节点的子元素不允许是元素节点)。

```
var gallery = document.getElementById("imagegallery");
gallery.parentNode.insertBefore(placeholder, gallery);
```
#### 在已有元素后面插入一个新元素
DOM没有提供这样的方法，但是我们可以自己编写这样一个insertAfter函数：

```
function(newElement, targetElement){
    var parent = targetElement.parentNode;
    if (targetElement == parent.lastChild){
        parent.appendChild(newElement);
    }
    //如果目标元素是父元素的最后一个子元素
    //就直接把新元素添加给父元素
    else{
        parent.insertBefore(newElement, targetElement.nextSibling);
    }
    //如果不是，就将新元素插入到目标元素的下一个兄弟元素之前
}
```
### Ajax
#### XMLHttpRequest对象
这个对象充当着浏览器中的脚本(客户端)与服务器之间的中间人的角色。以往的请求都由浏览器发出，而 JavaScript 通过这个对象可以自己发送请求，同时也自己处理响应。
(暂时没看懂。。。以后再单独看Ajax的书籍吧)