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
必须转义的元字符：
`(`  `[` `{` `\` `^` `$` `|` `)` `?` `*` `+` `.` `]` `}`
```
//匹配第一个"bat"或者"cat",不区分大小写
var pattern1=/[bc]at/i;
//匹配第一个" [bc]at",不区分大小写
var pattern2= / \[bc\]at/i; 
//匹配所有以"at"结尾的3个字符的组合，不区分大小写
var pattern3=/.at/gi;
//匹配所有".at"，不区分大小写
var pattern4=/\.at/gi;
```
## Function 类型
###没有重载
```
function addSomeNumber (num){
    return num+100;
}
function addSomeNumber (num){
    return num+200;
}
var result=addSomeNumber(100);
```
两个同名函数，后面的函数会覆盖前面的函数

###函数声明与函数表达式
解析器会先读取函数声明，使其在执行任何代码前之前可用（可访问）；函数表达式必须等到解析器执行到它所在的代码行才会被解释执行：
```
alert(sum(10,10));
function sum(num1,num2){
            return nim+1num2;
}
```
以上代码可以正常运行。因为解析器通过`(function declaration hoisting)函数声明提升`过程，读取并将函数声明添加到执行环境中。对代码求职时，JS引擎第一遍会声明函数并将它们放到源码树的顶部。
所以，即使函数声明的代码在调用代码的后面，JS引擎也会把函数声明提升到顶部。
```
alert(sum(10,10));
var sum=function(num1,num2){
        return num1+num2;
}
```
以上代码会报错，因为sum函数不是声明式，而在一个赋值表达式中。

###　作为值的函数
```
function createComparisonFunction(propertyName){
        return function(object1,object2){
             var value1=object1[propertyName];
             var value2=object2[propertyName];

             if(value1<value2){
                    return -1;
             }else if(value1>value2){
                    return 1;
             }else{
                    return 0;
             }
        }
}

var data=[{name:"Zachary",age:28},{name:"Nicholas",age:29}];
data.sort(createComparisonFunction("name"));
alert(data[0].name);  //Nicholas

data.sort(createComparisonFunction("age"));
alert(data[0].name);  //Zachary
```
### 函数内部属性
函数内有两个特殊的对象:`arguments`和`this`。`arguments`对象包含传入函数的所有参数，`arguments`对象有一个属性`callee`，这个属性是一个指针，指向拥有`arguments`对象的函数
```
//阶乘函数
function factorial(num){
        if(num<=1){
                return 1;
        }else{
            return num*factorial(num-1);
        }
}
```
阶乘函数一般都要用到递归算法，上面的代码，函数的执行和函数名耦合在一起，可以使用`arguments.callee`消除耦合：
```
function factorial(num){
        if(num<=1){
                return 1;
        }else{
            return num*arguments.callee(num-1);
        }
}
```
函数内另外一个特殊的对象是`this`，引用的是函数执行的环境对象。
```
window.color="red";
var o={color:"blue"};

function sayColor(){
    alert(this.color);
}
sayColor(); // "red"

o.sayColor=sayColor;    //sayColor()函数指针赋给对象o的新的属性
o.sayColor(); //  "blue"
```

**sayColor作为函数名仅仅是包含指针的变量，全局的sayColor()函数与o.sayColor属性指向同一个函数**
**ECMAScript5也规范了另外一个函数对象属性`caller`,它保存了调用当前函数的函数引用，如果是在全局作用域中调用当前函数，则它的值为null**
```
function outer(){
     inner();
}
function inner(){
    alert(inner.caller);  //弹窗显示outer()的源代码，因为outer()调用了inner(),所以inner.caller指向outer()
}
outer();
```
###函数属性和方法
ECMAScript中的函数也是对象，所以函数也有属性和方法。**每个函数都有两个属性:`length`和`prototype`,`lenght`表示函数希望接收的命名参数的个数**
```
function sayName(name){
        alert(name);
}
function sum(num1,num2){
        return num1+num2;
}
function sayHi(){
        alert("hi");
}
alert(sayName.length); //1
alert(sum.length);   //2
alert(sayHi.length);  //0
```
**对于ECMASCript的引用类型而言，`prototype`保存了引用类型实例方法，toString()和valueOf()实际保存在prototype名下**
ECMAScript中prototype的属性是不可枚举的。
**每个函数都包含两个非继承的方法:`apply()`和`call()`，用于在特定作用域中调用函数，实际等于设置函数体内this对象值,用于改变`this`指向的对象**
`apply()`方法接收两个参数：一个是在其中运行函数的作用域，另外一个是参数数组。
```
function sum(num1,num2){
        return num1+num2;
}
function callSum1(num1,num2){
            return sum.apply(this,arguments); //传入arguments对象 。this为全局对象window
}
function callSum2(num1,num2){
            return sum.apply(this,[num1,num2]); //传入数组 . this为全局对象window 
}
alert(callSum1(10,10)); //20
alert(callSum2(10,10));  //20
```
```
function sum(num1,num2){
        return num1+num2;
}
function callSum(num1,num2){
        return sum.call(this,sum1,num2);   //  call的参数需要一个一个的传入，与apply()不同
}
alert(callSum(10,10));  // 20
```
**`call()`和`apply()`强大的地方是扩展函数的运行作用域**
```
window.color="red";
var o={color:"blue"};
function sayColor(){
    alert(this.color);
}
sayColor();                              //red, 当前的this 作用域就是全局window
sayColor.call(this);               //red,同上
sayColor.call(window);     //red,指定了window对象调用sayColor函数
sayColor.call(o);                  //blue,指定了o对象调用sayColor函数，所以sayColor函数内this指向o对象
```
ECMAScript5还定义了另外一个方法`bind()`,bind()方法创建一个新的函数, 当被调用时，将其this关键字设置为提供的值，在调用新函数时，在任何提供之前提供一个给定的参数序列
```
window.color="red";
var o={color:"blue"};
function sayColor(){
        alert(this.color);
}
var objectSayColor=sayColor.bind(o);   //这里的this就是对象o
objectSayColor(); //blue;
```
## 基本包装类型
![Javascript类型](https://chaihongjun.github.io/Professional_JS_For_Web_Developers_3rd/chapter5/jstype.jpg)
引用类型中有3个特殊的类型：Boolean,Number,String属于基本包装类型
```
var s1="some text";
var s2=s1.substring(2);
//相当于后台处理为（后台隐式的操作）
var s1=new String("some text");
var s2=s1.substring(2);
s1=null; //前面代码执行完毕之后，生命周期完结对象立即被销毁
```
基本包装类型和引用类型主要区别就是对象的生存周期，new操作创建的引用类型实例，在执行流代码离开当前作用域之前都一直保存在内存中。
自动创建的基本包装类型则只存在于一瞬间。**所以，无法给基本包装类型添加属性和方法。**
另外，基本包装类型调用`typeof`会返回`object`,所有基本包装类型的对象都会转换为`true`.
基本包装类型构造函数与同名转型函数不一样:
```
var value="25";

var obj=new Number(value);
alert(typeof obj);//object

var number=Number(value);
alert(typeof number); // number

```

### Boolean 类型
构造布尔值对象
```
var booleanObject=new Boolean(true);
```
### Number类型
number是与数字值对应的引用类型。
```
var numberObject =new Number(10);
```
**number类型一个将数值格式化为字符串的方法`toFixed()`，会按照指定的小数位返回数值的字符串表示(能够自动舍入，但是各个浏览器的表现会有不同)：**
```
var num=10;
alert(num.toFixed(2)); //"10.00"
```
**另外可格式化数值的方法`toExponential()`，返回以指数表示法(e表示法)表示的数值的字符串形式**
```
var num=10;
alert(num.toExponential(1)); //"1.0e+1"
```
**如果想表示某个数值的最合适的格式，应该使用`toPrecision()`方法：**
```
var num=99;
alert(num.toPrecision(1)); // "1e+2"
alert(num.toPrecision(2)); // "99"
alert(num.toPrecision(3)); // "99.0"
```

### String 类型
String类型是字符串的包装类型
```
var stringObject =new String("hello world");
```
1. 字符方法
用于访问字符串中特定的字符的方法:`charAt()`和`charCodeAt()`，这两个方法都接收一个参数，基于0 的字符位置。
`charAt()`方法以单字符形式返回给定位置的字符
```
var stringValue ="hello world";
alert(stringValue.charAt(1)); //"e"
```
如果想得到的是字符编码，则使用`charCodeAt()`:
```
var stringValue="hello world";
alert(stringValue.charCodeAt(1));// 101， 返回位置1的字符编码，即字母"e"的编码
```
ECMAScript5还定义了另外一个访问个别字符的方法。**可以通过[]语法加数字索引来访问字符串中特定的字符**
```
var stringValue=hello world";
alert(stringValue[1]); //"e"
```
2. 字符串操作方法
`concat()`用于将一个或多个字符串拼接起来，返回新字符串,对原来的字符串没有影响:
```
var stringValue="hello ";
var result=stringValue.concat("world");
alert(result); //"hello world"
alert(stringValue); //"hello "
```
ECMAScript还提供了三个基于子字符串创建新字符串的方法：`slice()`，`substr()`，`substring()`,都返回被操作的字符串的一个子字符串。
都接收两个参数：
第一个参数是起始位置，第二个参数是表示到哪里结束
```
stringObj.slice(start, [end])
//slice 可以对数组操作，substring不可以
//如果起始位置是负值则
stringObj.slice(length+start, length+[end])
stringObj.substring(0);
-------------------------------------------------
stringObj.substring(start,end);
stringObj.substr(start,length);
// start表示的是基于0的起始的位置
//end 表示的是基于0的结束的位置，但是不包含end
//length 表示要返回的字符串的长度


```
```
var stringValue="hello world";
alert(stringValue.slice(3));  //"lo world"
alert(stringValue.substring(3));  //"lo world"
alert(stringValue.substr(3));  //"lo world"
alert(stringValue.slice(3,7));  //"lo w"
alert(stringValue.substring(3,7));  //"lo w"
alert(stringValue.substr(3,7));  //"lo worl"
```
3. 字符串位置方法
**有两个可以从字符串中查找子字符串的方法：`indexOf()`和`lastIndexOf()`，返回子字符串的位置，如果没有则返回-1，indexOf()从字符串开头向后面搜索，lastIndexOf()则相反**
这两个方法也可以接受第二个参数:
```
var stringValue="hello world";
alert(stringValue.indexOf("o",6)); //7 , 从位置6开始向后搜索
alert(stringValue.lastIndexOf("o",6));  //4,从位置6开始向前搜索
```
```
var stringValue="Lorem ipsum dolor sit amet, consectetur adipisicing elit";
var positions =new Array();
//第一个"e"位置
var pos=stringValue.indexOf("e");
//将所有"e"的位置，依次放入新的数组
while(pos>-1){
            positions.push(pos);
            pos=stringValue.indexOf("e",pos+1);
}
alert(positions);
```
4. trim()方法
ECMAScript5定义了`trim()`方法，会创建字符串副本，清除字符串前置和后置的空格。
5. 字符串大小写转换方法
```
toLowerCase() //转换成小写
toLocaleLowerCase()
toUpperCase()  //转换成大写
toLocaleUpperCase()
```
6. 字符串的模式匹配方法
**`match()`方法只接收一个参数，要么是正则表达式，要么是RegExp对象(match()`本质上和RegExp的exec()是一致的):**
```
var text="cat,bat,sat,fat";
var pattern=/.at/;

//与pattern.exec(text)相同
var matches=text.match(pattern);
alert(matches.index);
alert(matches[0]);
alert(pattern.lastIndex);
```
**另外一个查找模式方法是`search()`,返回第一个匹配项的索引，如果没有找到则返回-1，`search()`始终是从字符串开头向后查找**
```
var   text="cat ,bat,sat,fat";
var pos=text.search(/at/);
alert(pos);  //1 ,at 第一次出现的位置
```
另外，ECMAScript提供了`replace()`方法，接受两个参数，第一个参数可以是RegExp对象或者一个字符串（这个字符串不会转换成正则表达式），第二个参数可以是字符串或者函数。
如果第一个参数是字符串，就会替换第一个子字符串，如果要替换全部的子字符串，那就要提供正则表达式，而且指定全局(g)标志:
```
var text="cat,bat,sat,fat";
var result=text.replace("at","ond"); //"cond,bat,sat,fat",将字符串中第一次出现的"at"替换为"ond"

result=text.replace(/at/g,"ond");/ "cond,bond,sond,fond", 将字符串中全部的"at"换成"ond"
```
第二个参数如果是字符串，可以使用一些特殊的字符序列:

|字符序列 | 替换文本|
|--- | ---|
|$$ | $|
| $& | 匹配整个模式的子字符串。与RegExp.lastMatch的值相同|
| $'|  匹配的子字符串之前的子字符串。与RegExp.leftContext的值相同|
|$`| 匹配的子字符串之后的子字符串。与RegExp.rightContext的值相同|
|$n| 匹配第n个捕获的子字符串，其中n等于0~9。例如，$1匹配的是第一个捕获组的子字符串。如果正则表达式没有定义捕获组，则使用空字符串|
|$nn| 匹配第nn个捕获的子字符串，其中nn等于01～99.例如，$01是匹配第一个捕获组的子字符串。如果正则表达式没有定义捕获组，则使用空字符串|

```
var text="cat,bat,sat,fat";
result=text.replace(/(.at)/g,"word($1)");
alert(result);
```
......


7.localeCompare()方法
`localeCompare()`方法比较两个字符串
```
stringA.localeCompare(stringB)
```
如果StringB在字母表中排在stringA前面,则返回1
如果StringB在字母表中排在stringA后面,则返回-1
如果stringA等于stringB，则返回0

8 .fromCharCode()方法
接收一个或者多个字符编码，然后将它们转换成字符串，本质上与`charCodeAt()`是相反操作
```
alert(String.fromCharCode(104,101,108,108,111)); //"hello"
```

9. HTML方法

......

## 单体内置对象
**内置对象:由ECMAScript实现提供不依赖宿主环境的对象，这些对象在ECMAScript程序执行前就已经存在**
开发人员无需显示实例化内置对象，它们已经实例化了。
Object,Array,String,
### Global对象
不属于任何其他对象的属性和方法，都是Global对象的属性和方法
1. URI编码方法
Global对象的`encodeURI()`和`encodeURIComponent()`方法可以对URI进行编码，以便发给浏览器，有效的URI不能包含某些字符，如空格。
**`encodeURI()`主要用于整个URI，`encodeURIComponent()`用于针对URI的某一段，`encodeURI()`不会对本身属于URI的特殊字符进行编码，比如冒号，正斜杠，问号和井号；而`encodeURIComponent()`会对它发现的任何非标准字符进行编码**：
```
var uri="http://www.wrox.com/illegal value.htm#srtart";

//"http://www.wrox.com/illegal%20value.htm#srtart"
alert(encodeURI(uri));

//"http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.htm%23srtart"
alert(encodeURIComponent(uri));
```

**使用`encodeURI()`编码之后的结果是除了空格之外的字符都原封不动，只有空格被替换成%20。`encodeURIComponet()`会将除`字母`和`数字`之外的数值进行编码替换**
**和`encodeURI() encodeURIComponent` 对应的方法是`decodeURI() decodeURIComponent()`,对前面编码的字符做解码**
2.eval()方法
`eval()`就像完整的ECMAScript解析器，只接收一个参数，要执行的ECMAScript或者JavaScript字符串:
```
eval("alert('hi')");
//等价于
alert("hi");
```
**当解析器发现代码调用`eval()`方法时，它会将传入的参数当作实际的ECMASCript语句来解析，然后把执行结果插入到原来的位置。通过`eval()`执行的代码被认为是包含该次调用的执行环境的一部分，所以，被执行的代码有与该执行环境相同的作用域链。通过`eval()`执行的代码可以引用在闭包环境中定义的变量**:
```
//外部定义变量
var msg="hello world";
//eval()里面的代码被替换为真正执行的代码
eval("alert('msg')");   //"hello world"
```
**在`eval()`中创建的变量或者函数不会被提升，因为，在解析代码的时候，它们被包含在一个字符串中，它们只在eval()执行的时候创建**

3.Global对象的属性

| 属性 | 说明 | 属性 | 说明|
|---|---|---|---|
| undefined | 特殊值 undefined| Date | 构造函数Date|
| NaN | 特殊值 NaN | RegExp| 构造函数 RegExp |
| Infinity | 特殊值 Infinity | Error | 构造函数 Error |
| Object | 构造函数 Object | EvalError | 构造函数 EvalError |
| Array | 构造函数 Array | RangeError | 构造函数 RangeError |
| Function | 构造函数 Function | ReferenceError | 构造函数 ReferenceError |
| Boolean | 构造函数 Boolean | SyntaxError |  构造函数 SyntaxError |
| String | 构造函数 String | TypeError | 构造函数  TypeError |
| Number | 构造函数 Number | URIError | 构造函数  URIError |

ECMAScript5 明确给undefined,NaN,Infinity赋值

4.window 对象
**ECMAScript没有指出如何直接访问Global对象，WEB浏览器将这个全局对象作为`window`对象的一部分实现,全局作用域中声明的变量和函数称为window对象的属性**
```
var color="red";   //全局变量
function sayColor(){    //全局函数
        alert(window.color);
}
window.sayColor();   //red
```
另外一种取得Global对象的方法:
```
var global=function(){
        return this;
}();
```
### Math对象
1.Math对象的属性

| 属性 | 说明 |
|---|---|
| Math.E | 自然对数的底数，常量e的值 |
| Math.LN10 | 10的自然对数 |
| Math.LN2 | 2的自然对数 |
| Math.LOG2E | 以2为底e的对数 |
| Math.LOG10E | 以10为底e的对数 |
| Math.PI | π的值 |
| Math.SQRT_2 | 1/2的平方根（即2的平方根的倒数）|
| Math.SQRT2 | 2的平方根 |

2.min()和max()方法
可以接收任意多个参数，返回最小和最大的值

3.舍入方法
```
//向上舍入，数值向上舍入最接近的整数
Math.ceil()

//向下舍入，数值向下舍入最接近的整数
Math.floor()

//标准舍入(四合五入)
Math.round()
```
```
alert(Math.ceil(25.9)); //26
alert(Math.ceil(25.5)); //26
alert(Math.ceil(25.1)); //26

alert(Math.round(25.9)); //26
alert(Math.round(25.5)); //26
alert(Math.round(25.1)); //25

alert(Math.floor(25.9)); //25
alert(Math.floor(25.5)); //25
alert(Math.floor(25.1)); //25
```

4.random() 方法
**`Math.random()` 返回大于等于0小于1的一个随机数**

可以利用random()从某个整数范围内随机选择一个值:
```
Math.floor(Math.random())*可能值的总数+第一个可能的值
```

**求某个范围内的一个随机数,返回任意两个整数之间可能的一个随机整数:**
```
function selectFrom(lowerValue,upperValue){
    var choices =upperValue -  lowerValue +1;   //可能返回值的数量总和
    return Math.floor(Math.random()*choices+lowerValue);
}
var num = selectFrom(2,10);
alert(num);
```
5. 其他方法

| 方法 | 说明 | 方法 | 说明 |
|---|---|---|---|
| Math.abs(num) | 返回num的绝对值 | Math.asin(x) | 返回x 的反正弦值 |
| Math.exp(num) | 返回Math.E的num次幂 | Math.atan(x) | 返回x的反正切值 |
| Math.log(num) | 返回num的自然对数 | Math.atan2(y,x) | 返回 y/x 的反正切值 | 
| Math.pow(num,power) | 返回num的power次幂 | Math.cos(x) | 返回x的余弦值 |
| Math.sqrt(num) | 返回num的平方根 | Math.sin(x) | 返回x的余弦值 | 
| Math.acos(x) | 返回x的反余弦值 | Math.tan(x) | 返回x的正切值 

## 小结
对象在JS中为引用类型，内置的引用类型可以创建特定的对象
1. Object是一个基础类型，其他所有类型都是从Object类型继承了基本的行为
2. Array类型是一组值的有序列表，同时提供了操作和转换这些值的功能
3. Date类型提供有关日期和时间的信息，金额当前日期和时间的相关计算功能
4. RegExp类型是ECMAScript支持正则表达式的一个接口。

函数实际上是Function类型的实例，所以函数也是对象，函数也有方法增强行为。

由于有基本包装类型，所以，基本类型值可以被当作对象来访问。
三种基本包装类型:
1. Boolean
2. Number
3. String
共同特征：
1. 每个保障类型都映射到同名的基本类型
2. 在读取模式下访问基本类型，会创建对应的包装类型的一个对象，方便数据操作
3. 操作基本类型的值一经完成，立即销毁新创建的包装对象

所有代码执行之前，作用域内就内置两个对象`Global`和`Math`。在大多数的ECMAScript的实现，都不能直接访问Global对象，WEB浏览器实现了承担GLOBAl角色的window对象。全局的函数和变量都是global对象的属性



[TOC]
