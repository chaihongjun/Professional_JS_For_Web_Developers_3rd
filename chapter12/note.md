
[TOC]


#DOM2和DOM3
DOM1主要定义的是XML和HTML的文档底层结构
DOM2和DOM3 引入更多的交互能力，所以DOM2和DOM3分为许多模块：

```
DOM Level 2 Core:在DOM1的基础上为节点添加了更多方法和属性
DOM level 2 Views:为文档定义了基于样式信息的不同视图
DOM Level 2 Events:说明了如何使事件和DOM文档交互
DOM Level 2Style: 定义了如何以编程方式来访问和改变CSS样式信息
DOM Level 2 Traversal and Range:引入遍历DOM文档和选择其特定部分的新接口
DOM Level 2 HTML:在DOM1 HTML基础上构建添加了更多属性方法和接口
```


##DOM变化
DOM2没有引入新类型，只是增加了新方法和新属性，DOM3既增强了现有的类型又新增了一些新类型

### 针对XML命名空间的变化
1. Node类型的变化
DOM2级中，Node类型包含下列特定于命名空间的属性:

>`localName`，不带命名空间前缀的节点名称
   `namesapceURI` ，命名空间URI或者null
    `prefix`，命名空间前缀或者null

当节点使用了命名空间前缀时，`nodeName=prefix+"："+localName`：
```
<html xmlns="http://www.w3.org/1999/xhtml">
    <head>
                <title>Example xhtml page</title>
     </head>
            <body>
                        <s:svg xmlns:s="http://www.w3.org/2000/svg" version="1.1" viewBox="0 0 100 100" style="width:100%;height:100%">
                            <s:rect x="0" y="0" width="100" height="100" style="fill:red" />
                         </s:svg>
              </body> 
      </html>                                
```   
以上对于`<html>`来说，它的`nodeName`和`tagName`就是`html`，`namespaceURI`就是`http://www.w3.org/199/xhtml`，`prefix`是null
对于`<s:svg>`元素，`tagName`是`s:svg`，`prefix`是s，`localName`是svg

与命名空间相关的方法:
```
isDefaultNamespace(namspaceURI) 判断给定的namespaceURI是否是当前节点默认命名空间
lookupNamespaceURI(prefix) 返回给定prefix的命名空间
lookupPrefix(namespaceURI) 返回给定命名空间的前缀
```

2. Document类型的变化
包含下列与命名空间相关的方法
```
createElementNS(namespaceURI,tagName) 创建一个属于命名空间namespaceURI的tagName元素
createAttributeNs(namespaceURI,attributeName) 创建一个属于命名空间namespaceURI的新的特性
getElementsByTagNameNS(namespaceURI,tagName) 返回属于命名空间namespaceURI的tagName元素NodeList
```

3. Element类型的变化
新增方法:
```
getAttributeNS(namespaceURI,localName) 返回属于命名空间namespaceURI的localName的特性
getAttributeNodeNS(namespaceURI,localName) 返回属于命名空间namespaceURI的localName的特性节点
getElementsByTagNameNS(namespaceURI,tagName) 返回属于命名空间namespaceUR的tagName元素的NodeList
hasAttributeNS(namespaceURI,localName) 确认当前元素是否有一个名为localName的特性，并且该特性属于命名空间namespaceURI
removeAttributeNS(namespaceURI,localName) 删除属于命名空间namespaceURI的名为localName的特性
setAttributeNS(namespaceURI,qualifiedName,value) 设置属于命名空间namespaceURI，特性名为qualifiedName，值为value的特性
setAttributeNodeNS(attNode) 设置属于命名空间namespaceURI，特性名为qualifiedName，值为value的特性
```

4. NamedNodeMap 类型变化
```
getNamedItemNS(namespaceURI,localName) 取得命名空间namespaceURI的localName项
removeNamedItemNS(namespaceURI,localName)  删除属于命名空间namespaceURI的localName项
setNamedItemNS(node) 添加node ，这个节点事先指定了命名空间
```

### 其他方面的变化
1. DocumentType类型的变化

新增3个属性`publicId`、`systemId`和`internalSubset`
```
document.doctype.publicId
document.doctype.systemId
document.doctype.internalSubset
```


## 样式

### 访问元素的样式

任何支持`style特性`的HTML元素在JavaScript中都有一个对应的`style属性`。这个`style`对象是`CSSStyleDeclaration`的实例，包含通过HTML的style特性指定额度所有样式信息，但不包含外部样式表和嵌入样式表层叠的样式。
在HTML元素的style特性中指定的CSS属性，都有JavaScript的Style对象属性一一对应。使用短划线的CSS属性名，将会转化为驼峰大小写形式:

| CSS属性 |  JavaScript属性 |
| - | - |
| bakground-image | style.backgroundImage |
| color | style.color |
| display | style.display |
| font-family | style.fontFamily |
| float | style.cssFloat |

由于`float`在JavaScript中山保留字，所以这里对应的JavaScript属性名是`cssFLoat`,(IE支持的是`styleFloat`)

1. DOM样式属性和方法
DOM2 Level Style 规范为style对象定义了属性和方法：
```
cssText：可以获取style特性中的CSS代码
length：元素的CSS属性数量
parentRule：CSS信息的CSSRule对象。
getPropertyCSSValue(propertyName) 返回给定属性的CSSValue对象
getPropertyPriority(propertyName) 如果给定的属性使用了`!important` ，则返回`important` 否则返回空字符串
getPropertyValue(propertyName) 返回给定属性的字符串值
item(index) 返回给定位置的CSS属性名称
removeProperty(propertyName) 从样式中删除给定的属性
setProperty(propertyName,value,priority)将给定的属性设置相应的值，并加上优先权标志(!important或者看字符串)
```
Style对象实际相当于一个集合，可以通过`[]`或者`item(index)` 访问给定位置的CSS属性(获取的是CSS属性名):

```
for(var i=0,len=myDiv.style.length; i<len; i++){
            alert(myDiv.style[i]); // 或者 myDiv.style.item(i)
}
```

2. 计算的样式


```
document.defaultView.getComputedStyle(元素,伪元素字符串/null)

```


### 操作样式表
应用于文档的所有样式表`CSSStyleSheet通过`document.styleSheets`集合表示，通过集合的`length`属性可知道文档中样式表的数量，通过`[]`或者`item()`可以访问每一个指定的样式表。
`CSSStyleSheet` 继承自`StyleSheet` ，继承的属性:

```
disabled，表示是否禁用样式表
href，样式表URL
media，当前样式表支持的所有媒体集合
ownerNode，指向拥有当前样式表的节点的指针。样式表可能是在HTML中通过 `<link>`或者`<style>` 引入的
parentStyleSheet，当前样式表如果是通过@import导入的，则这个属性指向导入它的样式表的指针
title，ownerNode中的title属性值
type，表示样式表类型的字符串。对CSS来说就是"type/css"
cssRules，样式表中包含的样式规则集合。IE有一个类似的rules属性。
ownerRule，若样式表是通过@import导入的，则这个属性指向导入的规则
deleteRule(index)，删除cssRules集合中指定位置的规则。IE有一个类似的removeRule()方法
insertRule(rule,index) ，向cssRules集合指定位置插入rule字符串。IE有一个类似的addRule()方法

```

```
//返回当前文档的所有样式表的href
var sheet=null;
for (var i=0,len=document.styleSheets.length;i<len;i++){
        sheet=document.styleSheets[i];
        alert(sheet.href);         
}
```
DOM规定了一个包含CSSStyleSheet对象的属性`sheet`,可以访问的CSSStyleSheet对象:
```
//非IE
document.getELementsByTagName("link")[0].sheet;
//IE
document.getELementsByTagName("link")[0].styleSheet;
```

1. CSS规则
`cssRule`对象表示样式表中的每一条规则。CSSRule是一个供其他多种类型继承的基类型，其中最常见的CSSStyleRule类型表示样式信息。
`CSSStyleRule`包含以下属性:
```
cssText，返回整条规则对应的文本，包含选择器和规则内容
parentRule，如果当前规则是导入的规则，则这个属性引用的就是导入规则
parentStyleSheet，当前规则所属的样式表
selectorText，返回当前规则的选择符文本
style，CSSStyleDeclaration对象，可以设置和取得规则中特定的样式值
type，表示规则类型的常量值，样式规则值为1
```
```
div.box{
    background-color: blue;
    width: 100px;
    height: 200px;
}

var sheet =document.styleSheets[0]; //获取第一条css文件资源
var rules =sheet.cssRules || sheet.rules;   //兼容写法，获取第一条CSS文件的所有规则列表
var rule= rules[0];  //获取第一条记录
alert(rule.selectorText);
alert(rule.style.cssText);
alert(rule.style.backgroundColor);

//当然也可以修改规则
rule.style.width="200px;";  //修改了div.box的宽度
```

2. 创建规则
DOM规定向现有的样式表中添加规则需要使用`insertRule(cssRule,index)`
```
//cssRule 表示具体的规则，index表示在哪个位置插入规则
document.styleSheet[0].insertRule(cssRule,index);

//IE的方法
document.styleSheet[0].addRule(cssSelector,rules,index);
```

```
//兼容性方法
// selectorText CSS选择器
// cssText 规则内容
//postion 要添加的位置
var sheet =document.styleSheet[0];
function insertRule(sheet,selectorText,cssText,position){
        if(sheet.insertRule){
              sheet.insertRule(selectorText+"{"+cssText+"}",position);
        }else if(sheet.addRule){
              sheet.addRule(selectorText,cssText,position);
        }
}
```

3. 删除规则
```
//删除第一个样式表中的第一条规则
document.styleSheet[0].deleteRule(0);   //DOM方法

document.styleSheet[0].removeRule(0);   //IE方法
```
```
//删除样式规则的兼容写法
//sheet为样式表指针
//index 要删除的规则索引
var sheet=document.styleSheet[0];
function deleteRule(sheet,index){
      if(sheet.deleteRule){
              sheet.deleteRule(index);
      }else if (sheet.removeRule){
              sheet.removeRule(index);
      }
}
```


### 元素大小 
内容不属于DOM Level 2 Style 规范，但是主流浏览器都支持

1. 偏移量（offset dimention），包括元素在屏幕上占用的所有可见空间（高度，宽度，内边距，滚动条，边框）
```
offsetHeight，元素在垂直方向上占用的空间大小，像素单位。包括元素的高度、（可见的）水平滚动条高度、上边框高度和下边框高度
offsetHeight =  border-top-height + padding-top + height + padding-bottom + border-bottom-height
offsetWidth，元素在水平方向上占用的空间大小，像素单位。包括元素的宽度、（可见的）垂直滚动条宽度、左边框宽度和右边框宽度
offsetWidth =  border-left-width + padding-left + width + padding-right + border-right-width
offsetLeft，元素的左外边框至包含元素的左内边框之间的像素距离
offsetTop，元素的上外边框至包含元素的上内边框之间的像素距离
```
参阅 https://www.cnblogs.com/xiaohuochai/p/5828369.html

![关于offsetparent](https://chaihongjun.github.io/Professional_JS_For_Web_Developers_3rd/chapter12/offset.jpg)

以上4个关于偏移量的内容都与`offsetparent`(定位父级)相关
`offsetparent`的定义：与当前元素最近的经过定位（position不是static）的父级元素.

一个元素的定位父级和这个元素本身的定位属性有关:
当元素自身的定位是`fixed`，则这个元素的定位父级是`null`
当元素自身没有`fixed`，而且这个元素的父级没有定位，则这个元素的定位父级是`<body>`
 当元素自身没有`fixed`，而父元素存在定位，则`offsetparent`是离这个元素最近的经过定位的元素

**可以看出元素的`offsetParent`是逐级向上查找，如果这个元素本身是`fixed`定位的，则它的offsetParent是null，如果这个元素定位不是`fixed`，则从元素层级向DOM树上层找到有定位属性的元素，这个有定位属性的元素就是前一个元素的offsetParent,否则就是`<body>`了。**

```
//获取元素的偏移
function getElementLeft(element){
                var acrualLeft =element.offsetLeft;
                var current=element.offsetParent;

                while(current!==null){
                               actualLeft+=current.offsetLeft;
                               current=current.offsetParent; 
                }
    
            return actualLeft;
}

 function getElementTop(element){
            var actualTop = element.offsetTop;
            var current = element.offsetParent;

            while(current!==null){
                    actualTop+=current.offsetTop;
                    current=current.offsetParent;
            }
           return  actualTop; 
 }
    
2. 客户区大小
客户区大小(client dimension)指元素内容和内边距占据的空间大小。
```
clientWidth :元素内容宽度+左内边距+右内边距
clientHeight :元素内容高度+上内边距+下内边距
```
**client dimension不含滚动条所占用的空间**


![client dimension](https://chaihongjun.github.io/Professional_JS_For_Web_Developers_3rd/chapter12/Dimensions-client.png)

3. 滚动大小
滚动大小(scroll dimension) 包含滚动内容的元素的大小。有些元素比如`<html>`即使没执行任何代码也可以自动添加滚动条。其他的需要通过CSS的overflow属性设置
与滚动条相关的属性:
```
scrollHeight：没有滚动条的情况下元素内容的总高度
scrollWidth：没有滚动条的情况下元素内容的总宽度
scrollLeft：被隐藏在内容区域做出的像素数，设置可改变元素的滚动位置
scrollTop：被隐藏在内容区域上方的像素数，设置可改变元素的滚动位置
```
![scroll](https://chaihongjun.github.io/Professional_JS_For_Web_Developers_3rd/chapter12/scroll.jpg)



4.确定元素大小



## 遍历
(to be continued)
