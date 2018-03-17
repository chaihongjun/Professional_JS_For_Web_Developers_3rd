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

**`concat()`方法默认创建数组的副本,如果传递给concat()的是一个或者多个数组，则该方法会将数组中的项添加到结果数组中。**
```
var colors=["red","green","blue"];
var colors2=colors.concat("yellow",["black","brown]);

alert(colors);   //   red,green,blue
alert(colors2);     //    red,green,blue,yellow,black,brown
```

**`slice()`基于当前数组的一个或者多个项创建新的数组，slice()可接受一个或者两个参数，分别表示返回项的起始和结束位置。如果只有一个参数，则表示起始位置到数组的末尾。如果是两个参数，则返回起始和结束位置直接的项，但不包含结束位的项。slice()不影响原始数组**
```
var colors=["red","green","blue","yellow","purple"];
var colors2=colors.slice(1);    //从索引位置1开始到最后
var colors3=colors.slice(1,4) ;//  从索引1开始，到索引4前面的值（不含索引4的值）

alert(colors2); //green,blue,yellow,purple
alert(colors3);//green,blue,yellow
```
**如果slice()的参数有负数，则用数组的长度加上这个负数值来确定相应的位置：**
```
var colors=["red","green","blue","yellow","purple"];
slice(-2,-1)  相当于 slice(5-2,5-1) 及slice(3,4)
```

**`splice()`主要用来向数组中添加数组项，但是实际可以`删除`,`插入`,`替换`数组元素的操作**
1.  可以删除任意数量的项（连续的），只需指定2个参数，第一个参数是要删除的第一个项的位置，第二个是项的数量
```
var colors=["red","green","blue","yellow","purple"];
colors.splice(0,2);  //意味着删除数组中的前面2个

alert(colors);  //    blue,yellow,purple
```
2. 向数组的指定位置插入元素，只需要3个参数（起始位置，0，要插入的项），如果要插入多个项，再传入第4个甚至更多个
```
var colors=["red","green","blue","yellow","purple"];
colors.splice(2,0,"black","gray");     //意味着在数组的第二个项后面插入元素

alert(colors);  //red,green,black,gray,blue,yellow,purple
```
3. 替换数组内容，只需要3个参数(起始位置，需要删除的项个数，要插入的项)
```
var colors=["red","green","blue","yellow","purple"];
colors.splice(2,3,"black","gray");     //从索引位置2开始连续删除3个项，然后插入新的项

alert(colors);  //red,green,black,gray
```



### 位置方法
**`indexOf()`和`lastIndexOf()`位置方法，有两个参数，第一个是需要查找的项，第二个是可选择的查找的起点位置索引,两个查找方向分别是从数组头部和尾部**
返回值为查找项的位置，如果没找到则返回-1,且查询过程使用严格相等模式(===)
```
var numbers=[1,2,3,4,5,4,3,2,1];
alert(numbers.indexOf(4));  //3，   4的索引值
alert(numbers.lastIndexOf(4));  //5，   最后那个4的索引值

alert(numbers.indexOf(4，4));  //5，   查找项4，从索引值4位置开始查询
alert(numbers.lastIndexOf(4，4));  //3，   查找项4，从索引值导数4位置开始查询

var person= {name:"Nicholas"};
var people= [{name:"Nicholas"}];

var morePeople = [person];

alert(people.indexOf(person));//  -1  查询不到
alert(morePeople.indexOf(person));//   0

```

### 迭代方法
ECMAScript有5种针对数组的迭代方法：every(),filter(),forEach(),map(),some()
这些方法都有两个参数，分别是数组项需要执行的函数和运行这个函数的作用域对象（可选的），会影响this的值。
这个被执行的指定的函数有3个参数（item,index,array）分别是数组项，数组索引，和数组对象本身，5个方法的区别:
>1. every()对所有数组项执行指定函数都返回true，则every()返回true。
2. filter()对所有数组执行指定的函数，返回的是函数返回值是true的数组项组成的数组
3. forEach() 只执行指定函数，没有返回值
4. map() 对数组项执行指定函数，并将返回结果组成数组
5. some()对所有的数组项执行指定的函数，只要有一个数组项的执行结果是true，该方法就返回true。

以上方法不会改变数组值

`every()`和`some()` 返回的是布尔值，`filter()`和`map()`返回的是新的数组，forEach没有返回值。
```
var numbers=[1,2,3,4,5,4,3,2,1];

var everyResult=numbers.every(function(item,index,array){
                return (item>2);
    });
    alert(everyResult);  // false ,不是所有数组项都大于2

var someResult=numbers.some(function(item,index,array){
                            return (item>2);
    });
alert(someResult);  // true, 数组有大于2的项

var filterResult=numbers.filter(function(item,index,array){
                    return (item>2);
    });
alert(filterResult); // [3,4,5,4,3]    ，将符合条件（数组项大于2）的数组项组合成新的数组


var mapResult=numbers.map(function(item,index,array){
                return item*2;
    });

alert(mapResult); // [2,4,6,8,10,8,6,4,2]

```

### 归并方法
**`reduce()`和`reduceRight()`迭代数组并构建一个最终的返回值，reduce从数组第一项开始逐个遍历到最后，reduceRight则从最后一项开始直到第一个项**
这两个方法都接收2个参数：在数组项上执行的函数和作为归并基础的初始值（可选的）。
传给这2个方法的函数接收4个函数(前一个值，当前值，索引，数组对象本身)
**prev与cur经过处理之后传给prev,cur指向后面一个数组项，依此类推**
```
var values=[1,2,3,4,5];
var sum=values.reduce(function(prev,cur,index,array){
       console.log(prev);
       console.log(cur);  
       console.log(prev+cur);
       console.log("-----------------");
       return   prev+cur;    
    });
    console.log(sum); //15
```
**reduce与reduceRight除了从哪里开始（数组头部还是尾部）不一样之外，其他都一样。**

## Date类型
Date类型的日期范围在1970年1月1日之前或之后100000000年
```
创建当前日期对象：
var now=new Date();
```
Date.parse()接收一个表示日期的字符串，并将该字符串转换成对应的日期毫秒数
Date.now() 表示调用这个方法的当前时间:
```
//取得开始时间
var start =Date.now();
doSomething();
//取得停止时间
var stop=Date.now();
//doSomething的函数执行消耗时间
result=stop - start;
```
UTC：世界标准时间


### 继承的方法
Date类重写了toString(),toLocaleString(),valueOf()
toString()和toLocaleString()在各个浏览器下输出的内容不相同，
valueOf()只返回时间日期的毫秒数


### 日期格式化方法
```
toDateString(),以特定的格式显示星期几，月，日和年
toTimeString(),以特定的格式显示时，分，秒和时区
toLocaleDateString(),以特定于地区的格式显示星期几，悦，日和年
toLocaleTimeString(),以特定于地区的格式显示时，分，秒
toUTCString(),以特定的格式显示完整的UTC日期
```

###日期／时间组件方法

略过

##RegExp 类型
RegExp类型支持正则表达式
```
var expression = /pattern/ flags;
```
>`pattern` 模式部分可以说任意简单或复杂的正则表达式，可包含字符类，限定符，分组，向前查找及反向引用。
每个正则表达式都可以带一个或者多个标志`flags`，用以表明正则表达式的行为。
`flags`可以有3种模式：
1. `g` 表示全局`global` 模式，将模式应用于所有的字符串，而非在发现第一个匹配项时立即停止。
2. `i` 表示不区分大小写(case-insensitive)模式，即在匹配项时忽略模式和字符串的大小写
3. `m`表示多行（multiline）模式，即在到达一行文本末尾时还会继续查找下一行中是否存在匹配的项。

**所以一个正则表达式就是一个匹配模式与3种标志的组合**

```
//匹配字符串中所有"at"的实例
var pattern1=/at/g;

//匹配第一个bat或者cat，而且忽略大小写
var pattern2=/[bc]at/i;

//匹配字符串中以"at"结尾的字符组合，且忽略大小写
var pattern3=/.at/gi;
```
必须转移的元字符：
```
( [ { \ ^ $ | ) ? * + . ] }
```

