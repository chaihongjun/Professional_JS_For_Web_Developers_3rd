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
一张图来解释作用域链：
![作用域链](https://chaihongjun.github.io/Professional_JS_For_Web_Developers_3rd/chapter4/scope_chain.gif)


[TOC]
