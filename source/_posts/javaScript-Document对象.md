---
title: JavaScript - Document对象
date: 2020-10-21 10:51:47
excerpt: Document将整个html页面中的标签，属性，文本，转化成对象来管理
index_img: https://gitee.com/AAA-T/blogimg/raw/master/img/javascript-doucument%E5%AF%B9%E8%B1%A1.jpg
tags: 
    - JavaScript 
    - Document
categories: JavaScript
---

# <center>JavaScript - Document对象

## 定义

Document将整个html页面中的标签，属性，文本，转化成对象来管理

![doucument和html页面中的关系](https://gitee.com/AAA-T/blogimg/raw/master/img/document%E4%B8%AD%E5%AF%B9%E8%B1%A1%E5%92%8Chtml%E9%A1%B5%E9%9D%A2%E7%9A%84%E5%85%B3%E7%B3%BB.png "document和html页面中的关系")

- document对象的理解
    1. document 对象管理了所有的html文档内容
    2. document 他是一种树结构的文档，有层级关系。
    3. 他可以让我们把所有的标签都对象化
    4. 我们可以通过虽有的document去访问所有的标签对象

## Document对象中的方法介绍

### doucment.getElementById(elementId);
通过标签的id属性查找标签dom对象，elementId是标签的id属性值
> 注：只能返回拥有制定id的第一个对象的引用
```
<body>
用户名：<input type="text" id = "username" value="doucment">
<button>校验</button>
</body>
```
当用户惦记了校验按钮，获取输入框重的内容，然后验证其是否合法

验证规则：必须由字母，数组，下划线组成，并且长度为5-12

```javascript
<script type ="text/javascript">
function onclickFun(){
//通过id 获取对应的标签对象
    var usernameObj = document.getEkementById("username");
    /[object HTMLInputElement] 他就是dom对象
    var usernameText = usernameObj.value;
    //用正则表达式设置规则
    var patt = /^\w{5,12}$/;
    //test()方法用于测试某个字符串，是不是匹配我的规则
    //符合是true 不符合是 false
    if(patt.test(usernameText)){
        alert("用户名合法！");
    } eles {
        alert("用户名不合法！");
    }
    
}    
</script>
```

### doucment.getElementsByName(elementName);

通过标签的name属性查找标签dom对象，elementId是标签的name属性值

### document.getElementsByTagName(tagname)
通过标签名查找标签dom对象。tagname是标签名

### document.createElement(tagName)
方法，通过给定的标签名，创建一个标签对象。tagName是要创建的标签名

