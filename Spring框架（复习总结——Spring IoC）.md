# 关于Spring框架的总结（二、Spring IoC）
## 2.Spring IoC

> Spring IoC
> 2.1.Spring IoC的基本概念
> 2.2.Spring IoC容器
> 2.3.Spring IoC中的bean标签
> 2.4.依赖注入

## **2.1.Spring IoC的基本概念**
<p>控制反转(Inversion of Control, IoC)是一个比较抽象的概念，是Spring框架的核心，用来消减计算机程序的精合问题。<font color=red>对于spring框架来说，就是由spring来负责控制对象的生命周期和对象间的关系。</font >依赖注入(Dpendency Injection,DI)是IOC的另一种说法，只是换了种角度来描述相同的概念，下面举一个例子来解释IOC和DI。</p>
<p>当人们需要一件东西时， 第一反应就是找东西， 例如想吃面包。在没有面包店和有面包店两种情况下，您会怎么做?在没有面包店时，最直观的做法可能是您按照自己的口味制作面包，也就是一个面包需要主动制作。然而时至今日，各种网店、实体店盛行，已经没有必要自己制作面包。想吃面包了，去网店或实体店把自己的口味告诉店家，一会就可以吃到面包了。注意，您并没有制作面包，而是由店家制作，但是完全符合您的口味。</p>
<p>
上面只是列举了一个非常简单的例子，但包含了控制反转的思想，即把制作面包等主动权交给店家。下面通过面向对象编程思想继续探讨这两个概念


当某个Java对象(调用者，例如您)需要调用另一个Java 对象(被调用者，即被依赖对象，例如面包)时，在传统编程模式下，调用者通常会采用“new被调用者”的代码方式来创建对象(例如您自已制作面包)。这种方式会增加调用者与被调用者之间的耦合性，不利于后期代码的升级与维护。

<font color=red>当Spring框架出现后，对象的实例不再由调用者来创建，而是由Spring容器(例如面包店)来创建。Spring容器会负责控制程序之间的关系(例如面包店负责控制您与面包的关系)，而不是由调用者的程序代码直接控制。这样，控制权由调用者转移到Spring容器，控制权发生了反转，这就是Spring的控制反转。

从Spring容器角度来看，Spring容器负责将被依赖对象赋值给调用者的成员变量，相当于为调用者注入它所依赖的实例，这就是Spring的依赖注入。

综上所述，控制反转是种通过描述 (在 Spring中可以是XML或注解)并通过第三方去产生或获取特定对象的方式。在Spring中实现控制反转的是loC容器，其实现方法是依赖注入。




## **2.2.Spring IoC容器**
**1.Spring IoC容器设计主要是基于BeanFactoty和ApplicationContext两个接口。下图为spring工厂的类结构体**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200712110712194.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDA5MjYy,size_16,color_FFFFFF,t_70)
BeanFactory 是 Spring 容器中的顶层接口，ApplicationContext 是它的子接口。

**2.BeanFactory 和 ApplicationContext 的区别**
BeanFactory 和 ApplicationContext 的区别：
创建对象的时间点不一样：

 - （1.）BeanFactory：什么使用什么时候创建对象。
 - （2.）ApplicationContext：只要一读取配置文件，默认情况下就会创建对象。




**3.ApplicationContext 接口的实现类**

 - 3.1ClassPathXmlApplicationContext：从类的根路径(src根目录)寻找指定的XML配置文件 ，最常使用。

```java
//初始化Spring容器ApplicationContext ，加载配置文件
ApplicationContext applicationContext=new ClassPathXMLApplicationContext（“ApplicationContext.xml”）
```

 - 3.2FileSystemXmlApplicationContext：从磁盘的绝对路径中寻找指定的XML配置文件，配置文件可以在磁盘的任意位置（限制比较大，如果换了设备或者文件xml配置位置改变将找不到配置文件，不推荐）。
 
  ```java
//初始化Spring容器ApplicationContext ，加载配置文件
ApplicationContext applicationContext=new FileSystemXmlApplicationContext（“D:\文件位置\src\ApplicationContext.xml”）
```
 - 3.3AnnotationConfigApplicationContext:使用注解配置容器对象时，需要使用此类来创建 spring 容器。用来读取注解。
  ```java
//初始化Spring容器ApplicationContext ，加载配置文件
ApplicationContext applicationContext=new AnnotationConfigApplicationContext（“注解配置类.class”）
```



## **2.3.Spring IoC中的bean标签**
**1.`<bean>`元素的常用属性及其子元素**


| 属性或子元素名称 |描述  |
|--|--|
|id  |Bean在BeanFactory中的唯一标识，在代码中通过BeanFactory获取Bean实例时需要以此作为索引名称 |
| class |  Bean的具体实现类，使用类的名|
| scope |  指定Bean实例的作用域，具体属性值及含义|
|`<constructor-arg>` |  `<bean>`元素的子元素，使用构造方法注入，指定构造方法的参数。该元素的index属性指定参数的序号，ref属性指定对BeanFactory中其他Bean的引用关系，type属性指定参数类型，value属性指定参数的常量值|
|`<property>`  |`<bean>`元素的子元素，用于设置一个属性。该元素的name属性指定Bean实例中相应的属性名称，value 属性指定Bean的属性值，ref 属性指定属性对BeanFactory中其他Bean的引用关系  |
|  `<list>`| `<property>`元素的子元素，用于封装List 或数组类型的依赖注入 |
| `<map>` | `<property>`元素的子元素，用于封装Map类型的依赖注入 |
| `<set>` | `<property>`元素的子元素，用于封装Set类型的依赖注入 |
|`<entry>`  |  `<map>`元素的子元素，用于设置一个键值对|


**2.bean 的作用范围和生命周期**
scope：指定对象的作用范围。
* <font color=red>singleton :默认值，单例的.
* <font color=red> prototype :多例的.
* request :WEB 项目中,Spring 创建一个 Bean 的对象,将对象存入到 request 域中.
* session :WEB 项目中,Spring 创建一个 Bean 的对象,将对象存入到 session 域中.
* global session :WEB 项目中,应用在 Portlet 环境.如果没有 Portlet 环境那么
globalSession 相当于 session.

<font color=red>单例对象：scope="singleton"</font>
一个应用只有一个对象的实例。它的作用范围就是整个引用。
生命周期：

 - 对象出生：当应用加载，创建容器时，对象就被创建了。
 - 对象活着：只要容器在，对象一直活着。
 - 对象死亡：当应用卸载，销毁容器时，对象就被销毁了。





<font color=red>多例对象：scope="prototype"</font>
每次访问对象时，都会重新创建对象实例。
生命周期：

 -   对象出生：当使用对象时，创建新的对象实例。
 -   对象活着：只要对象在使用中，就一直活着。
 -    对象死亡：当对象长时间不用时，被 java 的垃圾回收器回收了。






**3.bean的实例化**

 - 第一种方式：使用默认无参构造函数
  ```java
<bean id="类名" class="类位置"/>
```


 - 第二种方式：spring 管理静态工厂-使用静态工厂的方法创建对象
 ```java
/**
* 模拟一个静态工厂，创建业务层实现类
*/
public class StaticFactory {
        public static IAccountService createAccountService(){
             return new AccountServiceImpl();
} }
<!-- 此种方式是:使用 StaticFactory 类中的静态方法 createAccountService 创建对象，并存入 spring 容器
id 属性：指定 bean 的 id，用于从容器中获取
class 属性：指定静态工厂的全限定类名
factory-method 属性：指定生产对象的静态方法
-->
 <bean id="accountService"   class="com.itheima.factory.StaticFactory"   
  factory-method="createAccountService"></bean>
```


 - 第三种方式：spring 管理实例工厂-使用实例工厂的方法创建对象
 ```java
/**
* 模拟一个实例工厂，创建业务层实现类
* 此工厂创建对象，必须现有工厂实例对象，再调用方法
*/
public class InstanceFactory {
public IAccountService createAccountService(){
return new AccountServiceImpl();
} }
<!-- 此种方式是：
先把工厂的创建交给 spring 来管理。
然后在使用工厂的 bean 来调用里面的方法
factory-bean 属性：用于指定实例工厂 bean 的 id。
factory-method 属性：用于指定实例工厂中创建对象的方法。
-->
 <bean id="instancFactory" class="com.itheima.factory.InstanceFactory"></bean>
  <bean id="accountService"  factory-bean="instancFactory"
 factory-method="createAccountService"></bean>
```






## **2.4.依赖注入**
**在Spring中实现IoC容器的方法是依赖注入，依赖注入的作用是在使用Spring框架创建对象时动态地将其所依赖的对象(例如属性值)注入Bean组件中。Spring框架的依赖注入通常有两种实现方式，一种是使用构造方法注入，另一种是使用属性的setter方法注入。**

**1.使用构造方法注入**

```java
<!-- 使用构造函数的方式，给 service 中的属性传值
要求：
类中需要提供一个对应参数列表的构造函数。
涉及的标签：
constructor-arg
属性：
index:指定参数在构造函数参数列表的索引位置
type:指定参数在构造函数中的数据类型
name:指定参数在构造函数中的名称 用这个找给谁赋值
=======上面三个都是找给谁赋值，下面两个指的是赋什么值的==============
value:它能赋的值是基本数据类型和 String 类型
ref:它能赋的值是其他 bean 类型，也就是说，必须得是在配置文件中配置过的 bean
--> 
<bean id="类" class="类的位置"> 
<constructor-arg name="属性名" value="属性值"></constructor-arg> 
<constructor-arg name="属性名" ref="其他bean类型的id"></constructor-arg>
</bean> 
<bean id="其他bean类型的id" class="类型">
</bean>
```


**2.使用属性的setter方法注入**

```java
<!-- 通过配置文件给 bean 中的属性传值：使用 set 方法的方式
涉及的标签：
property
name：找的是类中 set 方法后面的部分
ref：给属性赋值是其他 bean 类型的
value：给属性赋值是基本数据类型和 string 类型的
实际开发中，此种方式用的较多。
--> 
<bean id="类" class="类的位置"> 
<property name="属性名" value="属性值"></property> 
<property name="属性名" ref="其他bean类型的id"></property>
</bean> 
<bean id="其他bean类型的id" class="类型">
</bean>
```











**参考文献：[1] 陈恒，楼偶俊，张立杰.Java EE框架整和开发入门到实践[M].清华大学出版社，2018-：.12-16**
**案例来自于黑马程序员Spring教学**
