[TOC]

# DOM
DOM是针对XHTML和XML文档的一个API，描绘了一个层次化的节点数，允许添加移除修改页面的某一部分

## 节点层次
DOM将任何的HTML或者XML文档描绘成一个由多层节点构成的结构。
节点分为几种不同的类型，每种类型分别表示文档中不同的信息及标记。

`<html>`是文档的最外层元素称为`文档元素`，每个文档只有一个文档元素。

DOM一共有12种节点类型，都继承自Node这个基类型，所有的节点类型都共享相同的基本属性和方法


### Node类型
DOM1定义Node接口，该接口将有DOM中的所有节点类型实现。
12种节点：
```
Node.ELEMENT_NODE(1);  //元素节点
Node.ATTROBUTE_NODE(2);  //元素属性节点
Node.TEXT_NODE(3);  //文本节点
Node.CDATA_SECTION_NODE(4);  //CDATA节点  
Node.ENTITY_REFERENCE_NODE(5);  //实体引用节点
Node.ENTITY_NODE(6);  //实体节点
Node.PROCESSING_INSTRUCTION_NODE(7);  //处理指令节点
Node.COMMENT_NODE(8);  //注释节点
Node.DOCUMENT_NODE(9);  //文档节点
Node.DOCUMENT_TYPE_NODE(10);  //文档类型节点
Node.DOCUMENT_FRAGMENT_NODE(11);  //文档片段节点
Node.NOTATION_NODE(12);  //DTD声明节点
```

兼容IE的判断节点类型方法:
```
//IE  没有公开Node的构造函数
if (someNode.nodeType==1){
        alert(someNode is an element!);
}
```
1. nodeName和nodeValue值
分别表示节点的名称和值。对于元素节点，nodeName保存元素的标签名，nodeValue的值始终是null。
2. 节点关系
每个节点都有`childNodes`属性，保存着`NodeList`对象。`NodeList`是类数组对象，保存一组有序的节点，并且是动态变化的，基于DOM结构动态执行查询的结果。
访问`NodeList`中保存的节点，可以通过`[]`语法访问，也可以通过`item()`方法:
```
var firstChild =someNode.childNodes[0];
var secondChild =someNode.childNodes.item(1);
var count = someNode.childNodes.length;   //包含的子节点数量
```
**关于`childNodes`节点的几个属性:**

>
`parentNode`:指向当前节点的父节点
`previousSibling`: 当前节点的前一个节点 ,childNodes中第一个节点的previousSibling是null
`nextSibling`:当前节点的后一个节点,childNodes中最后一个节点的nextSibling是null
如果childNodes中只有一个节点，则previousSibling和nextSibling都是null
`someNode.childNodes[0]=someNode.firstChild`
`someNode.childNodes[someNode.childNodes.length-1]=someNode.lastChild`
`如果someNode没有子节点,firstChild=lastChild=null`
    
所有节点都有属性`ownerDocument`，指向文档节点。通过这个属性可以不必回溯到顶端而直接访问文档节点
3. 操作节点
**向childNodes列表末尾添加节点`appendChild()`,添加之后childNodes新增节点，父节点等之间的关系指针都有相应的更新。`appendChild()`返回新增的节点**
```
var returnedNode =someNode.appendChild(newNode);
alert(returnedNode == newNode) ;//true
alert(someNode.lastChild==newNode); //true   ,childNodes最后添加了一个新的节点
```
**如果`appendChild()`操作的是旧的节点，则会将旧的节点移动到新的位置**:
```
//someNode有多个子节点
var returnedNode=someNode.appendChild(someNode.firstChild);  //移动someNode的第一子节点到someNode.childNodes的末尾
alert(returnedNode == someNode.firstChild);//false
alert(returnedNode == someNode.lastChild);//true    
```
**如果想把节点放在childNodes的某个特定位置，可以使用`inserBefore()`方法，该方法接收两个参数，要插入的节点和参照的节点。**
**插入新的节点之后，新的节点会成为参照节点的前一个同胞节点(`previousSibling`),同时返回这个新的节点**
**如果参照节点是`null`，则`inserBefore()`和`appendChild()`是一样的。**
```
//插入后成为最后一个子节点
returnedNode = someNode.insertBefore(newNode,null); //在最后null之前插入
alert(newNode=someNode.lastChild); //true

//插入后成为第一个子节点
var returnedNode=someNode.insertBefore(newNode,someNode.firstChild);//在第一个子节点前面插入
alert(newNode=someNode.firstChild); //true

//插入到最后一个子节点前面
var returnedNode=someNode.insertBefore(newNode,someNode.lastChild);
alert(newNode==someNode.childNodes[someNode.childNodes.length-2]);//true
```

**`replaceChild()`可以用新节点替换旧的节点，第一个参数是要插入的节点，第二个参数是要被替换的节点。要被替换的节点将从文档树中移除。**
```
//替换第一个子节点
var returnedNode=someNode.replaceChild(newNode,someNode.firstChild);
//替换最后一个子节点
var returnedNode = someNode.replaceChild(newNode,someNode.lastChild);
```
**`removeChild()`可以移除节点，参数为要被移出的节点**
```
//移除第一个子节点
var formerFirstChild=someNode.removeChild(someNode.firstChild);
//移除最后一个子节点
var formerLastChild=someNode.removeChild(someNode.lastChild);
```
**无论是`replaceChild()`替换掉的节点还是`removeChild()`删除的节点，仍然为文档所有，只是在文档树中没有位置。**

**以上4个操作方法`appendChild(newNode)`,`insertBefore(newNode,oldNode)`,`replaceChild(newNode,oldNode)`和`removeChild(oldNode)`都是通过父节点(`parentNode`)操作，且必须是包子节点类型的节点上**
4. 其他方法
**`cloneNode()`，创建调用某个节点的副本，可选参数类为布尔值，表示是否是深度复制，返回的节点属于文档所有，但是没有父节点**
```
someNode.cloneNode(true); //深度复制，复制本身节点和其子节点（如果有的话）
someNode.cloneNode();// 只复制节点本身，不复制其子节点（如果有的话）
```
**注意，`cloneNode()`只复制DOM节点，不会复制添加到节点上的Javascript属性，IE则会负责事件处理程序，因此建议在复制前，最好先移出事件处理程序**

**`normalize()`方法处理文档树中的文本节点，对于空的文本节点则删除，对于连续出现的文本节点则合并。**

### Document 类型
JavaScript通过Document类型表示文档，在浏览器中,`document`对象是`HTMLDocument`(继承自Document类型)的一个实例，表示整个HTML页面，又是`window`的一个属性，可以当作全局对象访问。
Document节点的特征:
>nodeType =9 
nodeName 的值是 "#document"
nodeValue 的值为null
parentNode 为null
ownerDocument 值为null
其子节点可能是一个DocumentType（最多一个）,Element(最多一个),ProcessingInstruction或Comment

Document类型可以表示HTML页面或者其他基于XML的文档，不过常见作为HTMLDocument的实例document对象

1. 文档的子节点
`document.documentElement`可取得`<html>`
`document.body`可取得`<body>`
`document.doctype`可取的`<!DOCTYPE>`   
```
document.documentElement==document.childNodes[0]==document.firstChild==<html>
```
2. 文档信息
```
document.title //文档标题
document.url //文档URL
document.domian //文档域名
document.referrer //取得来源页面的URL
```
特别注意domain设置的限制，如果开始是松散类型的，后面就不可以改成绷紧的:
```
document.domain="wrox.com";  //松散
document.domain="www.wrox.com"; //紧绷的，会出错
```

3.查找元素
`getElementById()`和`getElementsByTagName()`分别通过元素的ID（严格大小写匹配）和元素标签名称查找。
**`getElementsByTagName()`返回的是零或者多个元素的NodeList，在HTML中这个方法返回一个HTMLCollection对象，作为“动态”集合。可以通过`[]`语法或者`item()`方法访问HTMLcollection中的对象项，这个对象中的数量可以通过`length`属性取得。**
```
var images=document.getElementsByTagName("img");
alert(images.length); //图片数量
alert(images[0].src);  //第一张图片src
alert(images.item(0).src);  //第一张图片src
```
HTMLCollection还包含方法`namedItem()`，通过元素节点的`name`属性获取元素。
```
<img src="myimage.gif" name="myImage">

var myImg=images.namedItem("myImage")； //   **通过id或者name获取元素（当id不存在的时候查找name)**

```

关于HTMLCollection:
>https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCollection
HTMLCollection.item()
根据给定的索引（从0开始），返回具体的节点。如果索引超出了范围，则返回 null。
HTMLCollection.namedItem()
根据 Id 返回指定节点，或者作为备用，根据字符串所表示的 name 属性来匹配。根据 name 匹配只能作为最后的依赖，并且只有当被引用的元素支持 name 属性时才能被匹配。如果不存在符合给定 name 的节点，则返回 null。


`getElementsByName()`获取特定`name`的所有元素，最常用的是获取单选按钮：
```
<fieldset>
    <legend>Which color do you prefer?</legend>
    <ul>
        <li>
            <input type="radio" name="color" value="red" id="colorRed">
            <label for="colorRed">Red</label>
        </li>
        <li>
            <input type="radio" name="color" value="green" id="colorGreen">
            <label for="colorGreen">Green</label>
        </li>
        <li>
            <input type="radio" name="color" value="blue" id="colorBlue">
            <label for="colorBlue">Blue</label>
        </li>
    </ul>
</fieldset>
```
4.特殊集合
`document`对象还有一些特殊集合，都属于`HTMLCollection`对象：
>`document.anchors`：包含所有带`name`的`a`元素.
   `document.applets`：包含所有的`applet`元素.
   `document.forms`：包含所有的`form`元素，与`document.getElementsByTagName("form")`结果一致
    `document.images`：包含所有的`img`元素，与`document.getElementsByTagName("img")`结果一致
    `document.links`：包含所有的带`href`的`a`元素

5.DOM 一致性检测
`document.implementation`实现了对浏览器DOM检测的功能。
>DOM1的检测方法: document.implementation.hasFeature("要检测的DOM1的功能名称","对应的功能版本号");如果浏览器支持则返回`true`

```
//检测浏览器是否支持DOM1的XML1.0版本
var hasXmlDom=document.implementation.hasFeature("XML","1.0");
```

可检测的功能和版本号:

| 功能 | 版本号 | 说明 |
| -- | -- | -- |
| Core | 1.0 、2.0、3.0 | 基本的DOM，用于描述表现文档的节点树 |
| XML | 1.0、2.0、3.0 | Core的XML扩展，添加了对CDATA、处理指令和实体的支持 |
| HTML | 1.0、2.0 | XML的HTML扩展，添加了对HTML特有元素及实体的支持 |
| Views | 2.0 | 基于某些样式完成文档的格式化 |
| StyleSheets | 2.0 | 将样式表关联到文档 |
| CSS | 2.0 | 对层叠样式表1级的支持 |
| CSS2 | 2.0 | 对层叠样式表2级的支持 |
| Events | 2.0、3.0 | 常规的DOM事件 |
| UIEvents | 2.0、3.0 | 用户界面事件 | 
| MouseEvents | 2.0、3.0 | 由鼠标引发的事件(click、mouseover等) |
| MutationEvents | 2.0、3.0 | DOM树变化时引发的事件 |
| HTMLEvents | 2.0 | HTML4.01事件 |
| Range | 2.0 | 用于操作DOM树中某个范围ide对象和方法 |
| Traversal | 2.0 | 遍历DOM树的方法 |
| LS | 3.0 | 文件和DOM树之间的同步加载和保存 |
| LS-Async | 3.0 | 文件和DOM树之间的异步加载和保存 |
| Validation | 3.0 | 在确保有效的前提下修改DOM树的方法 |

6.文档写入
```
document.write();  //原样写入到页面
document.writeln();   //末尾会添加换行符
document.open()
document.close();
```
需要注意转义"\"

### Element 类型
Element类型用于表现XML或HTML元素，具有以下特征:
>nodeType = 1
   nodeName 为元素的标签名
   nodeValue 值为 null
   parentNode 可能是Document 或者 ELement
   其子节点可能是Element、Text、Comment、ProcessingInstruction、CDATASection或者EntityReference

要访问元素可以使用`nodeName`属性或者`tagName`属性 

1.HTML元素
HTML元素都有的特性:
>`id`,元素在文档中的唯一标识
`title`,有关元素的附加说明，一般通过工具提示条显示
`lang`,元素内容的语言代码，很少用
`dir`,语言的方向，值为"`lrt`"(left-to-right)或者"`rtl`"(right-to-left),很少使用
`className`,元素与class特性对应，为元素指定CSS类，由于`class`是ECMAScript的保留字，所以使用`className`

2.取得特性
操作元素特性的DOM方法主要有三个:`getAttribute()`，`setAttribute()`，`removeAttribute()`

通过`getAttribute()`方法也可以取得自定义特性，特性名称不区分大小写,"ID"和"id"代表的都是同一个特性。


3.设置特性
```
setAtttribute("特性名称","特性值");
```
**如果特性已经存在，则更新特性的值，通过`setAttribute()`方法即可以操作HTML的特性特可以操作自定义的特性。这个方法会统一将特性名称全部转换为小写。**
通过`元素.特性名称=特性值`这样的方式添加自定义属性，是无法添加成功的。
```
removeAttribute("特性名称");
```
特性和特性值都会彻底被删除

4.attributes 属性
**Element类型是使用attributes属性的唯一一个DOM节点类型，其中包含`NamedNodeMap`与`NodeList`类似是一个"动态"集合。`NamedNodeMap`保存了元素的每个特性，这个特性有`Attr`节点表示。**
`NamedNodemap`对象有下列方法：
>`getNamedItem(name)`，返回nodeName属性等于name的节点
`removeNamedItem(name)`，从列表删除nodeName属性等于name的节点
`setNamedItem(node)`，向列表添加节点，以节点的nodeName属性为索引。
`item(pos)` ，返回位于数字pos位置处的节点

`attributes`属性包含一系列节点。每个节点的`nodeName`就是特性的名称，节点的`nodeValue`就是特性的值
```
//获取ID的值
var id=element.attributes.getNamedItem("id").nodeValue;
//与下面方法一致
var id=element.attributes["id"].nodeValue'
```

`element.attributes.removeNamedItem("特性名称")`与在元素上调用`removeAttribute("特性名")`效果一致，唯一区别是`removeNamedItem("特性名称")`会返回被删除的特性的Attr节点
5. 创建元素
```
document.createElement("标签名");
```
`createElement`创建的元素同时也设置了`ownerDocument`属性，但是新元素没有被添加到DOM文档树中，需要通过`appendChild()`，`insertBefore()`或者`replaceChild()`方法添加。

6. 元素的子节点
```
element.getElementsByTagName("元素名称");
```
`getELementsByTagName()`也支持从元素节点开始查找

### Text 类型
text节点有以下特征：
>nodeType = 3
   nodeName 的值为"#text"
   nodeValue 的值为节点所包含的文本
   parentNode  是一个Element
   没有子节点
   data属性和nodeValue属性一样可以访问Text节点中的文本，而且互相影响。以下方法可以操作节点中的文本
   `appendData(text)`，将text加到节点的末尾
   `deleteData(offset,count)`，从`offset`位置开始删除`count`个字符
   `insertData(offset,text)`，在`offset`位置插入文本`text`
   `replaceData(offset,count,text)`，用`text`替换从`offset`开始到`offset+count`处的文本
   `splitText(offset)`，从`offset`位置开始将当前文本节点分成两个文本节点
    `substringData(offset,count)`，提取从`offset`开始到`offset+count`的字符串

文本节点的`length`属性保存着节点中的字符数

1.创建文本节点
```
document.createTextNode("文本内容");
```
新创建的文本节点如果要在页面显示，必须通过`appendChild()`，`insertBefore()`，`replaceChild()`方法。
```
var element=document.createElement("div");
  element.className="message";
var textNode = document.createTextNode("Hello world!");
  element.appendChild(textNode);
var anotherTextNode = document.createTextNode("Yippee!");
 element.appendChild(anotherTextNode);
document.body.appendChild(element);
```

2.规范化文本节点
在一个包含多个节点的父元素调用`normalize()`，则会把所有文本节点合并为一个:
```
var element=document.createElement("div");
  element.className="message";
var textNode = document.createTextNode("Hello world!");
  element.appendChild(textNode);
var anotherTextNode = document.createTextNode("Yippee!");
 element.appendChild(anotherTextNode);
document.body.appendChild(element);

alert(element.childNodes.length);  //2

element.normalize();  //合并文本节点
alert(element.childNodes.length);  //1
```

3.分割文本节点
与`normalize()`相反，`splitText()`方法将一个文本节点分成两个文本节点(按照指定的位置分割nodeValue):
**`splitText(index)` 也可以理解为截取的长度**
```
var element=document.createElement("div");
  element.className="message";
var textNode = document.createTextNode("Hello123world!");
  element.appendChild(textNode);

document.body.appendChild(element);

var newNode =element.firstChild.splitText(8);//  从开头到索引5(不含5位置)为一个内容，剩下的为另外一个内容
//“Hello ”, "world!"
  alert(element.firstChild.nodeValue);//  "Hello"
  alert(newNode.nodeValue);  //返回分割后新的文本 "world!"
  alert(element.childNodes.length);    //2


```




[TOC]
