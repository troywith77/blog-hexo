title: JavaScript DOM 编程艺术 第六章
date: 2015-03-26 20:31:52
tags:
---
```
function prepareGallery(){
    if(!document.getElementById||!document.getElementsByTagName||!document.getElementById("imagegallery")) return false;
    //检测浏览器是否支持以上方法及id为"imagegallery"的对象是否存在
    var gallery = document.getElementById("imagegallery");
    //将id为"imagegallery"的对象赋给变量 gallery
    var links = gallery.getElementsByTagName("a");
    //将gallery里的所有a元素赋给links变量
    for (var i = 0; i < links.length; i ++){
        //遍历links里的元素
        links.onclick = function(){
            return showPic(this) ? false : true;
            //像links里的元素添加onclick事件
            //如果showPic函数返回true则onclick事件为false，浏览器检测不到点击事件
            //就不会打开新链接，反之亦然
        }
    }
}
function showPic(whichpic){
    if(!document.getElementById("placeholder")) return false;
    //检查“placeholder”是否存在
    var source = whichpic.getAttribute("href");
    //获取运行showPic函数的元素的"href"值并将其赋给source变量
    var placeholder = document.getElementById("placeholder");
    //获取id为"placeholder"的对象并将其赋给placeholder变量
    if(placeholder.nodeName != "IMG") return false;
    //检查placeholder的节点名称是不是"IMG"(大写)，检查它是不是img元素
    placeholder.setAttribute("src", source);
    //设置placeholder的"src"属性为source变量的值
    if(document.getElementById("description")){
        //如果存在id为"description"的对象的话，就运行一下代码
        var text = whichpic.getAttribute("title") ? whichpic.getAttribute("title") : "";
        //如果存在"title"属性的话就将值赋给text，如果没有就将空字符串赋给text
        var description = document.getElementById("description");
        if(description.firstChild.nodeType == 3){
            description.fitstChild.nodeValue = text;
            //将description的第一个子元素(即文本节点)的节点值设置为text的值
        }
    }
    return true;
}
```

<!--more-->

### DOM Core 和 HTML-DOM
在上面用到了以下几种DOM方法：
- getElementById
- getElementsByTagName
- getAttribute
- setAttribute

这些方法都是DOM Core的组成部分。它们并不专属于JavaScript，支持DOM的任何一种程序设计语言都可以使用它们。
<br><br>
HTML-DOM提供了许多描述各种HTML元素的属性。比如：

```
element.getAttribute("src")
//简化为
element.src
```

又比如说，HTML-DOM提供了一个body对象，可以把：

```
document.getElementsByTagName("body")[0]
//简化为
document.body
```
