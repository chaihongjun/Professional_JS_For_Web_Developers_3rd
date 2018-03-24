[TOC]

# 面向对象的程序设计

面向对象(Object-Oriented ,OO)
**都有一个标志，有类的概念，通过类可以创建任意多个具有相同属性和方法的对象，ECMASCript没有类的概念，所以它的对象和基于类的语言中的对象有所不同。**

** ECMAScript-262把对象定义为:
>无序属性的集合，其属性可以包含基本值，对象或者函数，严格来讲，**对象是一组没有特定顺序的值**.我们可以把ECMAscript的对象想象成散列表：一组名值对，其中值可以说数据或者是函数


## 理解对象
早期是这样创建一个对象(通过Object实例化):
```
var person=new Object();
person.name="Nicholas";
person.age=29;
person.job="Software Engineer";

person.sayName=function(){
    alert(this.name);
}
```
现在对象字面量创建对象成为首选:
```
var person={
        name:"Nicholas",
        age:29,
        job:"Software Engineer",

        sayName: function(){
            alert(this.name);
        }
}
```

### 属性类型
**为了表示特征是内部值，语法规范使用`[]`。ECMAScript有两种属性:`数据属性`和`访问器属性`**

1. 数据属性
数据属性包的一个数据值的位置，这个位置可以读取和写入，数据属性有4个描述行为的特征
[Configurable]:表示能否通过delete删除属性从而重新定义属性，能否修改属性的特征，或者能否把属性改成访问器属性。默认是true
[Enumerable]:表示能否通过for-in循环返回属性，默认值true
[Writable]:表示能否修改属性的值。默认值true
[Value]:包含这个属性的属性值。读取属性值的时候，从这个位置读取；写入属性值的时候把新值保存在这个位置。默认值是undefined

前面的例子:
```
var person={
        name:"Nicholas",
        age:29,
        job:"Software Engineer",

        sayName: function(){
            alert(this.name);
        }
}
```
直接在对象上定义的属性,name,age,job等等，他们的[[Configurable]], [[Enumerable]] ,[[Writable]]特性值为true,而[[Value]]被指定为特定的值
比如name为Nicholas

**要修改属性默认的特性必须使用ECMAScript5的`Object.defineProperty()`方法,该方法接收3个参数：属性所在对象，属性名和一个描述符对象，
描述符（descriptor）对象的属性必须是configurable,enumerable,writable和value，设置其中一个或者多个**：
```
var person ={};
Object.defineProperty(person,"name",{
            writable:false,    //不可修改name属性值
            value:"Nicholas"
    });
alert(person.name); //Nicholas
person.name="Grey";      //不可修改name属性值
alert(person.name); //Nicholas
```
**把 configurable 设置为false,就不能从对象中删除属性，一旦把属性定义为不可配置就无法再变回可配置,调用`Object.defineProperty()`,如果不指定，configurable,enumerable,writable的特性默认值都是false**

2. 访问器属性
访问器属性不含数据值；包含一对`getter`和`setter`函数（这两个函数不是必须的），在读取访问器属性的时候会调用`getter函数`,这个函数负责返回有效的值；在写入访问器的属性时，会调用`setter函数`并传入新值，这个函数决定如何处理数据。访问器属性有以下4种特性：
[Configurable]:表示是否能通过delete删除属性从而重新定义属性，能否修改属性的特性或者能否把属性修改为数据属性。对于直接在对象上定义的属性，默认值true.
[Enumerable]:表示能否通过for-in循环返回属性。对于直接在对象上定义的属性，默认值true.
[Get]:在读取属性时调用。默认值undefined
[Set]:在写入属性时调用。默认值undefined

**访问器属性不能直接定义,必须使用`Object.defineProperty()`定义**
```
var book = {
      //定义book对象两个默认属性
    _year: 2004,    // "下划线"定义的属性方式用于表示只能通过对象方法访问的属性
    edition: 1
};
//定义访问器属性 year
Object.defineProperty(book, "year", {
    get: function() {
        return this._year;
    },
    set: function(newValue) {
        if (newValue > 2004) {
            this._year = newValue;
            this.edition += newValue - 2004;
        }
    }
});
book.year = 2005;
alert(book.edition); //2
```

不一定要同时指定setter和getter，只指定getter意味着属性是不可写的，只指定setter意味着属性不可读。
不支持Object.defineProperty()方法的浏览器，一般使用两个非标准的方法`__defineGetter__`和`__defineSetter__`:
```
var book={
    _year:2004,
    edition:1
};
//非标准方法:
book.__defineGetter__("year",function(){
        return this._year;
    });
   book.__defineSetter__("year",function(newValue){
        if(newValue>2004){
                this._year=newValue;
                this.edition+=newValue-2004;
        }
   }) ;
   book.year=2005;
   alert(book.edition); //2
```

### 定义多个属性

**为对象定义多个属性的可能性很大,ECMAScript5定义了一个方法`Object.defineProperties()`，利用这个方法可以通过描述符一次定义多个属性。**
这个方法接收两个参数(都是对象)：第一个参数是要添加修改属性的对象，第二个对象的属性是与第一个对象中要添加或者修改的属性一一对应：
```
var book={};
//给对象book 定义多个属性
Object.defineProperties(book,{
          //定义数据属性
              _year:{
                            writable:true,
                               value:2004 
              } ,
               //定义数据属性
              edition:{
                    writable:true,
                    value:1
              } ,
               //定义访问器属性
              year:{
                get:function(){
                            return this._year;
                },
                set:function(newValue){
                        if(newValue>2004){
                                    this._year=newValue;
                                    this.edition+=newValue-2004;
                        }
                 }
              }
    });
```

###读取属性的特征
**使用ECMAScript5的`Object.getOwnPropertyDescriptor()`方法，可以取得给定属性的描述符**
这个方法接收2个参数：属性所在的对象和和要读取其描述符的属性名称，返回值是一个对象，如果是访问器属性，这个对象有configurable,enumerable,get和set
;如果是数据属性，这个对象的属性有configurable,enumerable,writable和value：
```
var book={};
Object.defineProperties(book,{
                _year:{
                        value:2004
                },
                edition:{
                    value:1
                },
                year:{
                    get:function(){
                            return this._year;
                    },
                    set:function(newValue){
                        if(newValue>2004){
                                this._year=newValue;
                                this.edition+=newValue-2004;
                        }
                    }
                }
    });
    // {value: 2004, writable: false, enumerable: false, configurable: false}
    var descriptor = Object.getOwnPropertyDescriptor(book,"_year"); 
    alert(descriptor.value); //2004
    alert(descriptor.configuable); // false
    alert(typeof descriptor.get);

    //{_year: 2004, edition: 1}
    var descriptor = Object.getOwnPropertyDescriptor(book,"year"); 
    alert(descriptor.value); //undefined
    alert(descriptor.enumerable); // false
    alert(typeof descriptor.get); //function
```


## 创建对象
Object或者字面量可以创建单个对象，但是使用同一个接口创建很多对象会产生很多重复代码。

### 工厂模式
工厂模式抽象了创建具体对象的模式，用函数来封装以特定接口创建对象的细节：
```
function createPerson(name,age,job){
        var o=new Object();  //显示的创建对象
        o.name=name;
        o.age=age;
        o.job=job;
        o.sayName=function(){
                alert(this.name);
        };
        return o;
}
var person1=createPerson("Nicholas",29,"Software Engineer");
var person2=createPerson("Grey",27,"Doctor");
```
工厂模式解决了创建多个相似对象的问题，但没有解决对象识别问题（怎么知道一个对象的类型）

### 构造函数模式

Object，Arrary等原生构造函数，运行时uhi自动出现在执行环境。也可以自定义构造函数:
```
function Person(name,age,job){
    //没有显示的创建对象
        this.name=name;
        this.age=age;
        this.job=job;
        this.sayNama=function(){
                alert(this.name);
        };
}
var person1=new Person("Nicholas",29,"Software Engineer");
var person2=new Person("Grey",27,"Doctor");
```
创建Person的一个对象，需要new操作符。这样的方式调用构造函数有4个步骤：
1. 创建一个新对象
2. 将构造函数的作用域赋给新的对象（所以this指向了这个新的对象）
3. 执行构造函数中的代码(为这个新的对象添加属性)
4. 返回新的对象

1.将构造函数当作函数
**构造函数和其他函数唯一区别是调用方式不同，只要通过new操作符调用的就是构造函数**


2. 构造函数的问题




### 原型模式
**我们创建的函数都有一个`prototype`(原型)属性，这个属性是一个指针，指向一个对象，这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。**
**`prototype`就是通过调用构造函数而创建的那个对象实例的原型对象。**
**使用原型对象可以让所有对象实例共享它的属性和方法，不必在构造函数中定义对象实例信息。**
```
function Person(){
}
Person.prototype.name="Nicholas"; 
Person.prototype.age=29; 
Person.prototype.job="Software Engineer"; 
Person.prototype.sayName = function(){
            alert(this.name);
}
var person1=new Person();
person1.sayName(); // "Nicholas"

var person2=new Person();
person2.sayName();// "Nicholas"

alert(person1.sayName==person2.sayName); //true
```
Person所有属性和方法添加到了它的原型属性中，这些方法和属性属于Person构造对象共享


