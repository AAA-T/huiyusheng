---
title: JavaScript - 参数及对象
date: 2020-10-21 10:51:47
excerpt: 就是在function 函数内**不需要定义**，但却可以直接用来获取所有**出入**参数的变量，我们管它叫做隐形参数。
index_img: https://gitee.com/AAA-T/blogimg/raw/master/img/javascript%E5%8F%82%E6%95%B0%E5%8F%8A%E5%AF%B9%E8%B1%A1.jpg
tags: 
    - JavaScript 
    - argments
categories: JavaScript
---

# <center> JavaScript 参数及对象

## 隐形参数 ***argments*** （只在function() 函数内）

就是在function 函数内**不需要定义**，但却可以直接用来获取所有**出入**参数的变量，我们管它叫做隐形参数。

隐形参数特别像java基础的可变长参数一样

```java
public void fun(object ...args)
```
>注：变长参数在编译为字节码后，在方法签名中就是以数组形态出现的。这两个方法的签名是一致的，不能作为方法的重载。如果同时出现，是不能编译通过的。可变参数可以兼容数组，反之则不成立

在js中的隐形参数和java中的可变长参数一样，操作类似于一个数组


```JavaScript
function argmentsTest(){
    alert(argments);//结果：接受到的是一个数组对象 object argments
    alert（argments.length）；//结果：argments 是一个数组对象，可以像数组一样用.length获取到这个数组的长度
    for(var i=0;i<argments.length;i++){
        //argments 可以像数组，通过循环获取到数组中的所有数据
        alert(argments[i]);
    }
}
fun(1,"id",true);
```

## js的自定义对象

### Object形式的自定义对象
#### 定义

- `var 变量名 = new Object();`//对象实例 
- 变量名.属性名 = 值 // 定义一个属性
- 变量名.函数名 = function() //定义一个函数
  
#### 对象的访问
- 变量名.属性/函数名();
  
```JavaScript
var obj = new Object();
obj.name = "自定义对象obj";
obj.time = "2020/10/18"
obj.printf=function(){
    alert("对象名：" + this.name + ",创建时间：" + this.time);
}

alert(obj.name);
alert(obj.printf);
```
### 花括号形式的自定义对象

#### 定义

```JavaScript
var 变量名 = { // 定义一个空对象
属性名 ： 属性值 //定义对象中的属性
方法名 ： function(){//定义对象中的方法
}
}
```
#### 访问

- 变量名.属性/函数





