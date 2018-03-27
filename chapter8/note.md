[TOC]

#BOM
ECMAScripts是JavaScript的核心，在Web中使用JavaScript，则BOM是真正的核心

## window对象
表示浏览器的一个实例，在浏览器中window对象既是JavaScript访问浏览器窗口的一个接口，又是ECMAScript规定的Global对象 
```
var age=29;
function sayAge(){
        alert(this.age);
}
alert(window.age);  //29
sayAge();  //29
window.sayAge();  //29
```
全局作用域定义了变量age,因此可以通过window.age访问。sayAge()函数是在全局作用域中声明的，因此函数内的this映射为window。
**虽然全局变量会成为window对象的属性，但是定义的全局变量和在window对象上直接定义属性有差别：全局变量无法通过delete操作符删除，而直接在window对象上定义的属性可以**:
```
var age=29; //定义全局变量
window.color="red"; // window对象上的属性
delete window.age; //IE<9出错，其他浏览器返回false
delete window.color; //IE<9出错，其他浏览器返回true

alert(window.age); //29   无法删除
alert(window.color);//undefined 已经删除age属性
```
另外尝试访问未声明的变量会报错，但是通过window对象可以知道某个可能未声明的变量是否存在：
```
var newValue=oldValue; //报错

var newValue=window.oldValue; //undefined
```

### 窗口关系及框架
**如果页面包含框架，每个框架都有自己的window对象，保存在frames集合中。在frames集合中，可以通过数值索引（从0开始，从左到右，从上到下）或者框架名称来访问相应的window对象。**

```
<html>
    <head>
    <title>Frameset Example</title>
    </head>
<frameset row="160,**">
            <frame src="frame.htm" name="topFrame">
                <frameset cols="50%,50%">
                             <frame src="anotherFrame.htm" name="leftFrame">
                             <frame src="yetanotherFrame.htm" name="rightFrame">
                 </frameset>
</frameset>
</html>
```
```
//最外层框架
window.frames[0]
 window.frames["topFrame"]
 top.frames[0]
 top.frames["topFrame"]
 frames[0]
 frames["topFrame"] 
```
```
//内部左侧框架
window.frames[1]
 window.frames["leftFrame"]
 top.frames[1]
 top.frames["leftFrame"]
 frames[1]
 frames["leftFrame"] 
```
```
//内部右侧框架
window.frames[2]
 window.frames["rightFrame"]
 top.frames[2]
 top.frames["rightFrame"]
 frames[2]
 frames["rightFrame"] 
```

与top相对的另外一个window对象是`parent`,指向当前框架的直接上层框架，某些情况下parent可能等于top，没有框架情况下,parent一定等于top。

### 窗口位置
```
//分别获取窗口相对于屏幕左边和上边的距离
//IE ,Safari,Opera,Chrome
window.screenLeft
window.screenTop

//Firefox
window.screenX
window.screenY
```
以下方法可以跨浏览器获得窗口左边和上边的位置:
```
var leftPos=(typeof window.screenLeft =="number")?window.screenLeft:window.screenX;
var topPos=(typeof window.screenTop =="number")?window.screenTop:window.screenY;
```

**moveTo(x,y)和moveBy(x,y)函数都可以将窗口精确的移动到一个坐标位置。moveTo(x,y)是将窗口移动到坐标为(x,y)的位置，
moveBy(x,y)则是将窗口向下移动位置y,向右移动位置x.**

### 窗口大小
```
//IE9+ FireFox Safari  返回浏览器本身尺寸
window.outerWidth
window.outerHeight
```
跨浏览器获取页面视口大小：
```
var pageWidth=window.innerWidth;
var pageHeight=window.innerHeight;

if(typeof pegeWidth!="number"){
            if(document.compatMode=="CSS1Compat"){
                        pageWidth=document.documentElement.clientWidth;
                        pageHeight=document.documentElement.clientHeight;
            }else{
                        pageWidth=document.body.clientWidth;
                        pageHeight=document.body.clientHeight;
            }
}

```
调整窗口大小:
```
//新窗口宽度为width,高度为height
window.resizeTo(width,height);
//新窗口和原窗口尺寸的差值
window.resizeBy(width,height);
```
###导航和打开窗口
```
window.open(url,target,string,boolean);
//url:要加载的URL
// target:窗口目标。_self,_top,_parent,_blank
//string,一个特性字符串
//boolean,表示新页面是否取代历史记录中当前加载页面的布尔值

```
第三个参数的设置选项:

| 设置 | 值 | 说明 | 
|-|-|-|
| fullscreen | yes或no | 表示浏览器窗口是否最大化.仅限IE |
| height | 数值 | 表示新窗口的高度。不能小于100 |
| left | 数值 | 表示新窗口的坐坐标。不能是负值 |
| location | yes或no | 表示是否在浏览器窗口中显示地址栏。不同浏览器默认值不同。如果为No,可能地址栏隐藏或者被禁用 |
| menubar | yes或no | 表示是否在浏览器中显示菜单栏。默认no | 
| resizable | yes或no | 表示是否可以通过拖动浏览器窗口的边框改变大小。默认no |
| scrollbars | yes或no | 表示如果内容在视口中显示不下，是否允许滚动。默认no |
| status | yes或no | 表示是否在浏览器窗口中显示状态栏。默认no |
| toolbar | yes或no | 表示是否在浏览器窗口中显示工具栏。默认no |
| top | 数值 | 表示新窗口的上坐标。不能是负值 |
| width | 数值 | 表示新窗口的宽度。不能小于100 |

关闭窗口可以用通过`close()`方法，不过只对通过`window.open()`打开的窗口有效，新建的window对象有一个opener属性，保存打开它的原始窗口对象。
在chrome中将opener设置为null,则表示标签页在单独的进程中运行。

### 间歇调用和超时调用
```
//超时调用
setTimeout(function(){
            //code here

    },time);

//间隙调用
setIntervarl(function()}{
                //code here
    },time);
```
超时和间歇调用都会返回一个数值ID，这个ID是计划执行代码唯一标识符，可以通过这个ID来取消超时和间歇调用:
```
//超时调用
var timeoutId= setTimeout(function(){
            //code here

    },time);

clearTimeout(timeoutId);//取消超时调用

//间隙调用
setIntervarl(function()}{
                //code here
    },time);
```
关于超时和间歇调用：由于JS的单线程语言，所以存在以下情况。当程序执行队列没有执行完之前，超时执行可能在超时过后不会立即执行。同样的
间歇调用可能遇到的问题是，前一个周期内代码没有执行完成，后面一个周期的代码又开始要执行了。
所以，最好不要使用间歇调用。

###系统对话框
```
//只有一个"确定"按钮
alert();
//两个按钮，确认和取消，分别对应返回true和false
confirm();
//两个按钮，确认和取消，以及一个输入框，确认按钮返回输入框内的值
prompt()
```

## location 对象
**location对象提供了与当前窗口中加载的文档有关的信息，和一些导航功能,location对象既是window对象的属性又是document对象的属性**
location对象将URL解析为独立的片段:

| 属性名 | 例子 | 说明 |
|-|-|-|
| hash |  "#contents" | 返回URL中的hash（#后面的字符），如果URL不含散列，则返回空字符串 |
| host | "www.wrox.com:80" | 返回服务器名称和端口号（如果有） |
| hostname | "www.wrox.com" | 返回不带端口号的服务器名称 |
| href | "http://www.wrox.com" | 返回当前加载页面的完整URL。`location.toStirng()`也返回这个值 |
| pathname | "/WileyCDA/" | 返回URL中的目录和（或者）文件名 |
| port | "8080" | 返回URL的端口号，如果不含，则返回空字符串 |
| protocol | “http:” | 返回页面使用的协议。通常是http:或者https: |
| search | "?q=javascript" | 返回URL中的查询字符串。字符串以`?`开头 |


### 查询字符串参数
```
//解析URL中的查询字符串
function getQueryStringArgs(){
        //取得查询字符串并去掉开头的问号
        var qs=(location.search.length>0?location.search.substring(1):""),
        //保存数据的对象
                args={},
        //取得每一项
                 items=qs.length?qs.split("&"):[],     //返回name=value形式的数组
                 item=null,
                 name=null,
                 value=null,
         //for 循环中使用        
                 i=0,        
                 len=items.length;
    for(i=0;i<len;i++){
                item=items[i].split("=");      //拆分name=value形式的数组元素，放入数组item中
                name=decodeURIComponent(item[0]);
                value=decodeURIComponent(item[1]);
                if(name.length){
                        args[name] = value;
                }
    }

        return args;
}

```

###位置操作
改变浏览器位置：
```
location.assign("http://www.wrox.com");
```
如果通过`replace()`方法则，无法使用浏览器的前进后退按钮

另外，重新加载页面可以使用`reload()`方法，如果要强制从服务器加载，需要传入true参数:
```
location.reload();   //重新加载页面（可能从缓存中加载）
location.reload(true); //重新加载（从服务器重新加载）
```


## navigator 对象

| 属性或方法 | 说明 | IE | Firefox | Safari/Chrome | Opera |
|-|-|-|-|-|-|
| appCodeName | 浏览器名称，通常是Mozilla | 3.0+ | 1.0+ | 1.0+ | 7.0 + |
| appMinorVersion | 次版本信息 | 4.0+ | - | - | 9.5+ |
| appName | 完成的浏览器名称 | 3.0+ | 1.0+ | 1.0+ | 7.0+ |
| appVersion | 浏览器版本 | 一般不与实际版本对应 | 3.0+ | 1.0+ | 1.0+ | 7.0+ |
| buildID | 浏览器编译版本 | - | 2.0+ | - | - |
| cookieEnabled | 是否启用cookie | 4.0+ | 1.0+ | 1.0+ | 7.0+ |
| cpuClass | 客户端计算机使用的CPU类型 | 4.0+ | - | - | - |
| javaEnabled() | 表示浏览器使用使用java | 4.0+ | 1.0+ | 1.0+ | 7.0+ |
| language | 浏览器的主语言 | - | 1.0+ | 1.0+ | 7.0+ |
| mimeType | 在浏览器中注册的MIME类型数组 | 4.0+ | 1.0+ | 1.0+ | 7.0+ |
| onLine | 表示浏览器是否联网 | 4.0+ |  1.0+ | - | 9.5+ |
| oscpu | 客户端计算机使用的CPU | - | 1.0 | - | - |
| platform | 浏览器所在系统平台 |  4.0+ | 1.0+ | 1.0+ | 7.0+ |
| plugins | 浏览器安装插件信息数组 | 4.0+ | 1.0+ | 1.0+ | 7.0+ |
| preference() | 设置用户的首选项 | - | 1.5+ | - | - |
| product | 产品名称（如Gecko）| - | 1.0+ | 1.0+ | - |
| productSub | 产品的次要信息 (如Gecko版本)| - | 1.0+ | 1.0+ | - |
| registerContentHandler() | 针对特定的MIME类型将一个站点注册微处理程序 | - | 2.0+ | - | - |
| registerProtocolHandler() | 针对特定的协议将一个站点注册微处理程序 | - | 2.0+ | - | - |
| systemLanguage() | 操作系统语言 | 4.0+ | - | - | - |
| userAgent | 浏览器的用户代理字符串 | 3.0+ | 1.0+ | 1.0+ | 7.0+ |
| userLanguage | 操作系统默认语言 | 3.0+ | - | - | 7.0+ |
| userProfile | 借以访问用户个人信息的对象 | 4.0+ | - | - | - |
| vendor | 浏览器品牌 | - | 1.0+ | 1.0+ | - |
| vendorSub | 有关供应商的次要信息 | - | 1.0+ | 1.0+ | - |


### 检测插件
对于非IE浏览器可以通过plugins数组完成：
plugins数组包含以下属性:

```
name:插件名称
description:插件的描述
filename:插件的文件名
length:插件处理的MIME类型
```

```
// 检测插件(IE 无效)
function hasPlugin(name){
            name=name.toLowerCase();
            for(var i=0;i<navigator.plugins.length;i++){
                            if(navigator.plugins[i].name.toLowerCase().indexOf(name)>-1){
                                        return true;
                            }
            }
         return false;   
}

//检测Flash
alert(hasPlugin("Flash"));
//检测Quicktime
alert(hasPlugin("QuickTime"));
```
###  注册处理程序
(略)

## screen 对象
screen对象主要用来表明客户端能力，下表列出所有属性和支持的浏览器

| 属性 |  说明 |   IE |  Firefox | Safari/Chrome | opera | 
|-|-|-|-|-|-|
| availHeight | 屏幕的像素高度减系统部件高度之后的值（只读） |  |  |  |   |
| availLeft | 未被系统部件占用的最左侧的像素值（只读） |  |  |  |   |
| availTop | 未被系统部件占用的最上方的像素值（只读） |  |  |  |   |
| availWidth |  屏幕的像素宽度减系统部件宽度之后的值（只读） |  |  |  |   |
| bufferDepth |  读写用于呈现屏外位图的位数 |  |  |  |   |
| colorDepth |  用于表现颜色的位数；多数系统都是32（只读） |  |  |  |   |
| deviceXDPI | 屏幕实际的水平DPI(只读)  |  |  |  |   |





##　history 对象
