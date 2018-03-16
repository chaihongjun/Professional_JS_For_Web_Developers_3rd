[TOC]
# 引用类型
**引用类型的值（对象）是引用类型的一个实例，在ECMAScript中，引用类型是一种数据结构，用于将数据和功能组织起来。**看起来像是类，由于不具备类的面向对象支持的类和接口等基本结构，**所以引用类型被称为对象定义，因为它们描述的是一类对象所具有的属性和方法**


## Object类型
创建Object实例有两种方法：
```
// 方法1：通过构造函数
var person =new Obejct();
//添加属性
person.name="Nicholas";
person.age=29;
//方法2：通过对象字面量方法
var person={
    name:"Nicholas",
    age:29
};
//也可以写成这样
var person={};       //与 new Object() 相同
person.name="Nicholas";
person.age=29;
```
**对象字面量方法中通过逗号（,）来分隔不同的属性，最后一个属性后面不添加逗号**

**访问对象属性有两种方法，点`.`语法和`[]`语法**
```
1. 对象.属性
2. 对象["属性"]
person.name
person["name"]
```
使用`[]`方法的好处是可以通过变量访问属性：
```
var propertyName = name;
person[propertyName];
```
如果属性名包含会导致语法错误的字符，或属性名使用了关键字或保留字，则可以使用`[]`语法：
```
person["first name"];
```
"first name" 中间有空格，无法使用点语法。
通常，除非必须使用变量访问属性，一般建议使用点语法

## Array 类型
ECMAScript的数组，每一个项目可以保存任意类型的数据，而且数组的大小是可以动态调整
创建数组的基本方式有2种：
```
//第一种使用Array构造函数
var colors=new Array();
var colors=Array(10); //10个项目
//第二种使用的是数组字面量方法
var colors=["red","blue","green"];
var names=[];
 ```
读取和设置数组的值时，使用`[]`和相应的值基于0的数字索引，通过`length`属性可以`设置`和`读取`数组的项目个数。
未指定的数组项将用undefined


###检测数组
Array.isArray()方法可判断某个值是不是数组，而不管它是在哪个全局执行环境中创建的。
```
if(Array.isArray(value)){
    //对数组value操作
}
```
### 转换方法
所有对象都有toLocaleString(),toString()和valueOf()方法,。数组的转换字符串方法，将数组的每个值以字符串形式拼接，默认以逗号分隔。valueOf()返回的还是数组
```
var colors= ["red","blue","green"];
alert(colors.toString());    //red,blue,green
alert(colors.valueOf()); //red,blue,green
alert(colors);// red,blue,green  系统在后台调用toString() 
```
如果是使用join()拼接，则可以指定分隔符，join()默认的分隔符也是逗号：
```
var colors=["red","blue","green"];
alert(colors.join()); //red,blue,green
alert(colors.join("undefined")); //red,blue,green 
alert(colors.join("||"));  //red||blue||green
```

### 栈方法
**栈是一种LIFO（Last-In-First-Out,后进先出）的数据结构，最新添加的最早被删除。栈的压入和弹出都在栈的顶部，数据的出口和入口都是一个。**
ECMAScript专门为数组提供`push()`和`pop()`方法实现栈的行为。
**`push()`方法可接受任意数量的参数，将参数逐个添加到数组的末尾，并返回修改后的数组的长度。
`pop()`方法则是从数组的末尾移出最后一项，并减少了数组的`length`值，然后返回移出的那项**
**`push()`和`pop()`方法都是在数组的尾部操作**

```var colors=new Array() ; // 构造数组
var count = colors.push("red","green");  // colors=["red","green"],返回数组长度
alert(count); //2

count=colors.push("black"); // colors=["red","green","black"] 返回数组长度
alert(count); //3

var item=colors.pop(); //弹出black 弹出最后一个项，并返回它   colors=["red","green"]
alert(item);  //black
alert(colors.length); //2
```
### 队列方法
**队列数据结构的方法是FIFO（First-In-First-Out，先进先出），队列的列表在末尾添加项，在列表的前端移除项**
**`shift()`方法将数组的第一个项移除并返回这项，然后数组的长度减1。
`unshift()`方法将在数组头部添加任意个数组项，并返回新数组的长度。**
**`shift()`和`unshift()`方法都是在数组头部操作**
```
var colors=new Array();
var count=colors.push("red","green");// colors=["red","green"]
alert(count);//2

count=colors.push("black"); //colors=["red","green","black"];
alert(count); // 3

var item=colors.shift(); //弹出并返回 red  colors=["green","black"]
alert(item); //red
alert(colors.length); //2
```
```
var colors =new Array(); 
var count=colors.unshift("red","green");   //colors=["red","green"]
alert(count); //2

count = colors.unshift("black"); //colors=["black","red","green"]
alert(count); //3

var item=colors.pop();  // 弹出green  colors=["black","red"]
alert(item);// green
alert(colors.length); //2
```

### 重排序方法
数组已经有两个可直接使用的重排序方法`reverse()`和`sort()`
**`reverse()`方法反转数组的数组项顺序**
`sort()`方法默认情况下按照升序排列（最小的值在前面，最大的值在后面），sort方法会调用数组项目的toString()转换，然后比较。即使数组包含数值，**sort()比较的还是字符串**
sort()函数可以接收一个比较函数：
```
function compare(value1,value2){
    if(value1<value2){
        return 1;
    }else if(value1>value2){
        return -1;
    }else{
        return 0;
    }
var values=[0,1,5,10,15];
values.sort(compare);
alert(values); //15,10,5,1,0

//对于数值型或者valueOf()返回数值型的对象类型
//用第二个值减第一个值相比较
function compare(value1,value2){
    return value2-value1;
}



}
```
###操作方法

