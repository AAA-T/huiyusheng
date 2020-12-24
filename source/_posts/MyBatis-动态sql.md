---
title: MyBatis-动态sql
date: 2020-12-24 15:27:50
tags:
    - 动态sql
index_img: https://gitee.com/AAA-T/blogimg/raw/master/img/WeChatcc19638419c528ba797e6dbb397ac21f.png
excerpt: 动态sql:同一个dao的方法，根据不同的条件可以表示不同的sql语句，主要是where部分有变化
categories: 
    - MyBatis
---
# <center>MyBatis-动态sql</center>
h
**动态sql**:同一个dao的方法，根据不同的条件可以表示不同的sql语句，主要是**where**部分有变化

使用MyBatis提供的标签，实现动态sql的能力(if，where，foreach，sql)

使用动态sql的时候，dao方法的形参使用java对象

    使用场景 在多条件查询的时候 例如 根据姓名，年龄等去查询时，其中一个条件可能为空时 使用

    在使用> <  = 时最好使用实体
        
## 动态sql-if标签
语法格式：
```xml
<if test="biilan判断结果">
    sql代码
</if>


<!--在mapper文件中-->
<select id="selectStudent" resultType="cn.huiyusheng.domain.Student">
    select * form student
    <if test="条件">
        sql语句
    </if>
</select>
```

例子
```java
List<Student> selectIf(Student student);
```
mapper中
```xml
<!--If
        test:使用对象的属性值作为条件
        OGNL语法 对象图表达式

        在单独使用if的时候可能存在 第一个if不满足 第二个满足的情况 如： select * from student where or age = ?
        这种情况下 sql 语句的语法错误 我们 可以在 where 后面加一个不影响的 条件 如 1=1或者 id=-1 根据业务逻辑去添加

    -->
    <select id="selectIf" resultType="cn.huiyusheng.domain.Student">
        select * from student
        where id-1
        <if test="name!=null and name !=''">
            or name = #{name}
        </if>

        <if test="age > 0">
            or age = #{age}
        </if>
    </select>
```

## 动态sql-where标签

使用if标签时，容易引起sql语句语法错误。使用where标签解决if产生的语法问题。

使用where，里面是一个或多个if标签，当有一个if标签，判断条件为true，where标签会转为WHERE关键字附加到sql语句的后面，如果if没有一个条件为true，忽略where和后面的if

where标签会删除和他最近的or或者and
```xml
<where>
    <if test ="条件1">sql语句1</if>
    <if test ="条件2">sql语句2</if>
</where>
```
```java
//where
List<Student> selectWhere(Student student);
```

mapper 中
```xml
<!--where-->
<select id="selectWhere" resultType="cn.huiyusheng.domain.Student">
    select * from student
    <where>
        <if test="name !=null and name != ''">
            or name = #{name}
        </if>
        <if test="age > 0">
            or age > #{age}
        </if>
    </where>
</select>
```

## 动态sql-foreach循环标签
使用foreach可以循环数组，list集合，一般使用在in语句中。
语法：
```xml
<foreach collection"集合类型" open="开始的字符" close ="结束的字符"
item="集合中的成员" separator ="集合成员之间的分割符号">

#{item 的值}

</foreach>

标签属性:
collection: 表示循环的对象是 数组，还是list集合。如果dao接口方法的形参是数组
            colloction="array",如果dao接口形参是list，collection="list"
            
open:循环开始时的字符。sql.append("(");
close:循环结束时的字符。sql.append(")");
item:集合成员，自定义的变量。 Integer item = idList.get(i); //item是集合成员
separator:集合成员之间的分割符。 sql.append(",") //集合成员之间的分割符
#{item 的值}：获取集合成员的值
            
```
### 遍历List<简单类型>
dao 接口
```java
//foreach-1
List<Student> selectForeachOne(List<Integer> idlist);
```
mapper文件
```xml
<!--foreach第一种方式，循环简单类型的list-->
<select id="selectForeachOne" resultType="cn.huiyusheng.domain.Student">
    select * from student
    <if test="list != null and list.size>0">
        where id in
            <foreach collection="list" open="(" close=")" separator="," item="myId">
                #{myId}
        </foreach>
    </if>
</select>
```
测试
```java
@Test
public void testSelectForeachOne(){
    SqlSession session = MyBatisUtils.getSqlSession();
    StudentDao dao = session.getMapper(StudentDao.class);

    List<Integer> idlist = new ArrayList<>();
    idlist.add(1001);
    idlist.add(1002);
    idlist.add(1003);
    List<Student> students = dao.selectForeachOne(idlist);
    session.close();
    students.forEach(stu-> System.out.println(stu));
}
```
### 遍历List<对象类型>

dao接口
```java
//foreach-2
List<Student> selectForeachTwo(List<Student> studentList);
```

mapper 文件
```xml
<!--foreach第二种方式，循环对象类型的list-->
<select id="selectForeachTwo" resultType="cn.huiyusheng.domain.Student">
    select * from student
    <if test="list != null and list.size>0">
        where id in
            <foreach collection="list" open="(" close=")" separator="," item="stu">
                #{stu.id}
        </foreach>
    </if>
</select>
```

测试
```java
@Test
public void testSelectForeachTwo(){
    SqlSession session = MyBatisUtils.getSqlSession();
    StudentDao dao = session.getMapper(StudentDao.class);
    List<Student> students = new ArrayList<>();
    Student s1 = new Student();
    s1.setId(1001);
    Student s2 = new Student();
    s2.setId(1009);
    students.add(s1);
    students.add(s2);

    List<Student> students1 = dao.selectForeachTwo(students);
    session.close();
    students1.forEach(stu-> System.out.println(stu));
}
```

## 动态sql-sql标签

sql 标签表示一段sql代码，可以是表名，几个字段，where条件都可以，可以在其他地方复用sql标签的内容。

使用方法
1. 在mapper文件中定义sql代码片段<sql id="唯一字符串"> 部分sql语句 </sql>
2. 在其他位置，使用include标签引用某个代码片段

例如：

```xml
<sql id="selectAll">
    select * from student
</sql>

<sql id="studentFieldList">
    id,name,email
</sql>

<select id="selectIf" resultType="cn.huiyusheng.domain.Student">
   <include refid="selectAll" />
    where id-1
    <if test="name!=null and name !=''">
        or name = #{name}
    </if>
    <if test="age > 0">
        or age = #{age}
    </if>
</select>

<!--where-->
<select id="selectWhere" resultType="cn.huiyusheng.domain.Student">
    select <include refid="studentFieldList"></include> from student
    <where>
        <if test="name !=null and name != ''">
            or name = #{name}
        </if>
        <if test="age > 0">
            or age > #{age}
        </if>
    </where>
</select>

```

## 动态sql-不常用的标签

### choose、when、otherwise

语法格式：

```xml
<choose>
    <when test="条件1">
        sql1
    </when>
    <when test="条件2">
        sql2
    </when>
    <otherwise>
        sql3 条件1，2都不满足时执行条件3
    </otherwise>
</choose>
```

choose 类似于java中的switch语句 when 等价于 case otherwise等价于 defult

### trim标签

如果where标签 和期望值 不相同时，可以通过trim标签来定制标签元素的功能
格式：
```xml
<trim prefix="标签名" prefixOverrides="被忽略的文本 需要空格 如"and |or"">
  ...
</trim>
会移除所有prefixOrefixOverrides属性中中鼎的内容 并且插入prefix属性中的内容 
```
例如：
```xml
<trim prefix="WHERE" prefixOverrides="AND |OR ">
  ...
</trim>

和where 标签有相同的功能 当 where后面的 第一个为 and 或者or 时会将他们删除掉 然后 将where 放在该位置
```
### set 标签

```xml
<set>
    <if test="username != null">username=#{username},</if>
    <if test="password != null">password=#{password},</if>
    <if test="email != null">email=#{email},</if>
    <if test="bio != null">bio=#{bio}</if>
</set>
set 会动态的在 行首插入set关键字，并且删除掉额外的，号
```

