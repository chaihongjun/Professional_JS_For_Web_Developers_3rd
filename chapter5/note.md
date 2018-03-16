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
