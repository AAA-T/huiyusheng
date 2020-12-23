---
title: Java  - 常用类
date: 2020-10-22 20:16:47
update: 2020-11-05 17:45:50
excerpt: Java - String
index_img: https://gitee.com/AAA-T/blogimg/raw/master/img/java-%E5%B8%B8%E7%94%A8%E7%B1%BB.jpg
tags: 
    - Java
    - api
categories: java
---

# Java - 常用类

## 字符串相关的类

### String 
#### 类的特性
- String:字符串，使用一堆""印起来表示。
    1. String生命为final的，不可被继承
    2. String实现了Serializable接口：表示字符串是支持序列化的
    3. String实现了comparable接口：表示String可以比较大小（实现了 comparable 就可以比较大小）
    4. String内部定义了final char[] value 用于储存字符串数据
    5. String：代表不可变的字符序列。简称：不可变性
        1. 当对字符串重新赋值时，需要充血制定内存区域赋值，不能使用原有的value进行赋值
        2. 当对现有的字符串进行链接操作时，也需要重新指定内存区域赋值，不能直接去在原有的value上赋值
        3. 当调用String的replace方法修改指定的字符或字符串时，也需要重新制定内存区域，不能在原有的value 上赋值
    6. 通过字面量的方式(区别于new)给一个字符串赋值，此时的字符串值生命在字符串常量池中
    7. 字符串常量池是不会存储相同的内容的字符串的。

- String的是实例化方式：
    - 方式一：通过字面量定义的方式
    - 方式二：new + 构造器的方式
    ```java
    //通过字面量定义的方式：此时的S1和S2的数据生命在方法区的字符串常量池中
    String s1 = "javaee";
    String s2 = "javaee";
    //通过new + 构造器的方式：此时的s3和s4保存的地址值，是数据在堆空间中开辟空间以后对应的地址值
    String s3 = new String("JavaEE");
    String s4 = new String("JavaEE");
    ```
> 注：``String str1 = "abc" String Str2 = new String("abc");`` 的区别？
> 
>答：str1 是以字面量的方式定义的，他的存储的地址值是常量池中abc的地址值，而str2 是以new + 构造器的方式创建的，str2存储的地址值是堆空间中的value属性地址值（value属性是一个char [] 数组），而value属性存储的是常量池中的abc的地址值

> ***特别注意***：
> ```java
> class Person{
> String name; 
> String age;
> public Person(){}
> 
> public Person(String name,String age){
> this.name =name;
> this.age = age
>
> }
> }
> public class Test(){
>    public static void main(String[] age){
>        Person p1 = new Person("tom","12");
>        Person p2 = new Person("tom","12");
>        System.out.println(p1 == p2);//false
>        System.out.println(p1.name.equals(p2.name))//true
>        System.out.println(p1.name == p2.name)//true
>    }
>}
>
> ```
> Person对象重的name 属性是以字面量的形式定义的
> 并不是new 的方式，所以当对象被实例化的时候，构造器给属性赋值并没有将属性的值在堆空间中创建而是在常量池中创建（属性是String类型），所以当两个实例的属性的初始化值一样时，p1.name == p2.name也为true

>> String  s  = new String("abc");方式创建对象，在内存中创建了几个对象
>>
>> 在堆中创建了一个对象，在常量池中创建了一个对象

**结论：**

**1.常量与常量的拼接结果在常量池。且常量池中不会存在相同的内容的常量**

**2.只要其中偶一个是变量，结果就在堆中**

**3.如果拼接的结果是intern()方法，返回值就在常量池中**

**4.一个变量用final 修饰后 这个变量就会变成一个常量**

**一道面试题**
```java
public class StringTest{
    String str = new String("good");
    char[] ch = {'t','e','s','t'};
    
    public void change(string str,char []){
        str = "test ok";
        ch[0] = 'b';
    }
    public static void main(String[] args){
        StringTest ex = new StringTest();
        ex.change(ex.str,ex.ch);
        System.out.println(ex.str);
        System.out.println(ex.ch);
    }
}
```
#### **String中的方法**

int length(): 返回字符串长度 return value.length

char charAt(int index):返回某索引处的字符return value[index]

boolean isEmpty():判断字符串是否为空 return value.length

String toLowerCase():使用默认语言环境，将String中的所有字符转换为小写

String toUpperCase():使用默认语言环境，将String中的所有字符转换为大写

String trim():返回字符串副本，忽略前导空白和尾部空白
```java
//trim 示例
String str = "    hello   world   ";
String str1 = str.trim();
System.out.println("---" + str + "---")//结果：---    hello   world   ---
System.out.println("---" + str1 + "---")//结果---hello   world---
```
boolean equals(Object obj):比较字符串的内容是否相同

boolean equalsIgnroeCase(String anorherString):与equals方法类似，忽略大小写

String concat(String str):将指定字符串链接到此字符串的结尾。等价于+（字符串拼接）

int compareTo(String another):比较两个字符串的大小
```java
String str = "abc";
String str1 = new String("abd");
System.out.println(str.compareTo(str1));//结果：str<str1 返回-1 等于返回0 大于返回1
```
String substring(int beginIndex):返回一个新的字符串，从第beginIndex 开始到最后的一个新字符串

String substring(int beginIndex,int end endIndex):返回一个新的字符串，从beginIndex到endIndex的一个新的字符串

boolean endsWith(String suffix):测试此字符串是否以指定后缀结束

boolean startsWith(String prefix):测试此字符串是否以指定的前缀开始

boolean startsWith(String prefix,int tiffset):此时此字符串从指定索引开始的字符串是否以指定的前缀开始

boolean contains(CharSequence s):当且仅当此字符串包含指定的char值序列时，返回true

int indexOf(String str):返回指定字符串在此字符串中第一次出现处的索引

int indexOf(String str,int fromIndex):从fromIndex开始，返回指定字符串在此字符串中第一次出现处的索引，

注：index of lastIndexOf方法如果未找到都是返回-1


**替换**

String replace(char oldChar,Char newChar):将字符串中久的字符修改为新的字符

String replace(CharSequence target,charSequence replacement):将字符串中指定的 target替换为 replacement

String replaceAll(String regex,String replacement):使用给定的replacement替换所有的符合正则表达式的子字符串

String replaceFirst(String regex,String replacement):使用给定的replacement替换第一个的符合正则表达式的子字符串

**匹配**

boolean matches(String regex):告知此字符串是否匹配给定的正则表达式
```java
String tel = "0571-4534289";
//判断这是否是一个杭州的固定电话
boolean result = tel.matches("0571-\\d{7,8}");
System.out.println(result);//结果true
```

**切片**

String[] split(String regex):根据给定正则表达式匹配拆分字符串

String[] split(String regex,int limit):根据匹配跟定的正则表达式来拆分此字符串，最多不超过limit个，如果超过了，剩下的全部都放到最后一个元素中

#### **String与基本数据类型的转换**

String --> 基本数据类型、包装类：调用包装类的静态方法：parseXxx(str)

基本数据类型、包装类 --> String:调用String重载的valueOf(xxx)

#### **String与char[]的转换**

String --> char[]:调用String的toCharArray()

char[] --> String:调用String的构造器
```java
String str1 = "abc123";  
char[] charArray = str1.toCharArray();
for (int i = 0; i < charArray.length; i++) {
    System.out.println(charArray[i]);
}
// a/n b/nc /n ...

char[] arr = new char[]{'h','e','l','l','o'};
String str2 = new String(arr);
System.out.println(str2);// hello
```

#### **String与byte[]的转换**
编码：String --> byte[]:调用String的getBytes()

    解码：byte[] --> String:调用String的构造器
    编码：字符串 -->字节  (看得懂 --->看不懂的二进制数据)
    解码：编码的逆过程，字节 --> 字符串 （看不懂的二进制数据 ---> 看得懂）
    说明：解码时，要求解码使用的字符集必须与编码时使用的字符集一致，否则会出现乱码。
```java
@Test
public void test3() throws UnsupportedEncodingException {
    String str1 = "abc123中国";
    byte[] bytes = str1.getBytes();//使用默认的字符集，进行编码。
    System.out.println(Arrays.toString(bytes));

    byte[] gbks = str1.getBytes("gbk");//使用gbk字符集进行编码。中文字符一个字符 占连个字节 
    System.out.println(Arrays.toString(gbks));

    System.out.println("******************");

    String str2 = new String(bytes);//使用默认的字符集，进行解码。
    System.out.println(str2);

    String str3 = new String(gbks);
    System.out.println(str3);//出现乱码。原因：编码集和解码集不一致！

    String str4 = new String(gbks, "gbk");
    System.out.println(str4);//没有出现乱码。原因：编码集和解码集一致！
}
```
> 常见算法

> 1.模拟一个trim方法，去除字符串两端的空格

```java

```
> 2.将一个字符串进行反转。将字符串中指定部分进行反转。比如“abcdefg”反转为”abfedcg”

```java
    public static void main(String[] args) {
        StringDemo demo = new StringDemo();
        String Str = "abcdef";
        String reverse = demo.reverse2(Str, 2, 5);
        System.out.println(reverse);
    }
    public String reverse(String str,int startIndex,int endIndex){
        StringBuffer Str =new StringBuffer(str.substring(startIndex-1,endIndex));
        Str.reverse();
        String Str1 = new String(Str);
        StringBuffer Str2 =new StringBuffer(str);
        Str2.replace(startIndex-1,endIndex,Str1);
        String Str3 = new String(Str2);
        return Str3;
    }

    public String reverse1(String str,int startIndex,int endIndex){
        char[] str1 = str.toCharArray();
        for (int i = startIndex-1,j=endIndex-1;i<j;i++,j--){
            char temp = str1[i];
            str1[i] = str1[j];
            str1[j] = temp;
        }
        return new String(str1);
    }

    public String reverse2(String str,int startIndex,int endIndex){
        String reverseStr = str.substring(0,startIndex-1);
        for (int i = endIndex-1; i >=startIndex-1 ; i--) {
            reverseStr += str.charAt(i);
        }
        reverseStr += str.substring(endIndex);
        return reverseStr;
    }
```
> 3.获取一个字符串在另一个字符串中出现的次数。
比如:获取“ ab”在 “abkkcadkabkebfkabkskab” 中出现的次数

```java

```
> 4.获取两个字符串中最大相同子串。比如:
str1 = "abcwerthelloyuiodef“;str2 = "cvhellobnm" 提示:将短的那个串进行长度依次递减的子串与较长的串比较。

```java

```

> 5.对字符串中字符进行自然顺序排序。
> 提示:
> 1)字符串变成字符数组。 

> 2)对数组排序，选择，冒泡，Arrays.sort(); 

> 3)将排序后的数组变成字符串。 

```java

```

### StringBuffer和StringBuilder

    String、StringBuffer、StringBuilder三者的异同？
    String:不可变的字符序列；底层使用char[]存储
    StringBuffer:可变的字符序列；线程安全的，效率低；底层使用char[]存储
    StringBuilder:可变的字符序列；jdk5.0新增的，线程不安全的，效率高；底层使用char[]存储

    源码分析：
    String str = new String();//char[] value = new char[0];
    String str1 = new String("abc");//char[] value = new char[]{'a','b','c'};

    StringBuffer sb1 = new StringBuffer();//char[] value = new char[16];底层创建了一个长度是16的数组。
    System.out.println(sb1.length());//
    sb1.append('a');//value[0] = 'a';
    sb1.append('b');//value[1] = 'b';

    StringBuffer sb2 = new StringBuffer("abc");//char[] value = new char["abc".length() + 16];

    //问题1. System.out.println(sb2.length());//3
    //问题2. 扩容问题:如果要添加的数据底层数组盛不下了，那就需要扩容底层的数组。
             默认情况下，扩容为原来容量的2倍 + 2，同时将原有数组中的元素复制到新的数组中。

            指导意义：开发中建议大家使用：StringBuffer(int capacity) 或 StringBuilder(int capacity)
            
#### StringBuffer中的一些常用的方法：

StringBuffer append(xxx)：提供了很多的append()方法
，用于进行字符串拼接

StringBuffer delete(int start,int end)：删除指定位置的内容

StringBuffer replace(int start, int end, String str)：把[start,end)位置替换为str

StringBuffer insert(int offset, xxx)：在指定位置插入xxx

StringBuffer reverse() ：把当前字符序列逆转

public int indexOf(String str)

public String substring(int start,int end):返回一个从start开始到end索引结束的左闭右开区间的子字符串

public int length()

public char charAt(int n )

public void setCharAt(int n ,char ch)

总结：
    增：append(xxx)
    
    删：delete(int start,int end)
    
    改：setCharAt(int n ,char ch) / replace(int
    start, int end, String str)
    查：charAt(int n )
    
    插：insert(int offset, xxx)
    
    长度：length();
    
    *遍历：for() + charAt() / toString()
```java
    @Test
    public void test(){
        StringBuffer s1 = new StringBuffer("abc");
        s1.append(1);
        s1.append("1");
        System.out.println(s1);
//        s1.delete(2,4);
//        s1.replace(1,2,"hello");
        s1.insert(2,false);
        s1.reverse();
        System.out.println(s1);
        System.out.println(s1.length());

    }
```
对比String、StringBuffer、StringBuilder三者的效率：
    
    从高到底排列：StringBuilder>StringBuffer>String
```java
    @Test
    public void test3(){
        //初始设置
        long startTime = 0L;
        long endTime = 0L;
        String text = "";
        StringBuffer buffer = new StringBuffer("");
        StringBuilder builder = new StringBuilder("");
        //开始对比
        startTime = System.currentTimeMillis();
        for (int i = 0; i < 20000; i++) {
            buffer.append(String.valueOf(i));
        }
        endTime = System.currentTimeMillis();
        System.out.println("StringBuffer的执行时间：" + (endTime - startTime));

        startTime = System.currentTimeMillis();
        for (int i = 0; i < 20000; i++) {
            builder.append(String.valueOf(i));
        }
        endTime = System.currentTimeMillis();
        System.out.println("StringBuilder的执行时间：" + (endTime - startTime));

        startTime = System.currentTimeMillis();
        for (int i = 0; i < 20000; i++) {
            text = text + i;
        }
        endTime = System.currentTimeMillis();
        System.out.println("String的执行时间：" + (endTime - startTime));

    }
```
#### String、StringBuffer、Stringbuilder之间的转换
String-->StringBuffer、StringBuilder:调用StringBuffer、StringBuilder构造器

StringBuffer、Stringbuilder -->String：1⃣️调用String的构造器；
2⃣️StringBuffe、StringBuilder的ToString方法

#### JVM 中字符串亮亮吃存放位置：

jdk1.6: 字符串常量池存储在方法区（永久区）

jdk1.7: 字符串常量池存储在堆空间

jdk1.8: 字符串常量池存储在方法区（元空间）
## JDK 8 以前的日期时间API

1. System类中的currentTimeMillis()
```java
@Test
    public void test1(){
        long time = System.currentTimeMillis();
        //返回当前时间与1970年1月1日0时0分0秒之间以毫秒为单位的时间差。
        //称为时间戳
        System.out.println(time);
    }
```
2. java.util.Date类
           |---java.sql.Date类

    1. 两个构造器的使用
         >构造器一：Date()：创建一个对应当前时间的Date对象
         >构造器二：创建指定毫秒数的Date对象
         
    2. 两个方法的使用
         >toString():显示当前的年、月、日、时、分、秒
         >getTime():获取当前Date对象对应的毫秒数。（时间戳）

    3. java.sql.Date对应着数据库中的日期类型的变量
        >如何实例化
        >如何将java.util.Date对象转换为java.sql.Date对象

```java
@Test
    public void test2(){

        //构造器一：Date创建一个对应当前时间的Date对象
        Date date1 = new Date();
        System.out.println(date1.toString());//Tue Nov 03 10:05:50 CST 2020
        System.out.println(date1.getTime());//1604369150851

        //构造器二：创建指定好描述的Date对象
        Date date2 = new Date(1604369150851L);
        System.out.println(date2.toString());//Tue Nov 03 10:05:50 CST 2020

        //创建java.sql.Date
        java.sql.Date date3 = new java.sql.Date(1604369150851L);
        System.out.println(date3);//2020-11-03

        //如何将java.util,Date 对象转换为java.sql.Date对象
        //情况一
//        Date date4 = new java.sql.Date(1604369150851L);
//        java.sql.Date date5 = (java.sql.Date) date4;
//        System.out.println(date5);//2020-11-03

        //情况二
        Date date6 = new Date();
        java.sql.Date date7 = new java.sql.Date(date6.getTime());
        System.out.println(date7);//2020-11-03

    }
    //1.System类中的currentTimeMillis()
    @Test
    public void test1(){
        long time = System.currentTimeMillis();
        //返回当前时间与1970年1月1日0时0分0秒之间以毫秒为单位的时间差。
        //称为时间戳
        System.out.println(time);
    }
```
SimpleDateFormat的使用：SimpleDateFormat对日期Date类的格式化和解析

    1.两个操作：
    1.1 格式化：日期 --->字符串
    1.2 解析：格式化的逆过程，字符串 ---> 日期

    2.SimpleDateFormat的实例化
```java
@Test
public void test() throws ParseException {
    //实例化SimpleDateFormat
    SimpleDateFormat sdf = new SimpleDateFormat();

    //格式化：日期 --->字符串
    Date date = new Date();
    System.out.println(date);
    String format = sdf.format(date);
    System.out.println(format);

    //解析：格式化的逆过程，字符串 ---> 日期

    String str = "11/4/20 2:49 PM";
    Date date1 = sdf.parse(str);
    System.out.println(date1);


    //******按照指定的方式格式话*******
//  SimpleDateFormat sdf1 = new SimpleDateFormat("yyyyy.MMMMM.dd GGG hh:mm:ss aa");
    SimpleDateFormat sdf1 = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
 //格式化
    String format1 = sdf1.format(date);
    System.out.println(format1);
    //解析:要求字符串必须是符合SimpleDateFormat识别的格式（通过构造器参数体现），
    //否则抛异常
    Date parse = sdf1.parse("2020-11-05 08:58:20");
    System.out.println(parse);
    }
```
练习一：字符串"2020-09-09"，转换为java。sql.date

练习二："三天打鱼两天晒网" 从1990-01-01开始  2020-11-5是在打鱼还是在晒网

```java
 @Test
public void test1() throws ParseException {
    String birth = "2020-09-09";
    SimpleDateFormat sdf1 = new SimpleDateFormat("yyyy-MM-dd");
    Date date = sdf1.parse(birth);
    java.sql.Date date1 = new java.sql.Date(date.getTime());
    System.out.println(date1);
    }
@Test
public void test2() throws ParseException {
    String start = "1990-01-01";
    SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd");
//  Scanner scanner = new Scanner(System.in);
    String end = "2020-11-05";
    Date date = format.parse(start);
    Date date1 = format.parse(end);
    long date3 = date1.getTime()-date.getTime();
    long day_one = 24*60*60*1000;
    long day_sum = date3/day_one;
    double dors = day_sum%5;
    if (dors == 1 || dors == 2 || dors == 3){
        System.out.println("正在打鱼");
    }else{
        System.out.println("正在晒网");
    }
}
/*
Calendar 日历类(抽象类 )的使用
 */
@Test
public void test3(){
    Calendar calendar = Calendar.getInstance();

    int days = calendar.get(Calendar.DAY_OF_MONTH);
    System.out.println(days);
    System.out.println(calendar.get(Calendar.DAY_OF_YEAR));
    }
```
## JDK 8 中新日期时间API
java.time -包含值对象的基础包

java.time.chrono - 提供对不同的日历系统的访问

java.time,format - 格式话的解析时间和日期

java.time.temporal - 包括底层框架和扩展特性

java.time.zone -包含失去支持的类

    说明：大多数开发者只会用到基础包和format包，也可能会用到temporal包。
    因此，尽管友68个新的公开类型，大多数开发者，大概只会用到其中的三分之一

    LocalDate, LocalTime. LocalDateTime 的使用
    说明：
        1.LocalDateTime相对于LocalDate, LocalTime使用更多
        2.类似于Calendar
```java
@Test
public void test(){
    //now 获取当前日期、时间、日期+时间
    LocalDate localDate = LocalDate.now();
    LocalTime localTime = LocalTime.now();
    LocalDateTime localDateTime = LocalDateTime.now();
    System.out.println(localDate);
    System.out.println(localTime);
    System.out.println(localDateTime);

    //of，设置指定的年、月、日、时、分、秒。没偏移量
    LocalDateTime localDateTime1 =LocalDateTime.of(2020,11,3,10,05);
    System.out.println(localDateTime1);

    //getXxxx获取相关属性
    System.out.println(localDateTime.getDayOfMonh());
    System.out.println(localDateTime.getDayOfWee());
    System.out.println(localDateTime.getMonth());
    System.out.println(localDateTime.getMonthValue());
    System.out.println(localDateTime.getMinute());

    //提前不可变性
    //withXxx()设置相关属性
    LocalDate localDate1 =localDate.withDayOfMonth(22);
    System.out.println(localDate1);
    System.out.println(localDate);

    LocalDateTime localDateTime2 =localDateTime.withHour(4);
    System.out.println(localDateTime2);

    //不可变性
    LocalDateTime localDateTime3 =localDateTime.plusMonths(3);
    System.out.println(localDateTime);
    System.out.println(localDateTime3);

    LocalDateTime localDateTime4 =localDateTime.minusDays(6);
    System.out.println(localDateTime);
    System.out.println(localDateTime4);

}
```

### Instant的使用
```java
@Test
public void test2(){
    //now获取本初子午线对应的标准时间
    Instant instant = Instant.now();
    System.out.println(instant);//2020-11-05T02:8:58.281Z

    //添加时间的偏移量
    OffsetDateTime offsetDateTime =instant.atOffset(ZoneOffset.ofHours(8));
    System.out.println(offsetDateTime);//2020-1105T10:20:06.631+08:00

    //获取从1970年1月1日0时0分0秒（utc）开始的秒数->Date来的getTime()
    long milli = instant.toEpochMilli();
    System.out.println(milli);

    //ofEpochMilli()：通过给定的好描述，获取Instnt实例 --》Date(long millis)
    Intant instant1 =Instant.ofEpochMilli(1604542942309L);
    System.out.println(instant1);

}
```
### DateTimeFormatter：
格式化或解析时间日期类似与SimpleDateFormat
```java
@Test
public void test3(){
//  方式一：预定义的标准格式。如：ISO_LOCAL_DATE_TIME;ISO_LOCAL_DATE;ISO_LOCAL_TIME
    DateTimeFormatter formatter = DateTimeFormatter.ISO_LOCAL_DATE_TIME;
        //格式化：日期--->字符串
    LocalDateTime localDateTime = LocalDateTime.now();
    String str1 = formatter.format(localDateTime);
    System.out.println(localDateTime);//2020-11-05T10:29:51.570
    System.out.println(str1);//2020-11-05T10:29:51.57

//  解析：字符串--->日期
    TemporalAccessor parse = formatter.parse("2020-11-05T10:29:51.57");
    System.out.println(parse);
//  方式二：
//  本地化相关的格式。如：ofLocalizedDateTime()
//  FormatStyle.LONG / FormatStyle.MEDIUM / FormatStyle.SHORT :适用于LocalDateTime
    DateTimeFormatter formatter1 = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.LONG);
    //格式化
    String str2 = formatter1.format(localDateTime);
    System.out.println(str2);//2019年2月18日 下午03时47分16秒


//  本地化相关的格式。如：ofLocalizedDate()
//  FormatStyle.FULL / FormatStyle.LONG / FormatStyle.MEDIUM / FormatStyle.SHORT : 适用于LocalDate
    DateTimeFormatter formatter2 = DateTimeFormatter.ofLocalizedDate(FormatStyle.MEDIUM);
    //格式化
    String str3 = formatter2.format(LocalDate.now());
//        System.out.println(str3);//2019-2-18

//        重点： 方式三：自定义的格式。如：ofPattern(“yyyy-MM-dd hh:mm:ss”)
    DateTimeFormatter formatter3 = DateTimeFormatter.ofPattern("yyyy-MM-d hh:mm:ss");
    //格式化
    String Str4 = formatter3.format(LocalDateTime.now());
    System.out.println(Str4);

//  解析
    TemporalAccessor accessor = formatter3.parse(Str4);
    System.out.println(accessor);
}
```
## Java 比较器
Comparable接口的使用举例：  自然排序

    1.像String、包装类等实现了Comparable接口，重写了compareTo(obj)方法，给出了比较两个对象大小的方式。
    2.像String、包装类重写compareTo()方法以后，进行了从小到大的排列
    3. 重写compareTo(obj)的规则：
        如果当前对象this大于形参对象obj，则返回正整数，
        如果当前对象this小于形参对象obj，则返回负整数，
        如果当前对象this等于形参对象obj，则返回零。
    4. 对于自定义类来说，如果需要排序，我们可以让自定义类实现Comparable接口，重写compareTo(obj)方法。
       在compareTo(obj)方法中指明如何排序
```java
    @Test
    public void test1(){
        String[] arr = new String[]{"AA","CC","KK","MM","GG","JJ","DD"};
        //
        Arrays.sort(arr);

        System.out.println(Arrays.toString(arr));

    }

    @Test
    public void test2(){
        Goods[] arr = new Goods[5];
        arr[0] = new Goods("lenovoMouse",34);
        arr[1] = new Goods("dellMouse",43);
        arr[2] = new Goods("xiaomiMouse",12);
        arr[3] = new Goods("huaweiMouse",65);
        arr[4] = new Goods("microsoftMouse",43);

        Arrays.sort(arr);

        System.out.println(Arrays.toString(arr));
    }
```
Comparator接口的使用：定制排序

    1.背景：
    当元素的类型没有实现java.lang.Comparable接口而又不方便修改代码，
    或者实现了java.lang.Comparable接口的排序规则不适合当前的操作，
    那么可以考虑使用 Comparator 的对象来排序
    2.重写compare(Object o1,Object o2)方法，比较o1和o2的大小：
    如果方法返回正整数，则表示o1大于o2；
    如果返回0，表示相等；
    返回负整数，表示o1小于o2。
    
```java
   @Test
    public void test3(){
        String[] arr = new String[]{"AA","CC","KK","MM","GG","JJ","DD"};
        Arrays.sort(arr,new Comparator(){

            //按照字符串从大到小的顺序排列
            @Override
            public int compare(Object o1, Object o2) {
                if(o1 instanceof String && o2 instanceof  String){
                    String s1 = (String) o1;
                    String s2 = (String) o2;
                    return -s1.compareTo(s2);
                }
//                return 0;
                throw new RuntimeException("输入的数据类型不一致");
            }
        });
        System.out.println(Arrays.toString(arr));
    }

    @Test
    public void test4(){
        Goods[] arr = new Goods[6];
        arr[0] = new Goods("lenovoMouse",34);
        arr[1] = new Goods("dellMouse",43);
        arr[2] = new Goods("xiaomiMouse",12);
        arr[3] = new Goods("huaweiMouse",65);
        arr[4] = new Goods("huaweiMouse",224);
        arr[5] = new Goods("microsoftMouse",43);

        Arrays.sort(arr, new Comparator() {
            //指明商品比较大小的方式:按照产品名称从低到高排序,再按照价格从高到低排序
            @Override
            public int compare(Object o1, Object o2) {
                if(o1 instanceof Goods && o2 instanceof Goods){
                    Goods g1 = (Goods)o1;
                    Goods g2 = (Goods)o2;
                    if(g1.getName().equals(g2.getName())){
                        return -Double.compare(g1.getPrice(),g2.getPrice());
                    }else{
                        return g1.getName().compareTo(g2.getName());
                    }
                }
                throw new RuntimeException("输入的数据类型不一致");
            }
        });

        System.out.println(Arrays.toString(arr));
    }
}
class Goods implements  Comparable{

    private String name;
    private double price;

    public Goods() {
    }

    public Goods(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Goods{" +
                "name='" + name + '\'' +
                ", price=" + price +
                '}';
    }

    //指明商品比较大小的方式:按照价格从低到高排序,再按照产品名称从高到低排序
    @Override
    public int compareTo(Object o) {
//        System.out.println("**************");
        if(o instanceof Goods){
            Goods goods = (Goods)o;
            //方式一：
            if(this.price > goods.price){
                return 1;
            }else if(this.price < goods.price){
                return -1;
            }else{
//                return 0;
               return -this.name.compareTo(goods.name);
            }
            //方式二：
//           return Double.compare(this.price,goods.price);
        }
//        return 0;
        throw new RuntimeException("传入的数据类型不一致！");
    }
}

```
## 其他常用类
    1.System
    2.Math
    3.BigInteger 和 BigDecimal
 