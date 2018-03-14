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
alert(isNaN("10")); //false ,"10"可以转换成数值
alert(isNaN("blue")); //true，字符串不能转换成数值
alert(isNaN(true)); //false， 可以转成成数值1
```

### 数值转换
```
//针对任意类型数据
Number()
//将字符串转换成数值
parseInt() //字符串转换成整数
parseFloat()  //字符串转换成浮点数
以上三个函数可以将非数值转换成数值
```
#### Number() 转换规则
>1. 如果参数的布尔值，则`true`和`false`分别被转换成`1`和`0`
2. 如果参数是数值，则是简单的传入和返回。
3. 如果是`null`,返回`0`
4. 如果是`undefined`，返回`NaN`
5. 如果是字符串，纯数字或者是十六进制格式，则会转换成对应的十进制（前导0会忽略），如果是空字符串，则会转换成0，如果字符串包含其他字符，则转换成NaN
6. 如果是对象，则调用对象的valueOf(),然后按照前面的规则转换；如果转换的结果是NaN，则用toString()，再按照前面的规则转换。


```
var num1=Number("Hello world!");  //NaN
var num2=Number("");  //0,空字符串
var num3=Number("000011");  //11
var num4=Number(true);  //1
```

#### parseInt() 的转换规则
```
var num1=parseInt("1234blue"); // 1234
var num2=parseInt(""); //NaN
var num3=parseInt("0xA"); //10 (十进制数)
var num4=parseInt(22.5); //22
 ** var num5=parseInt("070"); //56 (八进制) ECMAScript3 **
 ** var num5=parseInt("070"); //70(八进制)  ECMAScript5 无法解析八进制数值** 
var num6=parseInt("70"); //70 (十进制)
var num7=parseInt("0xf"); //15 (十六进制数)
```
ECMAScript3和ECMAScript5中parseInt()对八进制的解析结果不一致，ECMAScript5无法解析八进制，因此可以使用parseInt()第二个参数指明需要转换的的进制

```
var num=parseInt("0xAF",16); //175
var num1=parseInt("AF",16); //175，明确了是16进制，所以AF就是0xAF
var num2=parseInt("AF"); //NaN ，不知道是什么进制，AF无法转换
```
当指明了是16进制需要转换，则前面的“0x”可以省略。所以，**为了保证不出现出错，建议无论什么情况下都明确指定基数（进制）**

#### parseFloat() 的转换规则
与parseInt()类似，但是总忽略被转换的字符串前导0，并且会过略忽略字符串第二个以外的小数点。
16进制字符串始终转换结果是0
```
var num1=parseFloat("1234blue");   //1234
var num2=parseFloat("0xA"); //0 , 16进制始终转换结果是0
var num3=parseFloat("22.5"); //22
var num4=parseFloat("22.34.5"); //22.34
var num5=parseFloat("0908.5"); //908.5
var num6=parseFloat("3.125e7"); /31250000
```

## String 类型
**字符串类型可以由双引号`""`或者单引号`''`表示，并且，这两种语法形式没有区别**

### 字符串字面量 （转义序列，表示非打印字符，具有其他的用途）
| 字面量 |含义|
|--------|-----|
|  \n |  换行|
| \t  | 制表|
| \b| 退格|
| \r| 回车|
| \f| 进纸|
| \\| 斜杠|
| \'| 单引号(')，在用单引号表示的字符串中使用。例如：'He said,\'hey.\''|
| \"| 双引号(")，在用单双引号表示的字符串中使用。例如："He said,\"hey.\""|
| \xnn| 以十六进制代码nn表示的一个字符串(其中n为0~F)。例如：\x41表示"A"|
|\unnn| 以十六进制代码nnn表示的一个unicode字符（其中n为0～F）。例如，\u03a3表示希腊字母Σ|

字符串长度可以通过字符串属性`length`获得

### 字符串的特点
字符串不可变，一旦创建，字符串的值就不能改变
### 转换为字符串
```
方法1：
几乎每个值都有的toString()方法，但是null和undefined没有。默认toString()是按照十进制格式返回数值的字符串表示，
通过传入的参数可以控制转换的进制
var num=10;
alert(num.toString());  //"10"
alert(num.toString(2));  //"1010"   二进制   
alert(num.toString(8));  //"12"       八进制
alert(num.toString(10));  //"10"  十进制
alert(num.toString(16));  //"a"  十六进制
```
方法2：使用String()
String()的转换规则如下：
1. 如果有toString()方法，则调用toString()并返回相应的结果
2. 如果值是null，则返回"null"
3. 如果值是undefined，则返回"undefined"
```
var value1=10;
var value2=true;
var value3=null;
var value4;
alert(String(vaule1));  //"10"
alert(String(vaule2));  //"true"
alert(String(vaule3));  //"null"
alert(String(vaule4));  //"undefined"
```

## Object 类型
ECMAScript中的对象其实是一组数据和功能的集合。
```
var o=new Object();
var o=new Object; // 有效，但不推荐
```
Object的实例都有下面的属性和方法:
```
constructor:构造函数，保留用户创建对象的函数
hasOwnProperty(propertyName):检查给定的属性propertyName是否在对象实例当中
isPrototypeOf(object):检查实例对象是否是当前对象的原型
propertyIsEnumerable(propertyName)检查属性是否可以使用for-in 语句来枚举
toLocalString():返回对象的字符串表示，对应执行环境的地区对应
toString():返回对象的字符串表示
vauleOf():返回对象的字符串，数值或者布尔值表示。一般和toString()返回结果相同
```
**ECMAScript中Object是所有对象的基础，所以所有对象都有这些基本属性和方法**

## 操作符
包含算术操作符，位操作符，关系操作符和相等操作符等等
### 一元操作符
只能操作一个值的
#### 递增和递减操作符
```
var age=29;
//以下两种等价
++age;
age=age+1;
//以下两种等价
--age;
age=age-1;
```
**前置递增或者递减有副效应，变量的值在语句被求值以前改变的，即前置递增或者递减操作符会让变量先改变，
然后再改变语句**
```
var age=29;
var anotherAge=--age+2;
alert(age);    //28,--29
alert(anotherAge);   //30, --29+2,28+2
```

