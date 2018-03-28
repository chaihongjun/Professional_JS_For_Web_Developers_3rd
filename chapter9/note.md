[TOC]

# 客户端检测

##能力检测（特性检测）
基本模式：
```
if(object.propertyInQuestion){
            //使用object.propertyInQuestion
}
```

能力检测：
1. 先检测达成目的的常用特性
```
// IE 5 之前不支持document.getElementId,但是目前常用的是document.getElementById
//所以先检测document.getElementId
function getElement(id){
            if(document.getElementById){
                    return document.getElementById(id);
        } else if(document.all){
                return document.all(id);
        }
}

```

2. 必须测试实际要用到的特性
```
function getWindowWidth(){
            if (document.all) {  //假设是IE
                        return document.documentElement.clientWidth; //错误！
            }else{
                            return window.innerWidth;
            }
}
```
以上是错误的例子,IE和Opera都支持`document.all`。而且opera也支持`window.innerWidth`

### 更可靠的能力检测










[TOC]
