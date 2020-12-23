---
title: javaScript - 事件
date: 2020-10-21 11:51:47
excerpt: 电脑输入设备与页面进行交互的响应。我们称之为事件.
index_img: https://gitee.com/AAA-T/blogimg/raw/master/img/javascript%E4%BA%8B%E4%BB%B6.jpg
tags: 
    - JavaScript
    - onload
    - onclick
    - onchange
    - onblur
    - onsubmit
categories: JavaScript
---

# <center> javaScript - 事件</center>

## 定义
电脑输入设备与页面进行交互的响应。我们称之为事件

- 常用的事件
  - onload加载完成事件
    - 页面加载完成后，常用于做页面js代码初始化操作。
  - onclick单击事件
    - 常用语按钮的点击响应操作
  - onblur失去焦点事件
    - 常用于输入框失去焦点后验证其输入内容是否合法
  - onchange内容发生改变事件
    - 常用语下拉框和输入框内容发生改变后操作
  - onsubmit表单提交事件
    - 常用于表单提交前，验证所有表单项是否合法

## 事件的注册

>**事件的注册（绑定）**：告诉浏览器，当事件响应后要执行那些操作代码，叫事件注册或事件绑定

>**静态注册事件** : 通过html标签的事件属性直接赋予事件响应后的代码，这种方式我们叫静态注册

>**动态注册事件** ： 通过js代码得到标签的dom对象，然后再通过dom对象.事件名 = function(){}这种形式赋予事件响应后的代码叫做动态事件 

- 动态注册的基本步骤
    1. 获取标签对象
    2. 标签对象.事件名 = function(){}


## onload加载完成事件
- 页面加载完成后，常用于做页面js代码初始化操作。
  - 静态注册
    - 可以直接写在标签中`<body onload = "alert("静态注册onload事件")"></body>`
  
    也可以在标签中`<body onload = "onloadTest()">`
    在
    ```javascript
    <script type="text/javascript">
    function onloadTest(){
        alert("静态注册onload")
    }
    </script>
    ```
    将很多的代码写在方法中当页面加载完成后，会自动执行绑定的onload事件
   - 动态注册
     - 固定写法
    ```javascript
    window.onload = function(){
        alert("动态注册的onload事件")
    }
    ```
## onclick单击事件
- 常用语按钮的点击响应操作
  - 静态注册
    - 也可以在标签中`<button onclick = "onclickTest()">按钮</button>`
    在
    ```javascript
    <script type="text/javascript">
    function onclickTest(){
        alert("静态注册onclick")
    }
    </script>
    ```
    将很多的代码写在方法中当页面加载完成后，会自动执行绑定的onload事件
   - 动态注册
     - 固定写法
      ```javascript
      <body>
      <button id ="btn01">按钮</button>
      </body>
      window.onclick = function( ){
          //1.获取标签对象
          var btnObj = document.getElementById("btn01");
          btnObj.onclick = function(){
            alert("动态注册的onload事件");
          }
      }
      ```
## onblur失去焦点事件
- 常用于输入框失去焦点后验证其输入内容是否合法
```html
<body>
    用户名：<input type = "text" onblur ="onblurTest()"></br>
    密码：<input type = "text" id = "inp"></br>
     
</body>
```
```javascript
<script type ="text/javascript">
function onblurTest(){
    console.log("静态注册失去焦点事件");
}

window.onload = function (){
    var inpObj = document.getElementById("inp");
    inpObj.onblur = function(){
        console.log("动态注册失去焦点事件");
    }
}
</script>
```
## onchange内容发生改变事件
- 常用语下拉框和输入框内容发生改变后操作
```html
<body>
    请选择你想去的城市：
    <select onchange = "onchangeTest()" id = "sele">
    <option>城市</option>
    <option>上海</option>
    <option>杭州</option>
    <option>北京</option>
    </select>
</body>
```
```javascript
<script type ="text/javascript">
function onbchangeTest(){
    alert("onchange事件 下拉框中内容改变");
}

window.onload = function (){
    var selObj = document.getElementById("sele");
    selObj.onbchange = function(){
        alert("下拉框中的内容改变")
    }
}
</script>
```
## onsubmit表单提交事件
- 常用于表单提交前，验证所有表单项是否合法
```html
<body>
    <form action="http://location:8080" methon="post" onsubmit="return submitTest()">
        <input type = "submit" value="静态表单提交">
    </form>

    <form action="http://location:8080" methon="post" id = "form01">
        <input type = "submit" value="静态表单提交">
    </form>
</body>
```
```javascript
<script type ="text/javascript">
function onsubmitTest(){
    alert("静态注册表单提交事件----发现不合法");
    return false；
}

window.onload = function (){
    var formObj = document.getElementById("form01");
    formObj.onsubmit = function(){
        alert("动态注册表单提交事件----发现不合法)
        return false；
    }
}
</script>
```