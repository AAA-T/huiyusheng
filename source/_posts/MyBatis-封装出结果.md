---
title: MyBatis-封装出结果
date: 2020-12-23 10:08:36
tags:
    - resultType
    - resyltMap
index_img: https://gitee.com/AAA-T/blogimg/raw/master/img/1608542594653.jpg
excerpt: 封装输出结果：MyBatis执行sql语句，得到ResultSet,转化为java对象
categories: 
    - MyBatis
---

# <center>MyBatis-封装出结果

> 封装输出结果：MyBatis执行sql语句，得到ResultSet,转化为java对象

## 一、resultType
resultType属性:在执行select时使用，作为`<select>`标签的属性出现的。

resultType:表示结构类型，mysql执行sql语句，得到java对象的类型。

同名列赋值给同名的属性，

他的值有两类
1. java类型的全限定名称。    
2. 使用别名

> resultType：指定的是一个自定义对象，会根据查询到的列名 和对象中的属性 如果 列名和对应属性名相同才会进行赋值

### 1) resultType表示的是java自定义对象 -----最常用
```java
    public Student selectStudentById(@Param("studentId") Integer id);
```
```xml
 <select id="selectStudentById" parameterType="int" resultType="cn.huiyusheng.domain.Student">
        select id,name, email,age from student where id=${studentId}
    </select>
    
resultType:现在使用java类型的全限定名称。表示的意思 mybatis执行sql，把ResultSet中的数据转化为Student
类型的对象。MyBatis会做一下操作：
1. 调用cn.huiyusheng.domain.Student的无参数构造方法，创建对象。
    Student student = new Student(); // 使用反射创建对象
2.同名的列赋值给同名的属性。
    student.setid(rs.getInt("id"));
    student.setName(rs.getString("name"));
3.得到java对象，如果dao接口返回值是List集合，MyBatis把student对象放入到list集合。


所以执行 Student mystudent = dao.selectById(1001); 得到 数据库中 id = 1001这行的数据，
这行数据的列值，付给了mystudent对象的属性。你能得到mystudent对象。就相当于市id=1001这行数据。
```

### 2) resultType:表示的是基本数据类型
dao 方法

```java
long countStudent();
```

mapper 文件
```xml
    <!--
        执行sql语句，得到的是一个值(一行一列)
    -->
    <select id="countStudent" resultType="java.lang.Long">
        select count(*) from student
    </select>
```

### 3) resultType:表示一个map结构

```java
    //查询结果是一个map
    Map<Object,Object> selectMap(@Param("stuid") Integer id);
```

```xml
    <!--
        执行sql得到一个map结构数据，MyBatis执行sql，把ResultSet转化为map
        sql执行结果: 列名作为map的key，列值作为value
        sql执行得到的结果是一行记录，转化为map结构是正确的.

        dao接口返回是一个map，sql语句最多能获取一行记录，多余一样是错误的
        org.apache.ibatis.exceptions.TooManyResultsException: Expected one result (or null) to be returned by selectOne(), but found: 4
        期望通过selectOne()获取一个或0个结果，但是返回的结果有四个
    -->
    <select id="selectMap" resultType="java.util.Map">
        select * from student where id != #{stuid}
    </select>
```

> 练习:
>   输入一个省份id，得到省份id，省份name，城市id，城市姓名
>
>   例如输入省份id=1
>   
>   1 河北 石家庄
>   >   sql:
>   >   ```sql 
>   >   SELECT
>   >       p.id,p.name,c.id,c.name 
>   >   FROM
>   >       province p 
>   >   INNER JOIN
>   >       city c 
>   >   ON 
>   >       p.id=c.provinceid 
>   >   WHERE
>   >       p.id=1
>   >   ```
>   >   java dao中
>   >```java
>   >public interface ProvinceDao {
>   >    List<ProvinceCity> selectProvinceCity(@Param("pid") Integer provinceId);
>   >}
>   > ```
>   > java 实体类
>   > ```java
>   >public class ProvinceCity {
>   >    private Integer id;     //省份的id
>   >    private String name;    //省份的名称
>   >    private Integer cid;    //城市的id
>   >    private String cname;   //城市的名称
>   >
>   >    public Integer getId() {
>   >        return id;
>   >    }
>   >
>   >    public void setId(Integer id) {
>   >        this.id = id;
>   >    }
>   >
>   >    public String getName() {
>   >        return name;
>   >    }
>   >
>   >    public void setName(String name) {
>   >        this.name = name;
>   >    }
>   >
>   >    public Integer getCid() {
>   >        return cid;
>   >    }
>   >
>   >    public void setCid(Integer cid) {
>   >        this.cid = cid;
>   >    }
>   >
>   >    public String getCname() {
>   >        return cname;
>   >    }
>   >
>   >    public void setCname(String cname) {
>   >        this.cname = cname;
>   >    }
>   >
>   >    @Override
>   >    public String toString() {
>   >        return "ProvinceCity{" +
>   >                "id=" + id +
>   >                ", name='" + name + '\'' +
>   >                ", cid=" + cid +
>   >                ", cname='" + cname + '\'' +
>   >                '}';
>   >    }
>   >}
>   >```
>   > mapper 文件
>   >```xml
>   >    <select id="selectProvinceCity" resultType="cn.huiyusheng.vo.ProvinceCity">
>   >        select p.id,p.name,c.id cid,c.name cname from province p inner join city c on p.id=c.provinceid where p.id = #{pid};
>   >    </select>
>   >```
>   > java测试类
>   >```java
>   >    @Test
>   >    public void testSelectPC(){
>   >        SqlSession session = MyBatisUtil.getSqlSession();
>   >        ProvinceDao dao = session.getMapper(ProvinceDao.class);
>   >        List<ProvinceCity> provinceDaos = dao.selectProvinceCity(1);
>   >        provinceDaos.forEach(pro-> System.out.println(pro));
>   >        session.close();
>   >    }
>   >```
## 二、自定义别名

MyBatis提供的对java类型定义间断，好记名称，

自定义别名的步骤：
1. 在MyBatis主配置文件，使用typeAliase标签声明别名
2. 在mapper文件中，resultType="别名"


声明别名(MyBatis主配置文件,在xml文件后面):

```xml
<!--声明别名-->
    <typeAliases>
        <!--第一种语法格式
            type:java类型的全限定名称(自定义类型)
            alias:自定义别名
            
            优点：别名可以自定义
            缺点：每个类型必须单独定义
        -->
        <typeAlias type="cn.huiyusheng.domain.Student" alias="stu" />
        
        <!--第二种方式
            name：包名，mybatis会把这个包中所有类名做为别名(不用区分大小写)
            优点：使用方便，一次可以给多个类名自定义别名
            缺点：别名不能自定义必须是类名。
        -->
        <package name="cn.huiyusheng.domain"/>
    </typeAliases>
```

在mapper文件中使用
```xml
resultType="别名"
第一种

    <select id="selectById" parameterType="java.lang.Integer"
            resultType="stu">
        select * from student where id=#{studentId}
    </select>

第二种
    <select id="selectById" parameterType="java.lang.Integer"
            resultType="student">
            <!--是对应的类名  不区分大小写-->
        select * from student where id=#{studentId}
    </select>
```

> 不使用别名 使用 对应的全限定名称 不会出现冲突 简单明了

## 三、resultMap
resultMap:结果映射。自定义列名和java对象属性的对应关系。常用在列名属性名**不相同**的情况下

用法：
1. 先定义resultMap标签，指定列名和属性名称对应关系
2. 在select标签使用resultMap属性，在指定上面定义的resultMap的id值

```xml
    <!--使用resultMap定义列和属性的关系-->
        <!--定义resultMap
            id:给resultMap的映射关系起个名称，唯一值
            type:java类型的全限定名称
        -->
    <resultMap id="CustomMap" type="cn.huiyusheng.vo.CustomObject">
        <!--定义列名和属性名的对应-->
        <!--主键类型使用id标签-->
        <id column="id" property="cid"></id>
        <!--非主键列使用result标签-->
        <result column="name" property="cname"></result>
        <!--列名和属性名一直不需要配置-->
    </resultMap>
    <!--使用resultMap属性，指定映射关系的id
        resultMap和resultType 不能同时使用，二选一
    -->
    <select id="selectByCid" resultMap="CustomMap">
        select * from student where id=#{stuid}
    </select>
```

## 四、列名和java对象属性名称不一样的解决方式

1. 使用resultMap:自定义列名和属性名称对应关系
2. 使用resultType:使用列别名，让别名和java对象的属性名相同

## 五、模糊查询---like
第一种方式：在java程序中，把like的内容组装好。把这个内容传入到sql中

```JAVA
List<Student> selectLikeOne(@Param("name") String name)
```

mapper
```xml
    <select id="selectLikeOne" resultType="cn.huiyusheng.domain.Student">
          select * from student where
          name like #{name}
    </select>
```

执行like

```JAVA
@Test
public void testSelectAll(){
    //1.获取SqlSession
    SqlSession session =MyBatisUtil.getSqlSession();
    StudentDao dao =session.getMapper(StudentDao.class);
    String name = "%李%";
    List<Student> student = dao.selectLikeOne(name);
    session.close();
    student.forEach(stu->System.out.println("学生===="+stu))
}
```

第二种方式：在sql语句，组织like的内容 （不方便 假如要只查询某个内容开头的需要去修改 sql ）

```JAVA
List<Student> selectLikeTwo(@Param("name") String name)
```

mapper
```xml
    <select id="selectLikeTwo" resultType="cn.huiyusheng.domain.Student">
          select * from student where
          name like "%" #{name} "%" 
    </select>
```

执行like

```JAVA
@Test
public void testSelectAll(){
    //1.获取SqlSession
    SqlSession session =MyBatisUtil.getSqlSession();
    StudentDao dao =session.getMapper(StudentDao.class);
    String name = "李"; //模糊查询
    List<Student> student = dao.selectLikeOne(name);
    session.close();
    student.forEach(stu->System.out.println("学生===="+stu))
}
```