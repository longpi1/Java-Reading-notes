# 关于Spring框架的总结（一、spring的简单概述）


## 1.简介概述
> 关于Spring框架的简单介绍
> 1.1.什么是spring？
> 1.2.spring框架有哪些特点优势？
> 1.3.spring框架的体系结构


## **1.1.什么是spring**

   <p>    Spring是一个轻量级Java开发框架，最早由Rod Johnson创建，目的是为了解决企业级应用开发的业务逻辑层和其他各层的耦合问题。它是一个分层的JavaSE/EE full-stack(-站式)轻量级开源框架，为开发Java应用程序提供全面的基础架构支持。<font color=red>以 IoC（Inverse Of Control：反转控制）和 AOP（Aspect Oriented Programming：面向切面编程）为内核</font color=red>，提供了展现层 Spring MVC 和持久层 Spring JDBC 以及业务层事务管理等众多的企业级应用技术，还能整合开源世界众多著名的第三方框架和类库。Spring 负责基础架构，因此Java开发者可以专注于应用程序的开发。   </p>

## 1.2.spring框架有哪些特点优势？

1.降低组件之间的耦合性，通过 Spring 提供的 IoC 容器，可以将对象间的依赖关系交由 Spring 进行控制，避免硬编码所造成的过度程序耦合。

2.对AOP编程的支持，方便面向切面编程。

3.通过声明式方式灵活的进行事务的管理，提高开发效率和质量。

4.方便集成各种优秀框架，如hibernate,Struts2,JPA等

5.Spring具有高度可开放性，并不强制依赖于Spring，可以自由选择Spring部分或全部

6.方便程序的测试，可以用非容器依赖的编程方式进行几乎所有的测试工作。

## 1.3.spring框架的体系结构

Spring框架至今已集成20多个模块，这些模块发布在核心容器（Core Container）、数据访问/集成(Data Access/Integration) 层. Web层. AOP ( Aspect Oriented Programming,
面向切面的编程)模块、植入(Instrumentation) 模块、消息传输(Messaging)和测试(Test)模块中，如下图。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200709115142920.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDA5MjYy,size_16,color_FFFFFF,t_70)


**1、核心容器**
Spring的核心容器是其他模块建立的基础，由Spring core 、Spring beans、 Sping comtext、Spring context-support和Spring expression (Spring 表达式语言)等模块组成。
| 模块 |功能作用  |
|--|--|
|  Spring-core 模块| 提供了框架的基本组成部分，包括控制反转(Inversion of Control,IoC)和依赖注入(Dependency Injection, DI)功能。 |
| Spring-beans模块 |提供了BeanFactory, 是工厂模式的一个经典实现，Spring将管理对象称为Bean。  |
| Spring-context 模块 |建立在Core和Beans模块的基础之上，提供一个框架式的对象访问方式，是访问定义和配置的任何对象的媒介。ApplicationContext 接口是Context模块的焦点。  |
|Spring-context-support 模块  |支持整合第三方库到Spring 应用程序上下文，特别是用于高速缓存(EhCache、JCache)和任务调度(CommonJ、Quartz) 的支持。  |
|Spring-expression 模块  |  提供了强大的表达式语言去支持运行时查询和操作对象图。这是对JSP2.1规范中规定的统一表达式语言 (UnifiedEL) 的扩展。该语言支持设置和获取属性值、属性分配、方法调用、访问数组、集合和索引器的内容、逻辑和算术运算、变量命名以及从Spring的IoC容器中以名称检索对象。它还支持列表投影、选择以及常见的列表聚合。|


**2.AOP和Instrumentation**

| 模块 |功能作用  |
|--|--|
| Spring-aop 模块 |  提供了一个符合AOP要求的面向切面的编程实现，允许定义方法拦截器和切入点，将代码按照功能进行分离，以便干净地解耦。|
| Spring-aspects模块 |提供了与AspectJ的集成功能，AspectJ 是一个功能强大且成熟的AOP框架|
|  Spring istrument模块 |提供了类植入(nstrumentatonon) 支持和类加载器的实现，可以在特定的应用服务器中使用。  |

**3.消息**

>spring 4.0以后新增了消息(Spring messaging)模块，该模块提供了对消息传递体系结构和协议的支持

**4数据访问/集成**

> 数据访间集成层由JDBC、ORM. OXM. JMS和事务模块组成。

| 模块 |功能作用  |
|--|--|
| Spring-jdbc模块 |提供了一个 JDBC的抽象层，消除了烦琐的JDBC编码和数据库厂商特有的错误代码解析。  |
| Spring om模块 |  为流行的对象关系映射(Objeet Relational Mapping) API提供集成层，包括JPA和Hibemate.使用Spring orm模块可以将这些O/R映射框架与Spring提供的所有其他功能结合使用，例如声明式事务管理功能。|
| Spring-oxm 模块 | 提供了一个支持对象/XML映射的抽象层实现，例如JAXB、Castor、 JiBX和XStream。 |
|   Spring-jms 模块(Java Messaging Serice)| 指Java消息传递服务，包含用于生产和使用消息的功能。自Sping.1以后，提供了与Spring mesgingg模块的集成。 |
|Sping-tg模块(事务模块)  | 支持用于实现特殊接口和所有POIO (普通Java对象)类的编程和事务式声明管理 |

**5.Web**

> Web层由Spring-web. Spring webmvc、Spring websocket和Portet模块组成。

| 模块 |功能作用  |
|--|--|
|Spring-web模块  |提供了基本的Web开发集成功能， 例如多文件上传功能、使用Servlet监听器初始化一个 loC容器以及Web应用上下文。  |
|Spring-webmvc模块  |  也称为Web-Servlet模块，包含用于Web应用程序的SpringMVC和REST Web Services实现。Spring MVC框架提供了领城模型代码和Web表单之间的清晰分离，并与Spring Framework的所有其他功能集成|
|Spring-websocket 模块  |   Spring 4.0以后新增的模块，它提供了WebSocket和SockJS的实现。|
|  Portler模块|类似于Serlet模块的功能，提供了Porlet环境下的MVC实现。  |

**6.测试**

> Spring-test 模块支持使用JUnit或TestNG对Spring组件进行单元测试和集成测试。



参考文献：[1] 陈恒，楼偶俊，张立杰.Java EE框架整和开发入门到实践[M].清华大学出版社，2018-：.1-7
