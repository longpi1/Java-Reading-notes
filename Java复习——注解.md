><b> <h5>注解分类：
> <h6>1.普通注解
> <h6>2.元注解
><h6> 3.自定义注解<b>


<h5>一、普通注解

<b>：修饰java代码的注解

|JDK预定义的普通注解  | 作用 | 
|--|--|--|
|@Override  | 表示继承和改写 |  自带型注解 |
| @FunctionInterface | 声明功能性接口 |   |
|@Deprecated  | 表示废弃 |   |
|@SuppressWarnings  | 表示压制警告 |   |
| @SafeVarargs |不会对不定项参数做危险操作自带注解  |   |
|  |  |   |


**@Override：**

1.修饰方法,检查该方法是父类的方法
2.强制该函数代码必须符合父类中该方法的定义
3.避免代码错误

**@FunctionInterface**
声明功能性接口，比如判断java9新特性的函数式接口。

**@Deprecated**
1.修饰类/类的元素/包
2.标注为废除，建议不再使用这个类/元素/包


**@SuppressWarnings**
1.可以修饰变量/方法/构造函数/类等
2.压制各种不同类型的警告信息，使得编译器不显示警告
3.各种不同类型是叠加，如修饰类的警告类型，和修饰方法的警告类型，对于方法来说，是叠加的。
4.警告类型名称是编译器/IDE工具自己定的，Java规范没有强制要求哪些名称。编译器厂商需要自行协商，保证同名警告类型在各个编译器.上一样工作。

**@SafeVarargs**
不会对不定项参数做危险操作自带注解





<h5>二、元注解

|JDK预定义的元注解  | 作用 | 
|--|--|--|
|@Target  | 设置目标范围 |  自带型注解 |
| @Retention | 设置保持性 |   |
| @Documented  | 修饰文档 |   |
|@Inherited | 注解继承 |   |
| @Repeatable |可以重复修饰元注解  |   |


**@Target**
**限定目标注解作用于什么位置@Target({ElementType.METHOD})**

 - ElementType.ANNOTATION_ _TYPE (注:修饰注解)
 - ElementType.CONSTRUCTO
 - ElementType.FIELD
 - ElementType.LOCAL _VARIABLE .
 - ElementType.METHOD
 -  ElementType.PACKAGE
 - ElementType.PARAMETER
 - ElementType.TYPE (注:任何类型，即。上面的的类型都可以修饰)


**@Retention**

示例 : @Retention(RetentionPolicy.RUNTIME)

 - 这个注解用来修饰其他注解的存在范围
- RetentionPolicy.SOURCE注解仅存在源码，不在class 文件。
- RetentionPolicy.CLASS这是默认的注解保留策略。注解存在于.class文件，但是不能被JVM加载。
- RetentionPolicy.RUNTIME这种策略下，注解可以被JVM运行时访问到。通常情况下，可以结合反射来做一些事情。


**@Documented**

 - 指明这个注解可以被Javadoc工具解析，形成帮助文档


**@Inherited**

 -  让一个类和它的子类都包含某个注解
 -  普通的 注解没有继承功能


**@Repeatable**
 - 自JDK1.8引入
 - 表示被修饰的注解可以重复应用标注，-需要定义注解和容器注解

<h5>三、自定义注解

 **A. 自定义注解定义: 扩展java.lang.annotation.Annotation注解接口
 B. 注解可以包括的类型：**
1.8种 基本类型(int/ short/ long/ float/ double/byte/ char/boolean)
2.String
3.Class
4.enum类型,
5.注解类型
6.由前面类型组成的数组

 **C. 注解使用的位置**
**1.@Target可以限定位置
2.允许的位置**
包
类
接口
方法
构造器
成员变量
局部变量
形参变量
类型参数



参考：陈良育老师的java程序设计https://www.icourse163.org/course/ECNU-1206500807?tid=1206823217
