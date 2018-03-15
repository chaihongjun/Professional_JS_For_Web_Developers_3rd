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
#### 前置递增和递减操作符 
```
var age=29;
//以下两种等价
++age;
age=age+1;
//以下两种等价
--age;
age=age-1;
```
**前置递增或者递减有副效应，变量的值在语句被求值以前改变的，即前置递增或者递减操作符会让变量先改变，然后再改变语句**
```
var age=29;
var anotherAge=--age+2;
alert(age);    //28,--29
alert(anotherAge);   //30, --29+2,28+2
```
#### 后置递增和递减操作符
**后置与递增或递减和前置非常重要的区别，后置的递增或递减操作在包含他们的语句被求值之后才执行。**
```
var num1=2;
var num2=20;
var num3=num1-- + nun2; //22,       2--  +20
var num4=num1+num2; //21,      1+ 20
```
对于不同值的递增或递减规则：
>1. 应用于数字字符串时，先转换成数值，再执行加减操作，最后保留为数值类型
2. 应用于非数字字符串时，无论加减，均返回`NaN`
3. 应用于布尔值时，先转换成数值0或者1（如果布尔值是false或者true），然后再加减操作，最后保留数值
4. 应用于对象时，先调用valueOf()方法，取得一个可操作的值，然后对于加减操作，如果返回的是NaN，则再调用toString()之后再次执行加减操作

```
var s1="2";
var s2="z";
var b=false;
var f=1.1;
var o={
    valueOf:function(){
        return -1;
    }
};
s1++;   //  s1的值3,"2"++ ->2++  
s2++;  // s2的值NaN,"z"++
b++;   // b的值1,fasle++ -> 0++
f--;  //f的值0.1xxxxxxxxx ,1.1--
o--;  // o的值 -2,  valueOf(-1)--  ->   -1--
```

### 位操作符
ECMAScript将64位值转换成32位操作，然后，再转换回64位。
有符号的整数：
第一位是符号位（0表示正数，1表示负数）
**想获得一个负整数，可以使用该对应正整数的“取反加一”方法**
>取反加一：
1. 首先将正整数所有位的数值0和1互相对换，0换成1，1换成0。（取反）
2. 然后在最后一位加一。（加一）

```
18的二进制：
0000 0000 0000 0000 0000 0000 0001 0010
取反：
1111 1111 1111 1111 1111 1111 1110 1101
加1：
1111 1111 1111 1111 1111 1111 1110 1101
+                                     1
得到-18的二进制：
1111 1111 1111 1111 1111 1111 1110 1110
```

#### 按位非由波浪线(~)表示(NOT)
**按位非由波浪线(~)表示,取得当前位的反码。按位非操作的本质是操作数的负值减一**
#### 按位与(AND)
**按位与由和符号(&)表示，两个操作数按位对齐，然后按照规则执行AND，只有对应的两个位都是1才返回1**
#### 按位或(OR)
**按位或由竖线符号(|)表示，两个操作数按位对齐，然后按照规则执行OR，对应的两个位只要有1个是1就返回1**
#### 按位异或(XOR)
**按位异或由插入符号(^)表示，两个操作数按位对齐，然后按照规则执行XOR，对应的两个位互异的时候才返回1**
#### 左移
**操作符号由两个小于号组成(<<)，所有的位向左移动指定的位数，右侧空出来的位由0填充，左移不影响符号位，符号位不变**
#### 有符号的右移
**操作符号由两个大于号组成(>>)，数值位向右移动指定的位数，左侧空出来的位由符号位的值填充，右移不影响符号位，符号位不变**
#### 无符号的右移
**操作符号由三个大于号组成(>>>)，所有的位向右移动指定的位数，左侧空缺由0填充**

### 布尔操作符
布尔操作符：NOT，AND，OR
#### 逻辑非(!)
由叹号(!)表示，对操作数取反
>1. 如果操作数是对象，则返回false
2. 如果操作数是一个空字符串，返回true
3. 如果操作数是非空字符串，返回false
4. 如果操作数是0，返回true
5. 如果操作数是任意非0数值，返回false
6. 如果操作数是null,返回true
7. 如果操作数是NaN，返回true
8. 如果操作数是undefined，返回true

```
alert(!false); // true
alert(!"blue");   //false
alert(!0);  //true
alert(!NaN);  //true
alert(!""); //true
alert(!12345); //false
```
#### 逻辑与(&&)
用两个和号表示（&&），需要两个操作数
**只有两个操作数的值都是true的时候，逻辑与才返回true**
>1. 如果第一个操作数是对象，则返回第二个操作数
2. 如果第二个操作数是对象，只有第二个操作数是true的时候，才返回对象
3. 如果两个操作数都是对象，则返回第二个操作数
4. 如果有一个操作数是null，则返回null
5. 如果有一个操作数是undefined，则返回undefined

#### 逻辑或(||)
用两个竖线符号（||）表示，需要两个操作数
**只要有一个操作数是true，逻辑或就返回true**
>1. 如果第一个操作数是对象，则返回第一个操作数
2. 如果第一操作数是false,则返回第二个操作数
3. 如果两个操作数都是对象，则返回第一个操作数
4. 如果两个操作数都是null，则返回null
5. 如果两个操作数都是NaN，则返回NaN
6. 如果两个操作数都是undefined，则返回undefined


### 乘性操作符
包含：乘法，除法和求模
#### 乘法
星号（*）表示
>1. 如果两个操作数是常规数值，则按照常规的乘法计算
2. 如果只有一个操作数是有符号的，则结果是负数
3. 如果有一个操作数是NaN，结果是NaN
4. Infinity*0=NaN
5. Infinity与非0数值相乘，结果是Infinity或者-Infinity
6. Infinity*Infinity=Infinity
7. 如果有一个操作数不是数值，则在后台通过Number()先转换，再操作


#### 除法
斜线（/）表示
>1. 如果两个操作数都是常规数值，则按照常规除法计算
2. 如果有一个操作数是NaN，结果是NaN
3. Infinity/Infinity=NaN
4. 0/0=NaN
5. 非零的数值除以0，结果是Infinity或者-Infinity  ： 12/0=Infinity
6. Infinity或者-Infinity除以非0的值，返回Infinity或者-Infinity   ：-Infinity/12=-Infinity
7. 0除以 Infinity或者 -Infinity，返回0或-0：  0/-Infinity=-0   

#### 求模（求余）
百分号（%）表示
>1. 如果操作数都是数值，则正常计算，返回余数
2. 如果被除数是Infinity或-Infinity，除数是有限数值，则返回NaN： Infinity%5=NaN
3. （+/-）Infinity%（+/-）Infinity=NaN
4. 如果被除数是有限大的数值，除数是0，则返回NaN： 15%0=NaN
5. 如果被除数是有限大的数值，除数是Infinity或者-Infinity，则返回被除数： -15%-Infinity=-15
6. 如果被除数是0，则返回0： 0%Infinity=0
7. 如果有一个操作数不是数值，则后台通过Number()转化后再计算



### 加减操作符
#### 加法
>1. 如果两个操作数都是常规数值，则按正常情况相加
2. 如果有一个操作数是NaN，则返回NaN
3. Infinity+Infinity=Infinity, -Infinity+-Infinity=-Infinity,Infinity+-Infinity=NaN
4. +0 + +0=+0, -0 + -0 = -0, +0 + -0= +0
5. 如果两个操作数都是字符串，则拼接
6. 如果只有一个是字符串，则将另外一个操作数（数值，布尔值，对象等）转换成字符串再拼接

#### 减法
>1. 如果两个操作数都是常规数值，则按正常情况相减
2. 如果有一个操作数是NaN，则返回NaN
3. Infinity-Infinity=NaN, -Infinity- -Infinity=NaN ,Infinity- -Infinity=Infinity ， -Infinity -Infinity=-Infinity
4. +0 - +0=+0, +0 - -0=-0，-0 - -0=+0
5. 如果有一个操作数是字符串，布尔值,null，undefined则先后台通过Number()转换再计算，如果转换之后是NaN，则结果就是NaN
6. 如果有一个是对象，则通过valueOf()取得对象值，计算。如果对象值是NaN，则计算结果就是NaN。如果没有valueOf()，则用toString()取得字符串转换成数值


###关系操作符
<,>,<=,>=
>1. 如果两个操作数都是数值，则正常比较
2. 如果两个操作数是字符串，则比较字符编码值
3. 如果有一个是数值，则将另外一个转换成数值再比较
4. 如果有一个是对象，则使用valueOf(),如果没有valueOf(),再使用toString()转换再比较
5. 如果有一个操作数是布尔值，则转换成数值再比较


### 相等操作符
重要的概念：
**相等和不相等——先转换再比较
全等和不全等——仅比较不做类型转换**

#### 相等和不相等
（==）（!=）
>1. 如果有一个操作数是布尔值，则先转换为数值，true转换为1，false转换为0
2. 如果有一个操作数是字符串，另外一个是数值，则先将字符串
3. null==undefined


#### 全等和全不等
全等和全不等不做类型转换直接比较
```
var result1=("55"==55); //true   相等
var result2=("55"===55); //false    不全等
var result3=("55"!=55); //false      转化后相等
var result3=("55"!==55); //true    不全等
```


### 条件操作符
#### 赋值操作符
等号（=）表示赋值，右侧的值赋给左侧的变量
##### 复合赋值操作符
```
*=
/=
+=
-=
%=
<<=
>>=
>>>=
```

### 逗号操作符
多用于变量声明

## 语句
### if语句
```
if(condition)
        statement1
else
        statement2
```
condition可以是任意的表达式，如果结果不是布尔值，ECMAScript会自动调用Boolean()进行转换，condition如果结果为true则执行statement1,否则执行statement2。
statement1和statement2可以是一行代码，也可以是语句块({}花括号包裹起来的多行代码)
```
if(i>25){
            alert("Greater than 25");
}
else{
            alert("Less than or equal to 25");
}
```

### do-while语句
do-while 是一种后测试的循环语句，只有在循环体内代码执行之后才会测试出口条件，也就是说，**循环体内的代码至少执行一次**
```
do{
    statement
}
while(expression);
```

### while 语句
while则和前面的不同，它会在循环体内的代码执行之前先做出口条件的测试，所以，**有可能循环体内的代码永远不会执行。**
```
while(expression){
            statement
}
```

###  for 语句
for 语句也是一种循环测试语句，它具有在执行循环之前初始化变量和定义循环后要执行的代码的能力。
```
for(initialization;expression;post-loop-exoression){
    statement
}
```
initialization初始化变量可以在for循环外部声明`var`:
```
var count=10;
for(var i=0;i<count;i++){
    alert(i);
}
alert(i);
//等同于
var count=10;
var i;
for(i=0;i<count;i++){
    alert(i);
}
alert(i);
```
**ECMAScript没有块级作用域，因此上面示例中for循环内的变量i,可以在循环外访问到**

###  for-in 语句
for-in 是精准的迭代语句，可以枚举对象的属性。
```
for (property in expression)
            statement
```
```
for (var proName in window){
            document.write(proName);  //循环输出window对象属性
}
```
由于对象的属性没有顺序，因此循环出的属性名的顺序是不可预测的。


### label语句
给代码添加标签，以便将来使用
```
label:statement
```
### break 和 continue 语句
break和continue 用来在循环中精确控制代码的执行。
**其中,break会立即退出循环，强制执行循环后面的语句，continue也是退出循环，但是会重循环顶部开始继续执行。**
```
var num=0;
for (var i=1;i<10;i++){
        if(i%5==0){

            break;            //当i的值可以被5整除的时候（0，5，10...）
                             // 终止执行循环体的代码，执行循环体后面的代码
                            //这里当i等于5的时候不再执行循环
        }                        
      num++;               //当i的值为（0，5，10...）的时候，跳过这里（属于循环体代码）
}
alert(num);   //       4  ，当i=5的时候就不再循环，因此只循环了4次
```
```
var num=0;
for (var i=1;i<10;i++){
        if(i%5==0){

            continue;            //当i的值可以被5整除的时候（0，5，10...）
                             // 回到循环体头部，开始新的循环
                            //这里等于直接跳到i++
        }                        
      num++;               //当i的值为（0，5，10...）的时候，跳过这里,回到循环头部
}
alert(num);   //      8 ，当i为5和10的时候少了两次循环体内的执行，所以叠加只有8次
```
### with语句
将代码的作用域设置到一个特定的对象中：
```
with(expression)
statement;
```
```
var gs=location.search.substring(1);
var hostname=location.hostname;
var url=location.href;
//等价于
with(location){
    var gs=search.substring(1);
    var hostname=hostname;
    var url=href;
}
```
严格模式下不允许使用with语句，而且性能不佳，调试困难，不建议使用with语句
### switch语句
```
switch (expression){
        case value1:statement1
            break;
        case value2:statement2
            break;
        case value3:statement3
            break;    
          default: default_statement   
}
```
 switch 语句case 后面的value1,value2... 可以是变量，常量，或者表达式
```
switch("hello world!"){
        case "hello"+" world!":
                alert("Greeting was found.");
                break;
        case "goodbye":
                alert("Closing was found.");
                break;
        default:
                alert("Unexpected message was found.");        
}
```
```
var num=25;
    switch(true){
            case num<0:
            alert("Less than 0");
            break;
            case num>=0 && num<=10:
            alert("Between 0 and 10");
            break;
            case num>10 && num>=20:
            alert("Between 10 and 20");
            break;
            default:
            alert("More than 20");
    }
```
Switch 语句在比较值时，使用的是全等操作符，不存在类型转换过程。

## 函数
function 作为关键字，封装了多条语句。函数声明：
```
function functionName(arg0,arg1,arg2...){
        statements
}
```
函数调用:
```
functionName(arg0,arg1,arg2...);
```
**ECMAScript的函数定义不必指定是否返回值。**如果需要返回值，则在函数体内添加return 语句，return 语句之后的代码永远不会执行
严格模式的一些规定：
>1. 不能把函数命名为`eval`或者`arguments`
2. 函数的参数不能命名为`eval`或者`arguments`
3. 不能出现两个命名参数同名的情况

### 理解参数
ECMAScript不介意传入参数的个数和类型。即，如果定义的函数只接收两个参数，但是调用的时候传入1个或者3个都是可以的。
因为，ECMAScript中的参数在内部是以一个数组的形式表示的。函数始终接收的是这个数组，通过`arguments`对象可以访问这个数组
`arguments`对象和数组类似（不是Array实例），可以通过`[]`语法访问每个元素（`arguments[x]`），通过`length`属性可以确定传入参数的个数。
`argument`的值永远和对应命名参数的值保存同步：
```
function doAdd(num1,num2){
        arguments[1]=10;
        alert(arguments[0]+num2);
}
```
以上例子中，无论doAdd函数运行几次，每次传入的arguments[1]的值都是10，从而对应的num2的值一直都会被重置为10.

### 没有重载
ECMASCript无法实现函数的重载，两个相同名字的函数，函数名只属于后定义的那个。如果想"间接实现重载"，可以使用判断传入参数的个数来实现：
```
function doAdd(){
    if(arguments.length==1){
         //如果传入的是1个参数
        }else if(arguments.length==2){
                //如果传入的是2个参数
        }
}
//通过函数传入参数个数的不同，间接实现函数重载
doAdd(1);
doAdd(1,2);
```

## 小结
>1. ECMAScript 基本数据类型：Undefined Null Boolean Number和String
2. ECMAScript 复杂数据类型：Object，是所有对象的基础
3. Number可以表示所有类型数值，包括整数和浮点数
4. 严格模式为任意出错的地方施加了限制
5. 包含基本操作符，算术操作符，布尔操作符，关系操作符，相等操作符和赋值操作符
6. if ,while,do-while,for,switch等流程控制语句
7. ECMAScript函数无需指定返回值，如果未指定，则返回的是undefined
8. ECMAScript没有函数签名概念，函数参数是一个数组形式的对象arguments
9. ECMAscript无法实现重载
[TOC]
