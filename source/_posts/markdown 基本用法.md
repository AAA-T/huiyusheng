---
title: markdown 基本使用
date: 2020-10-21 10:51:47
excerpt: markdown 的一些基本用法
index_img: https://gitee.com/AAA-T/blogimg/raw/master/img/markdown%E5%9F%BA%E6%9C%AC%E7%94%A8%E6%B3%95.jpg
tags: markdown
categories: 
  - markdown
---



# <center> markdown使用

## 1.1标题

用#代表一个#代表一级标题连个代表二级标题以此类推

## 1.2正文

正文处 换行需要在代码中实际空出一行

## 1.3代码块

代码块用过两行```符号框处 使用某种语言是 在第一行的末尾加上 对应的名字，在代码块内就会执行对应的高亮的语法例如 java

``` JAVA

public class HelloWorld{
    public static void main(String[] args){
        System.out.println("hello world!");
    }
}
```

## 1.5行内代码

行内代码用``框起来

打印hello world` System.out.println("hello world !")`

## 1.6 列表

### 1.6.1有序列表

输入数组，加一个句号，然后tab键；可以控制控制多级列表


1. 先这样
   1. 再这样


### 1.6.2无序列表

无序列表，输入 - ，然后空格，同样tab键，可以控制多级列表

- 先这样
  - 再这样 

## 1.7 字体

- **通过 ** 来控制加粗**

- *一个*号控制斜体*

- ***斜体加粗***

- ~~删除线~~
  
  
## 1.8 插入表格

表头|表头|表头
---|:---:|---:
内容|内容|内容
内容|内容|内容

第二行分割表头和内容。

：有一个就可以了，为了对其可以多加几个

文字默认居左

-两边加居中
-右边加居右


示例：

姓名|技能|排行
--|:--:|--:
刘备|哭|大哥
关羽|打|二哥
张飞|骂|三弟


## 超链接

`[超链接名](超链接地址 "超链接title")`

title 可以不加

[百度](baidu.com)

## 图片

`![图片alt](图片地址 '' 图片title'')`

alt 就是现实再图片下面的文字，相当于对图片内容的解释

图片title就是图片的标题，当鼠标移到图片上时现实的内容。title客家可不加



![4K壁纸](http://pic.netbian.com/uploads/allimg/200618/005100-1592412660f973.jpg "图片示例")



## 分割线

---
***
----
*****

## markdown 中可以书写html代码 并且可以生效

<center> 居中可以使用html<标签center> </center>

## 引用

>该文章由AAA-T学习

