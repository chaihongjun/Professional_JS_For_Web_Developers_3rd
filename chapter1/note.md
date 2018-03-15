[TOC]
## JS历史略过
## JS的实现
**JavaScript=ECMAScript+DOM+BOM**

其中ECMAScript是JS核心,DOM是针对XML，但是经过扩展用于HTML的一个API，DOM把整个web页面映射成有多层节点的结构
## DOM的级别
DOM: Document Object Model文档对象模型，针对XML但是经过扩展用于HTML的API

1. **DOM0：实际不存在的级别，只是为了一个参考，最初是IE4和Netscape4.0支持DHTML**

2. **DOM1：1998年W3C标准，由两个模块组成DOM Core和DOM HTML。DOM Core规定了基于XML如何映射文档结构，简化对文档节点的访问和操作，DOM HTML则是在DOM Core的基础上扩展，增加了对HTML的支持（对象和方法），主要是对文档映射的支持，另外对DOMCore的扩展使DOM1 支持XML命名空间**

3. **DOM2 :  继续扩充，增加对鼠标和用户界面事件，范围，遍历（迭代DOM文档的方法）等细分模块，并且通过对象接口增加对CSS的支持，DOM2新增的模块：
DOM Views：定义了跟踪不同文档视图接口
DOM Events : 定义了事件和事件处理的接口
DOM Style : 定义了基于CSS为元素应用样式的接口
DOM Traversal and Range:定义了遍历和操作文档的接口**

4.  **DOM3 ：引入了以统一方式加载和保存文档的方法（DOM Load and Save），和DOM Validation。以及对DOM Core扩展，使DOM3支持XML Infoset,XPath 和XML Base等XML1.0规范。**

## BOM
BOM : Browser Object Model，浏览器对象模型，直到HTML5才将BOM标准化。本身，BOM只处理浏览器窗口和框架，但是习惯将针对浏览器的JS扩展也算作BOM的一部分。
e.g:
弹窗新浏览器窗口的功能
移动，缩放，关闭浏览器窗口的功能
Navigator对象（提供浏览器详细信息的对象）
提供用户显示器分辨率详细信息的screen对象
对cookies的支持


## 小结
1. ECMAScript 是JS的核心部分
2. DOM 提供了操作页面内容的方法和接口
3. BOM 提供了与浏览器交互的方法和接口


[TOC]

