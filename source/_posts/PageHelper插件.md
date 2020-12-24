---
title: PageHelper插件
date: 2020-12-24 17:32:01
tags:
    - PageHelper
index_img: https://gitee.com/AAA-T/blogimg/raw/master/img/1608542594653.jpg
excerpt: PageHelper做数据分页。在你的select语句后面假如分页的sql内容，如果你使用的mysql数据库他就是在 select * form student 后面加入limit语句。
categories: 
    - MyBatis
---
# <center>PageHelper插件
PageHelper做数据分页。在你的select语句后面假如分页的sql内容，如果你使用的mysql数据库他就是在 select * form student 后面加入limit语句。

使用步骤：

1. 使用maven导入 jar包
```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>5.1.10</version>
</dependency>
```

2. 在mybatis主配置文件中加入plugin声明
```xml
在<environments>之前加入
<plugins>
    <plugin interceptor ="com.github.pagehelper.PageInterCeptor" />
</plugins>
```

3. 在select语句之前，调用pageHelper.statrpage(页码，每页大小)


对比：

没有使用PageHepler
```sql
select * from student order by id
```

使用了 PageHepler
```sql
select * from student order by id

select * from student order by id limit ?,?
```
