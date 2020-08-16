> 复习日期相关类写的总结，有错误欢迎大家指出

## 日期相关类

<h4>一.Date类

<h6>A.构造方法


| 构造方法 | 方法作用 |
|--|--|
|Date()  | 根据当前系统时间创建 日期对象 |
| Date(long time) |  分配一个 Date对象，并将其初始化为表示自称为“时代”的标准基准时间以后的指定毫秒数，即1970年1月1日00:00:00 GMT。 |

<h6>B.成员方法

| 成员方法 | 方法作用 |
|--|--|
|long getTime(）|获取当前日期对象的毫秒值时间，与System.currentTimeMillis()效果相同 |
| String toLocaleString() |  根据本地格式转换日期对象|

<h4>二.DateFormat类&SimpleDateFormat类

> DateFormat类定义在java.test包中，是日期/时间格式化子类的抽象类。
> SimpleDateFormat是一个具体的类并且是DateFormat的子类，用于以区域设置敏感的方式格式化和解析日期。 它允许格式化（日期文本），解析（文本日期）和归一化。 

<h6>A.构造方法

| 构造方法 | 方法作用 |
|--|--|
|SimpleDateFormat(String s) | 根据指定模板创建 日期格式化对象 |
**模板创建样式以及结果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200602193601935.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDA5MjYy,size_16,color_FFFFFF,t_70)
<h6>B.成员方法

 成员方法 | 方法作用 |
|--|--|
|String format(Date d) |根据指定格式格式化日期对象 |
| Date parse(String S) |  根据指定格式解析字符串|



<h4>三.Calendar类

> Calendar类是一个抽象类，可以为在某一特定时刻和一组之间的转换的方法calendar fields如YEAR ， MONTH ， DAY_OF_MONTH ， HOUR ，等等，以及用于操纵该日历字段，如获取的日期下个星期。 时间上的瞬间可以用毫秒值表示，该值是从1970年1月1日00:00 00：00.000 GMT（Gregorian）的Epoch的偏移量。 

<h6>A.创建对象方式

```java
Calendar C = Calendar.newInstance();

//获取日历类对象
```

<h6>B.成员方法

 成员方法 | 方法作用 |
|--|--|
|int getint n) |获取指定日历字段信息 |
| void set(int n,int value) |将指定 日历字段设置为指定的值
|void add(int n,int value) |将指定 日历字段增加或减少指定的值
|void getTime() |将Calendar 类转换成Date类对象








