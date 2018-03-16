[TOC]
# 变量，作用域和内存问题

## 基本类型和引用类型
**ECMAScript变量包含两种类型：`基本类型`和`引用类型`,基本类型是指`简单的数据段`，引用类型指那些可能由`多个值构成的对象`**
5种基本数据类型:undefuned,null,boolean,number,string是按照值访问的，对象类型无法直接操作对象的内存空间，操作对象实际是操作对象的引用。
**引用类型的值是按照引用访问的。**

### 动态的属性
引用类型变量可以添加或者删除属性和方法
```
var person=new Object();
person.name="Nicholas";
alert(person.name); //Nicholas
```
以上代码创建了一个对象并且保存在变量person里，然后添加了属性name并赋值，如果对象不被销毁，或者属性不被删除，这个属性将一直存在
但是，**基本类型无法添加属性或者方法。**
```
var name = "Nicholas";
name.age=27;
alert(name.age);  //undefined
```

### 复制变量值
**从一个变量向另外一个变量复制基本类型和引用类型值时，也是不一样的。**
*复制基本类型*
如果从一个变量向另外一个变量复制基本类型的值，会在新的变量上创建新的值。
```
var num1=5;
var num2=num1;
```
以上num1和num2都是完全独立的，两个变量参与的操作不会互相影响。
> 复制前的变量对象
> 
|  |  |  | 
| - | :-: | 
|  |  | 
| num1 | 5(Number类型) |

————————————————
>复制后的变量对象
>
|  |  |  | 
| - | :-: | 
| num2 | 5(Number类型) |
| num1 | 5(Number类型) |

*复制引用类型*
当从一个变量向另外一个变量复制引用类型的值的时候，另外一个变量保存的是前一个变量的指针。两个变量实际引用的是同一个对象。
因此，改变其中一个变量，就会影响到另外一个变量
```
var obj1=new Object();
var obj2=obj1;    //obj2引用obj1对象
obj1.name="Nicholas";
alert(obj2.name); //Nicholas
```
obj1和obj2指向的是同一个对象(共享内存空间)：
> 复制前的变量对象
> 
|  |  |  | 
| - | :-: | 
|  |  | 
| obj1 | (Objectr类型) |

——|——————————————
Object
——|——————————————
>复制后的变量对象
>
|  |  |  | 
| - | :-: | 
| obj2 | (Object类型) |
| obj1 | (Object类型) |


### 传递参数
**ECMAScript 中所有的函数的参数都是按值传递的**，也就是把函数外部的值复制给函数内部的参数，就和把值从一个变量复制到另外一个变量一样。
向参数传递基本类型的值时，被传递的值会被复制给一个局部变量。向参数传递引用类型的值时，会把这个值的地址复制给一个局部变量，所以，这个局部变量的变化会在函数外部反映出来
```
function addTen(num){
            num +=10;
            return num;
}
var count=20;
var result=addTen(count);
alert(count);   //20
alert(result);    //30
```
变量count的值20被复制给函数内部参数num,然后num变量又被加上10，num=20+10=30,然而，传入的参数count的值仍然是20没有变化。
另外一个关于对象的例子（仍然是值传递）
```
function setName(obj){
        obj.name="Nicholas";
}
var person=new Object();
setName(person);
alert(person.name);    //"Nicholas"
```
这个段代码，无法确定ECMAScript的函数参数传递是值传递还是引用传递。**值传递和引用传递的区别就是，两个参数其中一个参数发生了变化，那么另外一个参数如果没发生变化，则属于值传递，如果也同时发生了变化则是引用传递。**
为了验证和判断ECMAScript 中所有的函数的参数都是按值传递还是引用传递用下面的例子验证：
```
function setName(obj){
        obj.name="Nicholas";
         //下面开始有变化
         //函数内部参数obj重新指向另外一个对象
         obj =new Object();
         //对新的对象添加新的属性和值
         obj.name="Greg";
}
var person=new Object();
setName(person);
alert(person.name);    //"Nicholas"

```
以上代码解析：
>1. 首先实例化对象person
2. 执行setName(person),将对象person作为参数传入函数setName()
3. 执行函数setName的第一行代码，添加新的属性和值，这个时候person和obj都有属性name且值都为Nicholas,但是这个时候仍然无法判断person是obj的一个副本，还是指向共同的一个内存空间（引用）
4. 接着obj被指向了一个新的对象
5. obj指向的新对象添加了属性name和新的值
6. 如果函数的参数传递是值传递的，那么新的obj对象将被赋予新的堆地址，而person还是原来那个person
7. 如果函数的参数传递是引用传递的，那么新的obj指向新的堆地址了，同时person也会指向新的堆地址（和obj指向的地址是一致的），person也会发生改变
8. 最后输出person的结果验证了函数的参数传递是值传递的（obj变了，但是person没变）


### 检测类型
typeof 用来检测基本数据类型，Undefined，Null，Boolean，Number，String和复杂类型Object，但是想具体检测引用类型，则可以使用instanceof
```
var s="Nicholas";
var b=true;
var i=22;
var u;
var n=null;
var o=new Object();

alert(typeof s); //String
alert(typeof b); //Boolean
alert(typeof i); //String
alert(typeof u); //undefined
alert(typeof n); //Object
alert(typeof o); //Object

alert(o instanceof Object); //true

```

## 执行环境及作用域
**执行环境(execution context)决定了变量或者函数有权访问的其他数据，决定了变量或者函数的行为。**
**每个执行环境都有一个对应的`变量对象(variable object)`用来保存在这个环境中定义的变量和函数**这个变量对象我们无法访问，但是后台会调用。
全局执行执行环境是最外围的一个执行环境。ECMAScript实现所在宿主环境的不同，表示执行环境的对象也不一样。ECMAScript的宿主环境也就是运行环境主要有在客户端的的浏览器(如chrome ,firefox等)以及可以运行在服务端的(Nodejs)
在浏览器中，全局执行环境被认为是`windows对象`，所以所有的全局变量和函数都是作为windows对象的属性和方法存在。
当某一个环境的代码执行完毕之后，这个环境将被销毁，这个环境内的变量函数也会同时被销毁。（全局执行环境需要应用程序的推出才会销毁。比如关闭一个Tab或者关闭整个浏览器）

函数也有执行环境，当程序执行流进入一个函数的时候，函数的环境（可理解为一个菜篮子）会被推入一个栈（环境栈，可理解为购物车）。函数执行完毕之后，这个环境会被从栈中弹出，程序的控制权被返回给前面一个执行环境。

当代码在执行过程中，变量对象会创建一个`作用域链（scope chain）`，用来保证对执行环境有访问权限的变量和函数能够有序的访问。
几张图来解释作用域链：
![作用域链](https://chaihongjun.github.io/Professional_JS_For_Web_Developers_3rd/chapter4/scope_chain.gif)
![作用域链](https://chaihongjun.github.io/Professional_JS_For_Web_Developers_3rd/chapter4/scope_chain2.png)
```
function add(num1, num2){
   var sum = num1 + num2;
   return sum;
}
add(3,4);
```
第2张图片结合代码表明：
>1. 作用域链的前端是当前执行环境的变量对象（也就是当前执行环境内的变量和函数），绿色部分那里
2. 绿色部分称为活动对象(Activation object)，它的头部是首先是全局对象window,然后是函数参数对象arguments，接着是当前执行环境的变量
3. 活动对象后面的跟着外层环境的变量对象，直到最外层的全局变量对象
4. 全局变量对象是作用域链中最后一个对象

标识符的解析过程是从作用域链的前端开始，沿着作用域链一级一级的搜索，直到找到为止。
```
var color ="blue";
function changeColor(){
        if(color==="blue"){
                    color="red";
        }else{
                    color="blue";
        }
}
changeColor();
alert("Color is now "+color);
```

| changeColor()  scope chain |
|------------------------------|
|   arguments                        |
|    全局变量 color                           |
可以在changeColor()访问变量color，是因为changeColor()的作用域链可以找到它。


**在局部作用域定义的变量可以在局部环境中与全局变量互换使用。**
```
var color="blue";                                               //0:全局环境 只能访问 color                                       
function changeColor(){                                         //1: changeColor() 环境 可以访问 color,anotherColor
            var anotherColor="red";
            function swapColors(){                               //2:swapColor()环境 可以访问color,anotherColor,tempColor
                    var tempColor=anotherColor;
                    anotherColor=color;
                    color=tempColor;
                    //可以访问color,anotherColor,tempColor
            }
                //可以访问color,anotherColor
                swapColors();
}
//  只能访问color;
changeColor();
```
内部的执行环境可以通过作用域链访问外层环境，但是外层环境无法访问内部环境的变量和函数，每个环境都可以向上搜索作用域链，以查询变量和函数，但是任何环境都不能通过作用域链向下查询进入另外一个环境。
window
>
>----------color
>
>
>----------changeColor()
>>
>----------anotherColor
>
>----------swapColors()
>>>
----------tempColor




### 延长作用域链
两种场景会延长作用域链：
1. try-catch语句执行到catch语句
2. 使用with语句
以上执行的时候会在作用域链的前端添加一个变量对象



###　没有块级作用域
JS没有块级的作用域，`{}`内的变量在外面依然可以访问
```
if(true){
        var color="blue";
}
alert(color);   //"blue"
```
以上代码如果在C,java之类有块级作用域的语言里，则不会输出blue,因为在if的语句块内没有输出就被销毁了。而在ECMAScript环境下，color的赋值保留在了当前的执行环境中，这里是全局变量。特别注意for循环:
```
for(var i=0;i<10;i++){
    doSomething(i);
}
alert(i);  //10    
```
for 循环创建的变量直到循环结束，依然保留在循环外部的环境中。
#### 声明变量
var 声明的变量会添加到最接近的执行环境，在函数内，最近接的环境就是函数的局部环境；with语句中最接近的环境就是函数环境，如果初始化的时候没有用`var`,则变量被添加到全局环境中。严格模式下，初始化未声明的变量会导致错误
```
function add(num1.num2){
            var sum=num1+num2;
            return sum;
}
var result=add(10,20); //30
alert(sum); // 报错，因为在add函数内声明的局部变量，无法在外层访问
```

#### 查询标识符
在某个环境中为了读取或者写入而引用一个标识符的时候，必须通过搜索来确定这个标识符实际代表的是什么。从作用域链的前端开始搜索，向上逐级查询，直到全局环境。
```
var color="blue";
function getColor(){
            return color;
}
alert(getColor()); //"blue"
```
上面的代码执行分析:
>1. 首先调用函数getColor()
2. getColor()的作用是返回标识符color
3. 在getColor()函数执行环境内,当前不知道color是一个变量还是函数还是属性，于是向上查询
4. 在外层查询到了color的声明，color是一个变量，而且也赋值了。
5. 最后弹出变量color的值

当然，如果在向上的搜索过程中发现了同名的标识符，则会中断查询过程:
```
var color="blue";
function getColor(){
            var color="red";
            return color;
}
alert(getColor()); //"red"
```
上述代码函数内的变量声明属于局部变量，局部变量会被先调用。函数返回标识符首先在局部环境内查找，因此，外层的全局变量不会被使用到

## 垃圾收集
JS有自动的垃圾收集机制，无需开发人员关心，JS的垃圾收集机制会按照固定的时间间隔周期性的检查不再使用的变量，并释放其占用的空间。

### 标记清除
javascript 常用的垃圾收集方式是mark-and-sweep，标记清除
### 引用计数
### 性能问题
### 管理内存

## 小结
1. JS变量可以保存两种类型的值：基本类型和引用类型
2. 基本类型值：Undefined，Null，Boolean，Number，String
3. 基本类型值占用的空间上固定大小的，所以被保存在栈中
4. 引用类型的值是对象，所以是保持在堆中
5. 从一个变量向另外一个变量复制基本类型的值。会创建这个值的副本
6. 包含引用类型值得变量实际包含的不是对象本身，而是对象在堆中的地址
7. 从一个变量向另外一个变量复制引用类型的值，复制的实际是指针（内存地址），所以两个变量指向的是同一个对象
8. 确定一个值是属于哪种类型可以使用typeof操作符，要想确认一个值是哪种引用类型可以使用instanceof操作符
所以的变量都存在于一个执行环境（作用域）中，这个执行环境决定了变量的声明周期，以及被访问的权限。
关于执行环境：
1. 执行环境分全局执行环境（全局环境）和函数执行环境
2. 每次进入一个执行环境，会创建一个用于搜索变量和函数的作用域链
3. 函数的局部环境不仅可以访问函数作用域中的变量，而且也可以访问包含它的（父）环境，甚至全局环境
4. 全局环境只能访问全局环境中定义的变量和函数，不能直接访问局部环境内的任何数据
5. 变量的执行环境有助于确定何时释放内存
6. 垃圾回收机制（略过）


[TOC]
