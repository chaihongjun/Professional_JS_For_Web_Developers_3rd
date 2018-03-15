[TOC]
# 在HTML中使用javascript
```
<script></script>
```
在HTML4.0.1版本包含有6个属性：
1. async 可选。表示立即下载脚本，但是不影响页面其他操作，比如下载其他资源等待加载其他脚本。这个只对外部脚本有效
2. charset 可选。对`src`引入的脚本明确字符集，大多数浏览器会忽略它，因此很少人使用。
3. defer 可选。表示脚本延迟到文档全部被解析完成和显示之后再执行，只对外部脚本有效。
4. language 已经被废弃
5. src 可选。引入的外部的脚本文件
6. type 可选。默认值是text/javascript。一般推荐不指定

所有的`<script>`必须在`<head>`里面


## defer 延迟脚本执行
**`defer`表明脚本立即下载完成，但在页面解析完成之后再运行此脚本。也就是立即下载脚本，等页面解析完成，再执行脚本**

## async 异步脚本
**`async` 表明脚本也是立即下载外部脚本，但是不保证按照脚本在`<head>`里面引入的顺序来执行的**
比如：
```
<!DOCTYPE html>
<html lang="cmn-hans">
<head>
    <meta charset="utf-8">
    <meta http-equiv=X-UA-Compatible content="IE=edge,chrome=1">
    <meta name="renderer" content="webkit">
   <!--引入JS-->
   <script src=js01.js"  charset="utf-8" async></script>
   <script src=js02.js"  charset="utf-8" async></script>
<title>javascript async</title>
</head>
<body>
   <!-- 这里是内容 -->
</body>
</html>
```
虽然，`js01.js`在`js02.js`文件的前面引入，他们都会立即下载，但是执行顺序有可能是第二个在第一个的前面。如果这两文件之见存在依赖关系，则不适合两个文件都使用`async`,使用异步的目标的是，不让页面等待这两个文件的下载和执行，从而异步加载页面的其他内容。
**异步脚本一定会在页面`load`事件前执行，但可能会在`DOMContentLoaded`事件之前或之后执行。**

### 关于defer 和async
**关于`async`和`defer`异步加载**
1. https://segmentfault.com/q/1010000000640869
2. https://segmentfault.com/a/1190000006778717

##小结
1. 所有`<script>`默认按照页面中出现的先后顺序去解析，只有前面的脚本解析完成了才会解析后面的脚本
2. 浏览器会先解析不含`defer`的`<script>`，所以，一般把`<script>`放在主要内容后面，`</body>`的前面。
3. `defer`会在文档完全呈现之后再执行，而且按照的是指定的先后顺序。
4. `async`不会阻塞文档呈现，也不会等待其他脚本
[TOC]
