[TOC]
# 事件

##事件流
描述的是从页面中接收事件的顺序。IE的事件流是冒泡流，Netscape的是捕获事件流

### 事件冒泡
IE的事件流叫事件冒泡(event bubbing),事件开始由最具体的元素（文档中嵌套层次最深的节点）接收，然后逐级向上传播到较为不具体的节点（文档）。

所有浏览器都支持事件冒泡，但实现有差异。现代浏览器冒泡到`Window`对象

### 事件捕获

与事件冒泡相反，不太具体的节点更早接收到事件，然后具体的节点最后接收到事件。意义在于事件到达预定目标之前先捕获它。由于老版本浏览器不支持事件捕获，所以建议使用事件冒泡，特殊情况用事件捕获。


### DOM事件流
**DOM2事件流包括三个阶段：`事件捕获`，`处于目标阶段`和`事件冒泡阶段`.**
首先发生的是事件捕获为截获事件提供机会，然后是实际的目标接收事件，最后一个阶段是冒泡阶段，可以在这个阶段对事件作出响应。

![DOM事件流](https://chaihongjun.github.io/Professional_JS_For_Web_Developers_3rd/chapter13/eventflow.svg)





## 事件处理程序
`事件`：用户或者浏览器自身执行的动作。
响应某个事件的函数叫`事件处理程序`（`事件侦听器`）
事件处理程序的名字以`on`开头:
```
onclick
onload
```

###HTML事件处理程序
某个HTML元素支持的每种事件，都可以使用一个与相应事件处理程序同名的HTML特性来指定，这个特性可以执行JS代码。
```
<input type="button" value="click" onclick="alert('clicked')">
```
`click`事件对应的事件处理程序`onclick`，在HTML元素对应的特性也是`onclick`。
事件处理程序中的代码在执行时，有权访问全局作用域中的代码

### DOM0级事件处理程序
```
//指定事件处理程序
要操作的对象引用.事件处理程序属性=事件函数

var btn=document.getElementById("myBtn");
btn.onclick=function(){
        alert("Clicked");
}

//要操作的对象引用: btn
//事件处理程序属性: onclick
//事件函数 :  function(){alert("Clicked");}
```
该方式添加的事件处理程序会在事件冒泡阶段被处理
```
btn.onclick=null;   //删除事件处理程序
```
**事件处理程序的作用域为所属元素的作用域**

### DOM2级事件处理程序
DOM2级事件定义了两个方法`addEventListener()`和`removeEventListener()`，所有DOM节点都有这两个方法：
```
addEventListener("事件名称","事件处理函数",布尔值);
removeEventListener("事件名称","事件处理函数",布尔值);
var btn=document.getElementById("myBtn");
btn.addEventListener("click",function(){
                        alert(this.id);

    },false);
```
通过`addEventListener()`添加的事件只能通过`removeEventListener()`移除，而且参数必须一致。
**所以通过`addEventListener()`添加的匿名事件函数将无法通过`removeEventListener()`移除**

`addEventListener()` 可以为一个元素添加多个事件，执行顺序则是按照添加顺序而来:
```
//点击按钮之后先弹出按钮ID，再弹出hellow world
var btn=document.getElementById("myBtn");
 btn.addEventListener("click",function(){
                    alert(this.id);
    },false);

     btn.addEventListener("click",function(){
                    alert("hello world!");
    },false);
```





###IE事件处理程序
IE对应的两个方法`attachEvent()`和`detachEvent()`
```
attachEvent("事件处理程序名称","事件处理程序函数")
detachEvent("事件处理程序名称","事件处理程序函数")
```
**这里的是`事件处理程序名称`而非`事件名称`,因此，如果是点击事件，则应该是`onclick`而不是`click`**
**`attachEvent()`的事件处理程序作用域为全局:**
```
var btn=document.getElementById("myBtn");
        btn.attachEvent("onclick",function(){
                    alert(this===window) ;// true
            });
```
**`attachEvent()` 与 `addEventListener`类似，但是执行顺序是相反的，最后添加的，最先执行**
IE事件的移除`detachEvent()`与`removeEventListener()`类似，必须和`attachEvent()`参数一致，才可以移除事件。


### 跨浏览器的事件处理程序
```
var EventUtil ={
                addHandler:function(element,type,handler){
                            if(element.addEventListener){
                                        element.addEventListener(type,handler,false);
                            }else if(element.attachEvent){
                                        element.attachEvent("on"+type,handler);
                            } else {
                                        element["on"+type]=handler;
                            }
                },
                removeHandler:function(element,type,handler){
                                if(element.removeEventListener){
                                            element.removeEventListener(type,handler,false);
                                }else if(element.detachEvent){
                                        element.detachEvent("on"+type,handler);
                            } else {
                                        element["on"+type]=null;
                            }
                }
}
```
首先检测是否支持DOM2级方法，如果不支持可能是IE系列，如果也不支持，则使用DOM0级方法。
```
var btn=document.getElementById("myBtn");
var handler=function(){
        alert("CLicked");
}
EventUtil.addHandler(btn,"click",handler);
EventUtil.removeHandler(btn,"click",handler);
```

##事件对象
触发DOM上的某个事件的时候，会产生一个事件对象`event`，这个对象包含了所有与事件相关的信息，包括导致事件的元素，事件的类型，和其他与特定事件相关的信息。
浏览器都支持`event`对象，但方式不同。

### DOM中的事件对象
兼容DOM的浏览器会将一个`event`传入事件处理程序中，无论是DOM0还是DOM2:
```
var btn=document.getElemetById("myBtn");
btn.onclick=function(){
            alert(event.type);  //"click"
}

btn.addEventListener("click",function(event){
                   alert(event.type);  //"click"
    },false)
```
```
<input type="button" value="Click Me" onclick="alert(event.type)">
```
通过HTML特性指定的事件处理程序，变量`event`保存着`event`对象
