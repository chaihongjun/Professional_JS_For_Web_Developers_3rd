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



