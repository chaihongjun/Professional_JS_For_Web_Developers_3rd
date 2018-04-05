[TOC]

# DOM 扩展
DOM的两个主要扩展Selectors API和HTML5

##  Selectors API 选择符API
核心方法：
`querySelector()`和`querySelectorAll()`
能够调用它们类型包括`Document`，`DocumentFragment`和`Element`


###   querySelector() 方法
接收一个CSS选择符，返回匹配该模式额度第一个元素，如果没匹配则返回null

### querySelectorAll() 方法
接收的也是CSS选择符，但返回的是所有匹配的元素，返回的是一个带有所有属性和方法的NodeList实例
，底层实现类似一组元素的快照，而非不断对文档进行搜索的动态查询.

要取得NodeList中的每一个元素，可以使用`[]`或`item()`方法:
```
var strongs=document.querySelectorAll("p strong");
var i,len,stong;
for (i=0,len=strongs.length;i<len;i++){
            strong=strongs[i]; // 或 strongs.item(i)
            strong.className="important";
}
```
### matchesSelector()方法
Selector API Level 2为Element类型新增matchesSelector()方法:
```
element.matchesSelector()
```
接收一个CSS选择器与调用元素进行匹配，如果匹配返回true,否则false。
目前没有支持这个方法的浏览器，若要使用最好包装:
```
function matchesSelector(element,selector){
            if (element.matchesSelector){
                    return element.matchesSelector(selector);
            }else if(element.msMatchesSelector){
                     return element.msMatchesSelector(selector);
            }else if(element.mozMatchesSelector){
                     return element.mozMatchesSelector(selector);
            }else if(element.webkitMatchesSelector){
                     return element.webkitMatchesSelector(selector);
            } else{
                        throw new Error("Not Supported.");
            }
}

if(matchesSelector(document.body,"body.page1")){
            //。。。。
}
```

## 元素遍历  Element Traversal 规范
Element Traversal API 为DOM新增5个属性
>`childElementCount` 返回子元素（不含文本和注释节点）的个数
    `firstElementChild` 返回第一个子元素
    `lastElementChild` 返回最后一个子元素
    `previousElementSibling` 指向前一个同辈元素
    `nextElementSibing` 指向后一个同辈元素

## HTML5

### 与类相关的扩充
1. getElementsByClassName()方法
接收一个或者多个类名的字符串，返回带有指定类的所有元素的NodeList
2. classList属性
classList属性是新集合类型DOMTokenList的实例，包含元素个数的`length`属性，要取得元素可以使用`[]`或者`item()`方法。
另外还定义了以下方法:

>`add(value)` 将给定的字符串值添加到列表中，如果已经存在就不添加
    `contains(value)` 表示列表中是否存在给定的值，如果存在返回true,否则返回false
    `remove(value)` 从列表中删除给定的字符串
    `toggle(value)` 如果列表存在给定值，则删除；如果列表不存在则添加

###焦点管理
```
//始终引用DOM中当前获得了焦点的元素
document.activeElement
```
```
var button = document.getElementById("myButton");
button.focus(); //获取焦点
alert(document.activeElement ===button); //true
```
文档刚加载完时`document.activeElement`中保持的是`document.body`，文档加载期间`document.activeElement`值为null
`document.hasFocus()`方法可以判断文档是否获得焦点

###HTMLDocument的变化
1. readyState 属性
```
//两个可能值
loading ,正在加载文档
complete ，已经加载完成

if(document.readyState=="complete"){
        //文档加载完毕执行的操作
}


```
2. 兼容模式
`document.compatMode`的值为`CSS1Compat`则是**标准模式**,值为`BackCompat`是**混杂模式**
3. head属性
`document.head`属性引用文档的`<head>`元素
 兼容性写法 `var head = document.head || document.getElementsByTagName("head")[0];`   


### 字符集属性
`charset`属性表示文档中实际使用的字符集，默认值为"utf-16"，可以通过`<meta>`或者响应头或者直接设置来修改。
另外一个属性`defaultCharset`表示根据默认浏览器及操作系统的设置，当前文档的字符集应该是什么

### 自定义数据属性
html5规定可以给元素添加非标准的属性，但是要加前缀`data-`，目的是为元素提供与渲染无关的信息，或者提供语义信息。
```
<div id="myDiv" data-appId="12345" data-myname="Nicholas"></div>
```
自定义属性可以通过元素的`dataset`属性来访问，`dataset`属性的值是DOMStringMap的实例，一对"name-value"名值对的映射。
**自定义属性是`data-myname`，则映射中的属性是`myname`**:
```
var div = document.getElementById("myDiv");
//获取自定义属性
var appId=div.dataset.appId;
var myName=div.dataset.myname;
//设置值
div.dataset.appId=23456;
div.dataset.myname="Michael";
```

### 插入标记
1.innerHTML 属性
在读模式下，`innerHTML`属性返回调用元素的所有`子节点`（包含元素、注释和文本节点）对应的HTML标记。
在写模式下，`innerHTML`属性根据指定的值创建新的DOM树，然后用这个新的DOM树替换调用元素原先的所有`子节点`牌牌琦。
不支持`innerHTML`属性的元素:`<col>`、`<colgroup>`、`<frameset>`、`<head>`、`<html>`、`<style>`、`<table>`、`<tbody>`、`<thead>`、`<tfoot>`、`<tr>`
2. outerHTML 属性
在读模式下，`outerHTML`返回调用它的元素及所有子节点的HTML标签。
在写模式下，`outerHTML`会根据指定的HTML字符串创建新的DOM子树，然后用这个DOM子树替换调用元素。
3. insertAdjacentHTML() 方法
>`element.insertAdjacentHTML(position, text)`,接收两个参数，插入的位置和要插入的HTML文本。position是下列必选之一
    `beforebegin` 在当前元素之前插入一个紧邻的同辈元素
    `afterbegin` 在当前元素之下插入一个新的子元素或在第一个子元素之前再插入新的元素
    `beforeend`  在当前元素之下插入一个新的子元素或在最后一个子元素之后再插入新的元素
    `afterend` 在当前元素之后插入一个紧邻的同辈元素

4.内存与性能问题
由于`innerHTML`、`outerHTML`、`insertAdjacentHTML()`效率很高，但是内存消耗较大，应当合理使用。

### scrollIntoView()方法
HTML5的页面滚动标准方法`scrollIntoView()`，可作用于所有HTML元素。
```
//element会向上让它的顶部和视口顶部对齐
element.scrollIntoView(); // 与element.scrollIntoView(true)相同

// element会向下让它的底部和视口底部对齐
element.scrollIntoView(false); 
```

##  专有扩展
###文档模式
`document mode`4种文档模式
....

### children 属性
`children`属性是`HTMLCollection`的实例，包含元素中同样还是元素的子节点。
当childNodes里面全部是元素节点的时候，与`children`一致


### contains() 方法
```
parentNode.contains(childNode);
```
如果父节点包含子节点，则返回`true`,否则是`false`


### 插入文本
1.innerText 属性 
操作元素中包含的所有文本内容，包括子文档树中的文本。

FireFox不支持`innerText`，但是有类似的属性`textContent`。兼容函数调用:
```
//获取元素节点内的文本
function getInnerText(element){
            return (typeof element.textContent=="string")?element.textContent:element.innerText;
}

//设置元素内的文本节点
function settInnerText(element,text){
           if(typeof element.textContent=="string"){
                    element.textContent =text;
           }else{
                    element.innerText=text;
           }
}
```
>`innerText`和`textContent` 返回内容不完全一样，`innerText`会忽略行内样式，`textContent`则保留行内样式和脚本

2. outerText 属性
`outerText`与`innerText`基本类似，只是扩大到了调用它的节点。
读取模式下,`outerText`和`innerText`一致。
在写模式下,`outerText`会替换整合元素（包含子节点），`innerText`只替换子节点.

**实际上新的节点会替换掉调用outerText的元素**
```
document.body.outerText="";  //相当于将Body及其子元素全部删除了
```


### 滚动
`scrollIntoViewIfNeeded()`方法将元素滚动到视口中可见，如果元素已经在可见区域内，则什么都不做。

`scrollByLines(lineCount)` 方法将元素的内容滚动指定的行高，`lineCount`的值可以是正值和负值

`scrollByPages(pageCount)` 将元素内容滚动指定的页面高度，具体高度由元素高度决定 

**`scrollIntoView()` 和`scrollIntoViewIfNeeded()`的作用对象是元素容器
`scrollByLines(lineCount)` 和`scrollByPages(pageCount)`的作用对象是元素本身**

```
//页面主体滚动5行
document.body.scrollBylines(5);

//在当前元素不可见的时候，让它进入浏览器的视口
document.images[0].scrollIntoViewNeeded();

//让页面主体往回滚动1页
document.body.scrollByPages(-1);
```

### 小结

1. Selector API 定义了两个基于CSS选择权从DOM中取得元素的方法:`querySelector()`和`querySelectorAll()`
2. Element Traversal,元素遍历方便从一个元素跳到另外一个元素
3. HTML5 扩展了`innerHTML`等属性以及焦点管理，字符集设置，页面滚动等扩展API



[TOC]
