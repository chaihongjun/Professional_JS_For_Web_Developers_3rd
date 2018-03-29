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
分别表示节点的名称和值。
对于元素节点，nodeName保存元素的标签名，nodeValue的值始终是null。

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








[TOC]
