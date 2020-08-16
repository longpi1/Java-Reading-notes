# 关于Spring框架的总结（三、Spring AOP）
## 3.Spring AOP

> Spring AOP
> 3.1.Spring AOP的基本概念
> 3.2.动态代理
> 3.3.AOP的常见术语
> 3.4.基于XML配置开发
> 3.5.基于注解开发


## 3.1.Spring AOP的基本概念
<p><font color=red>AOP (Aspect- Oriented Programming) 即面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。</font color=red>它与OOP (Object-Otiented
Programming,面向对象编程)相辅相成， 提供了与OOP不同的抽象软件结构的视角。在OOP中，以类作为程序的基本单元，而AOP中的基本单元是Aspect (切面)。利用AOP可以对业务逻辑的各个部分进行隔离,从而使得业务逻辑各部分之间的耦合度降低,提高程序的可重用性，同时提高了开发的效率。
</p>
<p>简单的说它就是把我们程序重复的代码抽取出来，在需要执行的时候，使用动态代理的技术，在不修改源码的基础上，对我们的已有方法进行增强。</p>



## 3.2.动态代理
**动态代理常用的有两种方式**
**（1）、JDK动态代理（基于接口的动态代理）**
1.创建接口以及实现类
```java
public interface IActor {
/**
* 基本演出
* @param money
*/
public void basicAct(float money);
/**
* 危险演出
* @param money
*/
public void dangerAct(float money);
}
/**
* 一个演员
*/
//实现了接口，就表示具有接口中的方法实现。即：符合经纪公司的要求
public class Actor implements IActor{
public void basicAct(float money){
System.out.println("拿到钱，开始基本的表演："+money);
}
public void dangerAct(float money){
System.out.println("拿到钱，开始危险的表演："+money);
} 
}
```
2.创建代理类

```java
public class Client {
public static void main(String[] args) {
//一个剧组找演员：
final Actor actor = new Actor();//直接
/**
* 代理：
* 间接。
* 获取代理对象：
* 要求：
* 被代理类最少实现一个接口
* 创建的方式
* Proxy.newProxyInstance(三个参数)
* 参数含义：
* ClassLoader：和被代理对象使用相同的类加载器。
* Interfaces：和被代理对象具有相同的行为。实现相同的接口。
* InvocationHandler：如何代理。
* 策略模式：使用场景是：
* 数据有了，目的明确。
* 如何达成目标，就是策略。
* 
*/
IActor proxyActor = (IActor) Proxy.newProxyInstance(
actor.getClass().getClassLoader(), 
actor.getClass().getInterfaces(), 
new InvocationHandler() {
/**
* 执行被代理对象的任何方法，都会经过该方法。
* 此方法有拦截的功能。
* 
* 参数：
* proxy：代理对象的引用。不一定每次都用得到
* method：当前执行的方法对象
* args：执行方法所需的参数
* 返回值：
* 当前执行方法的返回值
*/
@Override
public Object invoke(Object proxy, Method method, Object[] args) 
throws Throwable {
String name = method.getName();
Float money = (Float) args[0];
Object rtValue = null;
//每个经纪公司对不同演出收费不一样，此处开始判断
if("basicAct".equals(name)){
//基本演出，没有 2000 不演
if(money > 2000){
//看上去剧组是给了 8000，实际到演员手里只有 4000
//这就是我们没有修改原来 basicAct 方法源码，对方法进行了增强
rtValue = method.invoke(actor, money/2);
} }
if("dangerAct".equals(name)){
//危险演出,没有 5000 不演
if(money > 5000){
//看上去剧组是给了 50000，实际到演员手里只有 25000
//这就是我们没有修改原来 dangerAct 方法源码，对方法进行了增强
rtValue = method.invoke(actor, money/2);
} }
return rtValue;
}
});
//没有经纪公司的时候，直接找演员。
// actor.basicAct(1000f);
// actor.dangerAct(5000f);
//剧组无法直接联系演员，而是由经纪公司找的演员
proxyActor.basicAct(8000f);
proxyActor.dangerAct(50000f);
} }
```

**（2）、CGLIB动态代理（基于子类的动态代理）**
1.创建目标类

```java
public class Actor{//目标类
public void basicAct(float money){
System.out.println("拿到钱，开始基本的表演："+money);
}
public void dangerAct(float money){
System.out.println("拿到钱，开始危险的表演："+money);
} 
}
```

2.创建代理类

```java
public class Client {
/**
* 基于子类的动态代理
* 要求：
* 被代理对象不能是最终类
* 用到的类：
* Enhancer
* 用到的方法：
* create(Class, Callback)
* 方法的参数：
* Class：被代理对象的字节码
* Callback：如何代理
* @param args
*/
public static void main(String[] args) {
final Actor actor = new Actor();
Actor cglibActor = (Actor) Enhancer.create(actor.getClass(),
new MethodInterceptor() {
/**
* 执行被代理对象的任何方法，都会经过该方法。在此方法内部就可以对被代理对象的任何
方法进行增强。
* 
* 参数：
* 前三个和基于接口的动态代理是一样的。
* MethodProxy：当前执行方法的代理对象。
* 返回值：
* 当前执行方法的返回值
*/
@Override
public Object intercept(Object proxy, Method method, Object[] args, 
MethodProxy methodProxy) throws Throwable {
String name = method.getName();
Float money = (Float) args[0];
Object rtValue = null;
if("basicAct".equals(name)){
//基本演出
if(money > 2000){
rtValue = method.invoke(actor, money/2);
}
}
if("dangerAct".equals(name)){
//危险演出
if(money > 5000){
rtValue = method.invoke(actor, money/2);
} }
return rtValue;
}
});
cglibActor.basicAct(10000);
cglibActor.dangerAct(100000);
}
 }
```

## 3.3.AOP的常见术语


➊**切面**

切面(Aspect) 是指封装横切到系统功能(例如事务处理)的类。

❷**连接点**

连接点(Joinpoint)是指程序运行中的一些时间点， 例如方法的调用或异常的抛出。

❸**切入点**

切入点(Poineu)是指需要处理的连接点。在Spring AOP中，所有的方法执行都是连接点，而切入点是一个描述信息， 它修饰的是连接点。

❹**通知**

通知(Advice) 是由切面添加到特定的连接点(满足切入点规则)的一段代码，即在定义好的切入点处所要执行的程序代码，可以将其理解为切面开启后切面的方法，因此通知是切面的具体实现。

❺**引入**

引入( Introduction)允许在现有的实现类中添加自定义的方法和属性。

❻**目标对象**

目标对象(Target Object)是指所有被通知的对象。如果AOP框架使用运行时代理的方式(动态的AOP)来实现切面，那么通知对象总是一个代理对象。

❼**代理**

代理(Proxy) 是通知应用到目标对象之后被动态创建的对象。

❽**织人**

<p>织入( Weaving)是将切面代码插入到目标对象上，从而生成代理对象的过程。根据不同的实现技术，AOP 织入有3种方式:编译期织入，需要有特殊的Java 编译器;类装载期织入，需要有特殊的类装载器:动态代理织入，在运行期为目标类添加通知生成子类的方式。Spring AOP框架默认采用动态代理织入，而AspectJ (基于Java语言的AOP框架)</p>




**<a herf="https://blog.csdn.net/qq_42009262/article/details/105013992">Spring通知类型介绍**

根据Spring中通知在目标类方法的链接点位置，可以分为6种类型：

**（1）环绕通知：**
实现接口 |  功能描述      | 
|:--------:| -------------:|
| org.aopalliance.itercept.MethodInterceptor|  在目标方法执行前和执行后实施增强。可以用于日志记录、事务处理等功能。|

**（2）前置通知：**
实现接口 |  功能描述      | 
|:--------:| -------------:|
| org.springframework.aop.MethodBeforeAdvice|  在目标方法执行前实施增强。可以用于权限管理等功能。|

**（3）后置返回通知**：
实现接口 |  功能描述      | 
|:--------:| -------------:|
| org.springframework.aop.AfterReturningAdvice|  在目标方法成功执行后实施增强。可以用于关闭流、删除临时文件等功能。|

**（4）后置（最终）通知：**
实现接口 |  功能描述     | 
|:--------:| -------------:|
| org.springframework.aop.AfterAdvice|  在目标方法执行后实施增强。与后置返回通知不同的是，不管是否发生异常都要执行该通知，可应用于释放资源。|

**（5）异常通知：**
实现接口 |  功能描述      | 
|:--------:| -------------:|
| org.springframework.aop.ThrowsAdvice|  在方法抛出异常后实施增强。可以用于异常处理、记录日志等功能。|

**（6）引入通知：**
实现接口 |  功能描述      | 
|:--------:| -------------:|
| org.springframework.aop.IntroductInterceptor|  在目标类中添加一些新的方法和属性。可以用于修改目标类（增强类）。|


## 3.4.基于XML配置开发
**1.把通知类用 bean 标签配置起来**

```java
<!-- 配置通知 -->
 <bean id="txManager" class="com.itheima.utils.TransactionManager"> 
<property name="dbAssit" ref="dbAssit"></property>
</bean>
```

**2.使用 aop:config 声明 aop 配置**

```java
作用：用于声明开始 aop 的配置
<aop:config>
<!-- 配置的代码都写在此处 -->
</aop:config>
```
**3.使用 aop:aspect 配置切面**

```java
aop:aspect:
作用：
用于配置切面。
属性：
id：给切面提供一个唯一标识。
ref：引用配置好的通知类 bean 的 id。
 <aop:aspect id="txAdvice" ref="txManager">
<!--配置通知的类型要写在此处-->
</aop:aspect>
```

**4.使用 aop:pointcut 配置切入点表达式**

```java
aop:pointcut：
作用：
用于配置切入点表达式。就是指定对哪些类的哪些方法进行增强。
属性：
expression：用于定义切入点表达式。
id：用于给切入点表达式提供一个唯一标识
<aop:pointcut id="pt1" expression="execution(
execution:匹配方法的执行(常用)
execution(表达式)
表达式语法：execution([修饰符] 返回值类型 包名.类名.方法名(参数))" />
```

**5.使用 aop:xxx 配置对应的通知类型**

```java
aop:before
作用：
用于配置前置通知。指定增强的方法在切入点方法之前执行
属性：
method:用于指定通知类中的增强方法名称
ponitcut-ref：用于指定切入点的表达式的引用
poinitcut：用于指定切入点表达式
执行时间点：
切入点方法执行之前执行
<aop:before method="beginTransaction" pointcut-ref="pt1"/>
aop:after-returning
作用：
用于配置后置通知
属性：
method：指定通知中方法的名称。
pointct：定义切入点表达式
pointcut-ref：指定切入点表达式的引用
执行时间点：
切入点方法正常执行之后。它和异常通知只能有一个执行
<aop:after-returning method="commit" pointcut-ref="pt1"/>
aop:after-throwing
作用：
用于配置异常通知
属性：
method：指定通知中方法的名称。
pointct：定义切入点表达式
pointcut-ref：指定切入点表达式的引用
执行时间点：
切入点方法执行产生异常后执行。它和后置通知只能执行一个
<aop:after-throwing method="rollback" pointcut-ref="pt1"/>
aop:after
作用：
用于配置最终通知
属性：
method：指定通知中方法的名称。
pointct：定义切入点表达式
pointcut-ref：指定切入点表达式的引用
执行时间点：
无论切入点方法执行时是否有异常，它都会在其后面执行。
<aop:after method="release" pointcut-ref="pt1"/>
```

## 3.5.基于注解开发
**1.配置类的编写**

```java
@Configuration
@ComponentScan(basePackages="包名")
@EnableAspectJAutoProxy
public class SpringConfiguration {
}
```
**2.通知类的编写**

```java
@Component("txManager")
@Aspect//表明当前类是一个切面类
public class TransactionManager {
//定义一个 DBAssit
@Autowired
private DBAssit dbAssit ;
 }
```
**3.在增强的方法上使用注解配置通知**

```java
@Before
作用：
把当前方法看成是前置通知。
属性：
value：用于指定切入点表达式，还可以指定切入点表达式的引用。
//开启事务
@Before("execution(* com.itheima.service.impl.*.*(..))")
public void beginTransaction() {
try {
dbAssit.getCurrentConnection().setAutoCommit(false);
} catch (SQLException e) {
e.printStackTrace();
} 
}
@AfterReturning
作用：
把当前方法看成是后置通知。
属性：
value：用于指定切入点表达式，还可以指定切入点表达式的引用
//提交事务
@AfterReturning("execution(* com.itheima.service.impl.*.*(..))")
public void commit() {
try {
dbAssit.getCurrentConnection().commit();
} catch (SQLException e) {
e.printStackTrace();
} }
@AfterThrowing
作用：
把当前方法看成是异常通知。
属性：
value：用于指定切入点表达式，还可以指定切入点表达式的引用
//回滚事务
@AfterThrowing("execution(* com.itheima.service.impl.*.*(..))")
public void rollback() {
try {
dbAssit.getCurrentConnection().rollback();
} catch (SQLException e) {
e.printStackTrace();
} }
@After
作用：
把当前方法看成是最终通知。
属性：
value：用于指定切入点表达式，还可以指定切入点表达式的引用
//释放资源
@After("execution(* com.itheima.service.impl.*.*(..))")
public void release() {
try {
dbAssit.releaseConnection();
} catch (Exception e) {
e.printStackTrace();
}
 }
 @Around
作用：
把当前方法看成是环绕通知。
属性：
value：用于指定切入点表达式，还可以指定切入点表达式的引用。
/**
* 环绕通知
* @param pjp
* @return
*/
@Pointcut
作用：
指定切入点表达式
属性：
value：指定表达式的内容
@Pointcut("execution(* com.itheima.service.impl.*.*(..))")
private void pt1() {}
@Around("pt1()")
public Object transactionAround(ProceedingJoinPoint pjp) {
//定义返回值
Object rtValue = null;
try {
//获取方法执行所需的参数
Object[] args = pjp.getArgs();
//前置通知：开启事务
beginTransaction();
//执行方法
rtValue = pjp.proceed(args);
//后置通知：提交事务
commit();
}catch(Throwable e) {
//异常通知：回滚事务
rollback();
e.printStackTrace();
}finally {
//最终通知：释放资源
release();
}
return rtValue;
 }
```

**参考文献：[1] 陈恒，楼偶俊，张立杰.Java EE框架整和开发入门到实践[M].清华大学出版社，2018-：.39-45**
**案例来自于黑马程序员Spring教学**
