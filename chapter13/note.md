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
...
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
通过HTML特性指定的事件处理程序，变量`event`保存着`event`对象
```
<input type="button" value="Click Me" onclick="alert(event.type)">
```

`event`事件对象包含的属性方法:

| 属性/方法 | 类型 |  读/写 | 说明 |
| - | -| - | - |
| bubbles | Boolean | 只读 | 表明事件是否冒泡 |
| cancelable | Boolean | 只读 | 表明是否可以取消事件的默认行为 |
| currentTarget | Element | 只读 | 其事件处理程序当前正在处理事件的那个元素 |
| defaultPrevented | Boolean | 只读 | 为true表示已经调用了preventDefault()(DOM3级事件中新增) |
| detail | Integer | 只读 | 与事件相关的细节信息 |
| eventPhase | Integer | 只读 | 调用事件处理程序的阶段:1 表示捕获阶段 2 表示处于目标阶段 3表示冒泡阶段 |
| preventDefault() | Function | 只读 | 取消事件的默认行为。如果cancelable是true。则可以使用这个方法 |
| stopImmediatePropagation() | Function | 只读 | 取消事件的进一步捕获或者冒泡，同时阻止任何事件处理程序被调用(DOM3级事件中新增) |
| stopPropagation() | Function | 只读 | 取消事件的进一步捕获或冒泡。如果bubble为true，则可以使用这个方法 |
| target | Element | 只读 | 事件的目标 |
| trusted | Boolean | 只读 | 为true表示事件是浏览器生成的。为false表示事件是由开发人员通过JavaScript创建的(DOM3新增) |
| type | String | 只读 | 被触发的事件的类型 |
| view | AbstractView | 只读 | 与事件关联的抽象视图。等同于发生事件的Window对象 |

**事件处理程序内部，对象`this`始终等于`currentTarget`，而`target`则只包含事件的实际目标。**
如果直接将事件处理程序指定给力目标元素，则`this`，`currentTarget`，`target`包含相同的值：
```
var btn=document.getElementById("myBtn");
    btn.onclick=function(event){
                alert(event.currentTarget===this);//true
                alert(event.target===this); //true
    }
```
由于是按钮点击触发点击事件。所以`this`指向按钮,`currentTarget`指向按钮,事件目标`target`也是按钮
如果事件处理函数在按钮的父节点:
```
document.body.onclick=function(event){
            alert(event.currentTarget===document.body);//true
            alert(this===document.body);//true;
            alert(event.target===document.getElementById("myBtn")); //true
}
```
`this`和`currentTarget`都指向事件处理程序绑定的目标body上，但是实际点击事件是按钮触发的，所以`target`指向的是按钮
如果通过一个函数处理多个事件，可以使用`type`属性:
```
var btn=document.getElementById("myBtn");
var handler=function(event){
            switch(event.type){
                        case "click":
                         alert("Clicked");
                         break;
                         case "mouseover":
                         event.target.style.backgroundColor="red";
                         break;
                         case "mouseout":
                         event.target.style.backgroundColor="";
                         break;

            }
}； 

btn.onclick=handler;
btn.onmouseover=handler;
btn.onmouseout=handler;
```
以上代码针对事件的不同类型(`event.type`)，鼠标点击，鼠标移入经过，以及鼠标的离开，分别对事件对象作出相应改变。
**如果想阻止事件的特定默认行为，可以在事件的`cancelable`为`true`的时候，使用`preventDefault`属性**
```
var link=document.getElementById("myLink");
link.onclick=function(event){
            event.preventDefault();
}
```
```
avr btn=document.getElementById("myBtn");
btn.onclick=function(event){
            alert("Clicked");
            event.stopPropagation();
};

document.body.onclick=function(event){
            alert("Body clicked");
}
```
body监听点击事件，但是由于按钮阻止了事件的传播。
事件对象的`eventPhase`属性，分别用数字`1,2,3`来确定事件当前处于事件流的哪个阶段（捕获，目标对象，冒泡？）
```
var btn=document.getElemetById("myBtn");
btn.onclick=function(event){
            alert(event.eventPhase); // 2，事件目标对象上
}；

document.body.addEventListener("click",function(event){
            alert(event.eventPhase); //1,  监听捕获阶段
    },true);

document.body.onclick=function(event){
            alert(event.eventPhase);//3 ,事件冒泡阶段
}

```
**`event`对象只在事件处理程序执行期间存在，事件处理执行完毕之后就`event`对象将被销毁**


###　IE中的事件对象
访问IE中的`event`对象有几种不同方式，取决于指定事件处理程序的方法。
```
//1
//DOM0
//window.event 可以直接方法
var btn=document.getElementById("myBtn");
btn.onclick=function(){
        var event=window.event;
            alert(event.type); //click
}

//2
//attachEvent
则有一个`event`对象传入时间处理函数中
var btn=document.getElementById("myBtn");
btn.attacheEvent("onclick",function(event){
                alert(event.type); //"click"
    })

//3
//通过HTML特性指定事件处理程序(通过变量event访问event对象)
<input type="button" value="Click Me" onclick="alert(event.type)">

```
IE的`event`对象包含的属性和方法:

| 属性/方法 | 类型 | 读/写 | 说明 |
| - | - | - | - |
| cancelBubble | Boolean | 读/写 | 默认值是false，设置为true可以取消冒泡（与stopPropagation作用相同）|
| returnValue | Boolean | 读/写 | 默认值为true,设置为false可以取消事件的默认行为（与preventDefault作用相同）|
| srcElement | Element | 只读 | 事件的目标(与target相同) |
| type | String | 只读 | 被触发的事件类型 |

由于事件处理程序的作用域是根据指定它的方式来确定的，所以`this`不一定等于事件目标。`event.srcElement`始终指向事件目标


### 跨浏览器的事件对象
```
var EventUtil ={
        addHandler : function(element,type,handler){
             //省略
        },
        getEvent:function(event){
                return event?event :window.event;
        },
       getTarget :function(event){
                    return event.target||event.srcElement;
       },
       preventDefault:function(event){
                    if(event.preventDefault){
                            event.preventDefault();
                    }else {
                            event.returnValue=false;
                    }
       }
    
        stopPropagation:function(event){
                if(event.stopPropagation){
                        event.stopPropagation();
                }else{
                        event.cancelBubble=true;
                }
        }
    

}
```

## 事件类型
DOM3 事件
>UI(User Interface) 事件，用户与页面上的元素交互时发生
焦点事件，当元素获得或失去焦点时触发
鼠标事件，用户通过鼠标在页面上执行操作时触发
滚轮事件，使用鼠标滚轮（或类似设备）时触发
文本事件，在文档中输入文本时触发
键盘事件，用户通过键盘在页面上执行操作时触发
合成事件，当为IME（Input Method Editor输入法编辑器）输入字符时触发
变动（mutation）事件，当底层DOM结构发生变化时触发
变动名称事件，当元素或属性名变动时触发。（已经废弃）

###　UI事件
不一定是与用户操作有关的事件
>`DOMActive` 表示元素已经被用户操作（通过鼠标或键盘）激活。DOM3被废弃
`load`：当页面完全加载后在`window`上面触发，当所有的框架都加载完毕时在框架集上面触发，当图片加载完毕时在`<img>`元素上面触发，或者当嵌入的内容加载完毕时在`<object>`元素上面触发
`unload`：当页面完全卸载后在`window`上面触发，当所有的框架都卸载后在框架集上面触发，或者当嵌入的内容卸载完毕后`<object>`元素上面触发。
`abort`：在用户停止下载过程时，如果嵌入的内容没有加载完，则`<object>`元素上面触发。
`error`： 当发生JavaScript错误时在Window上面触发，当无法加载图像时`<img>` 元素上面触发，当无法加载嵌入内容时在`<object>`元素上面触发
`select`：当用户选择文本框（`<input>`或者`<textarea>`）中的一个或者多个字符时触发。
`resize`：当窗口或者框架的大小发生变化是在window或者框架集上触发
`scroll`：用户滚动带滚动条的元素中的内容时，在该元素上面触发。`<body>`元素中包含所加载页面的滚动条    

1. load 事件
页面加载完后（包括图像，JavaScript文件，CSS文件等外部资源）就会在window上触发。定义`onload`事件处理程序的两种方法:
```
//1.
EventUtil.addHandler(window,"load",function(event){
                        alert("Loaded!");
    });


 //2.
 <body onlick="alert('Loaded!')">   
```
2. unload 事件
与`load`事件对应，在文档被完全卸载之后触发。用户从一个页面切换到另外一个页面就触发
3. resize 事件
在`window`上面触发。由于浏览器触发resize机制不同，因此不应该在该事件内加入大量的计算。
4. scroll 事件
在`window`上面触发，但实际表示的是页面中相应元素的变化
```
EventUtil.addHandler(window,"scroll",function(event){
            if(document.compatMode=="CSS1Compat"){
                        alert(document.documentElement.scrollTop);
            }else{
                        alert(document.body.scrollTop);
            }
    });
```

### 焦点事件
在页面元素失去或者获得焦点时触发。与`document.hasFocus()`方法和`document.activeElement`属性配合可以知晓用户行踪
>`blur`：元素失去焦点时触发，这个事件不冒泡
`DOMFocusIn`：元素获得焦点时触发，与HTML的`focus`事件等价，冒泡。DOM3废弃
`DOMFocusOut`：元素失去焦点时触发，与HTML的`blur`等价，DOM3废弃
`focus`：元素获得焦点时触发，不会冒泡
`focusin`：元素获得焦点时触发，会冒泡，与HTML的`focus`等价
`focusout`：元素失去焦点时触发，与HTML的`blur`等价

**当焦点从页面的一个元素移到另外一个元素会依次触发以下事件:**
1.`focusout` 在失去焦点的元素上触发
2. `focusin` 在获得焦点的元素上触发
3.  `blur` 在失去焦点的元素上触发
4.  `DOMFocusOut` 在失去焦点的元素上触发
5.  `focus` 在获得焦点的元素上触发
6.  `DOMFocusIn` 在获得焦点的元素上触发



###　鼠标与滚轮事件
DOM3　９个鼠标事件：
>`click`，用户单击鼠标，或者按下后回车。
`dbclick`，用户双击鼠标
`mousedown`，用户按下任意鼠标按钮，不能通过键盘触发
`mouseenter`，鼠标光标从元素外部首次移动到元素范围内时触发。该事件不冒泡，而且光标移动到后代元素上不触发。
`mouseleave`，在元素上方的鼠标光标移到到元素范围之外时触发。该事件不冒泡，而且光标移到到后代元素不触发。
`mousemove`，鼠标指针在元素内部移动时重复触发.
`mouseout`，鼠标指针位于一个元素上方，用户将其移入另外一个元素时触发。又移入的另外一个元素可能位于前一个元素的外部，也可能是这个元素的子元素
`mouseover`，鼠标指针位于一个元素外部，然后用户将其首次移入另外一个元素边界之内时触发.
`mouseup`，用户释放鼠标按钮时触发


`mouseenter`和`mouseleave`不冒泡，其他鼠标事件都冒泡


1. 客户区坐标位置
鼠标事件在浏览器视口中的位置信息保存在事件对象的`clientX`和`clientY`属性中
```
//距离视口左边距离
event.clientX 
//距离视口顶部距离
event.clientY
```
**`clientX`和`clientY`可以知道鼠标在视口(viewpoint)的位置**

2. 页面左边位置
**页面坐标通过事件对象的`pageX`和`pageY`可以知道鼠标在页面的哪个位置**

在页面没有滚动的情况下`pageX`和`pageY`与`clientX`和`clientY`相等


3.  屏幕坐标位置
`screenX`和`screenY`分别表示相对屏幕的位置

4. 修改键
```
var div=document.getElementById("myDiv");
EventUtil.addHandler(div,"click",function(event){
                    event=EventUtil.getEvent(event);
                    var keys=new Array();

                    if(event.shiftKey){
                            keys.push("shift");
                    }

                    if(event.ctrKey){
                            keys.push("ctrl");
                    }

                    if(event.altKey){
                              keys.push("alt");  
                    }

                    if(event.metaKey){
                                keys.push("meta");
                    }
                alert("keys:"+keys.join(","));    
    });
```

5. 相关元素
DOM的事件对象属性`event.relatedTarget`提供事件对象相关元素信息，只对`mouseover`和`mouseout`事件有效

6. 鼠标按钮
DOM的button属性值`0`代表鼠标左键，`1`代表中间键（滚轮），`2`代表右键

7. 更多的事件信息
`event.detail` 给出事件的更多信息，对鼠标事件而言给出的是点击的次数

8. 鼠标滚轮事件
`mousewheel` 可以在页面任何元素上触发，最终冒泡到`document`(IE8)或`window`(IE9，及其他)
**`mousewheel`事件对象包含属性`wheelDelta`，向前滚动的时候wheelDelta是120的倍数，往后滚动则是-120的倍数**


9. 触摸设备
不支持`dbclick`事件，双击会导致窗口放大画面
轻击可单击元素触发`mousemove`事件，如果内容发生变化，则不会再发生其他事件，如果屏幕没有变化，则依次发生`mousedown`,`mouseup`和`click`事件
轻击不可单击元素不会触发任何事件。
`mousemove`事件也会触发`mouseover`和`mouseout`事件
两根手指在屏幕上移动会触发`mousewheel`和`scroll`事件

### 键盘与文本事件
`keydown` 键盘任意键按下的时候触发，如果按住不放，事件重复触发
`keypress` 键盘上的字符键被按下时触发，如果按住不放，事件重复触发
`keyup` 用户释放键盘上的键的时候发生
