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
`getElementsByTagName()`返回的是零或者多个元素的NodeList

[TOC]
