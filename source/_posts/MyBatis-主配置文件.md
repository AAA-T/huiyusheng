---
title: MyBatis-主配置文件
date: 2020-12-24 17:31:46
tags:
    - 主配置文件
index_img: https://gitee.com/AAA-T/blogimg/raw/master/img/1608542594653.jpg
excerpt: MyBatsi配置文件两大类：1.MyBatis主配置文件；2.MyBatis的mapper文件
categories: 
    - MyBatis
---
# <center> MyBatis-主配置文件

MyBatsi配置文件两大类：1.MyBatis主配置文件；2.MyBatis的mapper文件
1. MyBatis 主配置文件：提供MyBatis全局设置的。包含的内容日志，数据源，mapper文件位置
2. mapper文件:写sql语句的。一个表一个mapper文件


## 一、settings部分

settings是MyBatisde 全局设置，影响整个MyBatis的运行。这个设置一般使用默认值

```xml
<settings>
  <setting name="cacheEnabled" value="true"/>
  <setting name="lazyLoadingEnabled" value="true"/>
  <setting name="multipleResultSetsEnabled" value="true"/>
  <setting name="useColumnLabel" value="true"/>
  <setting name="useGeneratedKeys" value="false"/>
  <setting name="autoMappingBehavior" value="PARTIAL"/>
  <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
  <setting name="defaultExecutorType" value="SIMPLE"/>
  <setting name="defaultStatementTimeout" value="25"/>
  <setting name="defaultFetchSize" value="100"/>
  <setting name="safeRowBoundsEnabled" value="false"/>
  <setting name="mapUnderscoreToCamelCase" value="false"/>
  <setting name="localCacheScope" value="SESSION"/>
  <setting name="jdbcTypeForNull" value="OTHER"/>
  <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
</settings>
```

## 二、别名
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
        name：包名，mybatis会把这个包中所有类名做为别名(不用区分大写)
        优点：使用方便，一次可以给多个类名自定义别名
        缺点：别名不能自定义必须是类名。
    -->
    <package name="cn.huiyusheng.domain"/>
</typeAliases>
```

## 三、配置环境

```xml
<environments default="development">
    <environment id="development">
        <transactionManager type="JDBC"/>
        <!--配置数据源：创建Connection对象-->
        <dataSource type="POOLED">
            <!--driver:驱动的内容-->
            <property name="driver"value="com.mysql.cj.jdbc.Driver"/>
            <!--url:连接数据库的url-->
            <property name="url"
                     value="jdbc:mysql://8.135.78.61:3306/mybatis?seUnicode=true&amp;characterEncoding=utf-8"/>
            <!--username:用户名-->
            <property name="username" value="root"/>
            <!--password:密码-->
            <property name="password" value="123456"/>
        </dataSource>
    </environment>
</environments>




environments:  环境标签，在他的俩面可以配置多个environment
    属性：defult，必须是某个environment的id属性值，表示MyBatis默认连接的数据库 
environment :  表示一个数据库的连接信息
    属性: 自定义的环境的标示。唯一值
transactionManager: 事务管理器
    属性：type标示事务管理器的类型。
    属性值：1. JDBC:使用Connection对象，由MyBatis自己完成事务的处理
            2. MANAGED:管理，标示八十五的处理交给容器实现(由其他软件完成事务的提交，回滚)
dataSource: 数据源，创建的COnnection对象，连接数据库。
    属性：  type数据源的类型
    属性值：1. POOLED,  MyBatis会在内存中创建PooledDataSource类，管理多个Connection连接对象，使用的连接池。
            2. UnPOOLED,  不实用连接池，MyBatis创建一个UnPooledDataSource这个类，每次执行sql语句先创建Connection
                          对象，再执行sql语句，追后关闭Connection
            3。JNDI,    JAVA中命名和目录服务
```

## 四、使用数据库属性配置文件
需要把数据库的配置信息放到一个单独文件中，独立管理。这个文件扩展名是properties这个文件中，使用自定义的key=value的格式表示数据

使用步骤：
1. 在resource目录中，创建xxx.properties
2. 在文件中，使用key=value的格式定义数据。
    例如 jdbc。url=jdbc:mysql://localhost:3306/springdb
3. 在MyBatis主配置文件，使用property标签引入外部的属性配置文件
4. 在使用值的位置，使用${key}获取key对应的value(等号右侧的值)

例子
db.properties
```properties
mysql.username = root
mysql.password = 123456
mysql.url =jdbc.mysql://localhost:3306/SpringDemo
mysql.driverClassName = com.mysql.cj.jdbc.Driver
```

在主配置文件中导入
```xml
<properties resource="db.properties"/>
```
## 五、mapper标签

使用mapper指定其他mapper文件的位置,

mapper标签使用的两种方式：
```xml
<!-- sql mapper(sql映射文件)的位置-->
<mappers>
    <!--第一种方式，resources="mapper文件的路径"
        优点：文件清晰。加载的文件是明确的。
             文件的位置比较灵活
        缺点: 文件比较多时，代码量会比较大，管理难度大
    -->
    <mapper resource="cn/huiyusheng/dao/StudentDao.xml"/>
    <!--第二种方式，使用<package>
        name: mapper文件所在的包名
        特点： 把这个包重的所有mapper文件一次加载
        
        使用要求：
            1. mapper文件和dao接口在同一个目录
            2. mapper文件和dao接口的名称相同
    -->
    <package name="cn/huiyusheng/dao"/>
</mappers>
```

