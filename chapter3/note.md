[TOC]
# JS基本概念

## JS区分大小写

## 标识符规则
1. 变量，函数，属性，参数等标识符第一个字符必须是一个字母，或者下划线(_)或者一个美元符号($)
2. 其他字符可以是字母，数字，下划线(_)或者美元符号($)
3. 标识符推荐使用驼峰大小写格式
```
e.g:
firstSecond
myCar
doSomethingImportant
```
4. JS保留的关键词不可作为标识符

## 注释
```
//单行注释
/*
这这里面是代码，
适合多行或者块级注释
 */
```

## 严格模式
在ECMAScript5中，引入了严格模式(strict mode),会对ECMAScript3中一些不安全操作抛出错误，对一些不确定行为处理。
如果要对整个脚本启用严格模式，在代码顶部添加：
```
"use strict";
```
如果是想对某个函数内启用严格模式，也可以在函数内容运用：
```
function doSomething(){
    "use strict";
    //函数内容
}
```
## 语句
ECMAScript中的语句以分号`;`作为结尾，如果不添加分号，则解析器会去确定语句的结尾，增加解析器的消耗。
## 关键字和保留字
### 关键字
ECMA-262描述了关键字，这些关键字用来表示控制语句的开始和结束，或者执行特定操作。关键字也是保留字，不能作为标识符。
```
//ECMAScript的全部的关键字，*为5中新增的
break       do       instanceof     typeof 
case        else        new     var
catch       finally     return      void
continue        for     switch      while
debugger*        function       this         with               
default     if      throw               
delete      in      try
```
### 保留字
ECMA-262 还有另外一组不能作为标识符的**保留字**,目前做任何用途，但是未来可能被作为关键字，以下是**ECMA3**的全部保留字：
```
abstract        enum        int     short
boolean     export      interface       static
byte        extends     long        super
char        final       native      synchronized
class       float       package     throws
const       goto        private     transient
debugger        implements      protected       volatile
double      important       public
```
**ECMA5**非严格模式下的保留字：
```
class       enmu        extends     super
const       export      import
```
**ECMA5**严格模式对以下保留字做了限制：
```
implements      package     public
interface       private     static
let     protected       yield
```
另外，**ECMA5**严格模式还对以下两个做了限制，不可作为标识符：
```
eval        arguments
```
## 变量
ECMAScript的变量是松散类型的，可以保持任意类型的数据。
###变量定义方法
```
var 标识符;
```
比如：
```
var message;
```
未初始化变量值的变量，会先保存一个特殊的值`undefined`，这里`message`的值就是`undefined`。
**使用`var`定义的变量，成为了定义该变量的作用域中的局部变量。**
```
e.g:
function test(){
        var message="hi";//局部变量
}
test();
alert(message);//错误，无法调用test()内的局部变量
```
这里的`message`变量通过`var`定义在函数`test`内,所以`message`是局部变量，如果想使上面的代码可行，只需去掉声明前面的`var`:
```
function test(){
        message="hi";//全局变量
}
test();
alert(message);//不会报错，正确
```
另外，还可以同时初始化多个变量：
```
var message="hi",found=false,age=29;
```
**严格模式下，不能定义以`eval`或者`arguments`为名字的变量，否则语法错误。**
## 数据类型
ECMAScript5有五种简单的数据类型(基本数据类型):
```
1. undefined
2. Null
3. Boolean
4. Number
5. String
```
和一种复杂的数据类型:
```
Object
```
**`Object`本质是一组无序的名值对组成的。**
ECMAScript不支持创建自定义的数据类型，所有的最终值都是以上的6种类型之一。
### typeof 操作符
使用`typeof`可以判断变量值的类型
```
var message="some string";
var ok=true;
alert(typeof message);  //"string"
alert(typeof(message));  //"string"
alert(typeof 95); //"number"
alert(typeof ok); // "boolean"
```
**`typeof`是一个操作符而不是函数**，因此
```
typeof message 
typeof (message)
```
都可以，但是后面的`（）`不是必须的
### undefined类型
**变量声明但未初始化值就是`undefined`，不过包含`undefined`值的变量与没定义的变量还是不一样的**
```
//声明一个变量
var message;
//下面这个变量没有声明
//var age
alert(message);  //undefined;
alert(age); //产生错误
```


### Null类型
nulll类型是一个object（对象），null值表示一个空对象指针：
```
var car=null;
alert(typeof car); // "object"
```
**如果定义的变量准备存储对象，那么最好将变量初始化为null,这样方便检测对应的变量是否保存了对象的引用。**
```
if(car!=null){
    //对car 对象执行某些操作
}
```
由于`undefined`值是派生于`null`值，因此，他们的相等性检测是true:
```
alert(null==undefined) //true
```
## Boolean值
**Boolean的字面值是`true`和`false`，并且区分大小写，因此TRUE,False等等都不是Boolean值，他们都是标识符**
ECMAScript其他类型的值都有与这两个布尔值等价的值，如果想转化成布尔值，可以通过Boolean()函数：
```
var message="hello world!";
var messageAsBoolean=Boolean(message);   
alert(messageAsBoolean); //true
```
>
|数据类型       | 转换成true的值      | 转换成false的值|
|-------------|--------------------|-----------------|
|Boolean        | true                           |false|
|String             |任何非空的字符串       |""空字符串|
|Number      |任何非零的数字值(包括无穷大)     |0和NaN|
|Object      |任何对象        |null|
|Undefined       |不适用     |undefined|

## Number 类型
```
十进制字面量（0-9）
八进制字面量（0-7）：011，056
十六进制字面量（0-9，A-F）：0xAF，0x19
```
**严格模式下八进制字面量是无效的**

### 浮点数
**数值中包含一个小数点，且小数点后面至少有一位数字，浮点数最高精度是17位小数**

####NaＮ
NaN(Not a Number)是一个特殊的值，非数值。表示一个本来要返回数值的操作数未返回数值的情况。
**NaN与任何值都不想等，包括NaN本身**
```
alert(NaN==NaN); //false 
```
##### isNaN()
这个函数将会尝试将接收的参数转换成数值，并判断接收的参数是否"不是数"，**任何不能转换成数值的值都会返回true**
```
alert(isNaN(NaN)); //true,NaN不能变成数值
alert(isNaN(10)); //false ,10本身是数值
alert(isNaN("blue")); //true，字符串不能转换成数值
alert(isNaN(true)); //false， 可以转成成数值1
```
