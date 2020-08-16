# 第一部分:JVM（总结一概念以及内存分类）
**第二部分：** [JVM（对象生命周期和对象引用以及垃圾收集算法）](https://blog.csdn.net/qq_42009262/article/details/107727539)
> 一.JVM概念
> 二.JVM内存分类
> 
**传统程序语言:由程序员手动内存管理**

 - C/C++，malloc申请内存和ree释放内存

 - 由于程序员疏忽或程序异常，导致内存泄露

**现代程序语言:自动内存管理**

 - Java/C#, 采用内存自动管理
 - 程序员只需要申请使用，系统会检查无用的对象并回收内存-系统统一管理内存，内存使用相对高效

## 一.JVM概念



<p>JVM是Java Virtual Machine（Java虚拟机）的缩写，JVM是一种用于计算设备的规范，它是一个虚构出来的计算机，是通过在实际的计算机上仿真模拟各种计算机功能来实现的。</p>
<p>引入Java语言虚拟机后，Java语言在不同平台上运行时不需要重新编译。Java语言使用Java虚拟机屏蔽了与具体平台相关的信息，使得Java语言编译程序只需生成在Java虚拟机上运行的目标代码（字节码），就可以在多种平台上不加修改地运行。</p>

## 二.JVM内存分类
**JVM内存模型图：**
![内存](https://img-blog.csdnimg.cn/20200731221018492.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyMDA5MjYy,size_16,color_FFFFFF,t_70)                      

## 线程私有内存

**2.1 程序计数器**
<p>  

 - 程序计数器(Program Counter Register)是一块较小的内存空间，它的作用可以看作是当前线程所执行的字节码的行号指示器。
 -  PC存储当前方法（线程正在执行的方法称为该线程的当前方法）。

 - JVM通过改变程序计数器的值来决定下一条需要执行的字节码指令，进而进行选择语句、循环、异常处理、线程恢复等。
 - 当前方法为本地(native)方法时,pc值未定义(undefined)。
 - 当前唯一 一块不会引发OutOfMemoryError异常。。



 </p>

**2.2 Java虚拟机栈**
<p> 

 - 每个方法被执行的时候，Java虚拟机都会同步创建一个栈帧（Stack Frame）用于存储局部变量表、操作数栈、动态连接、方法出口等信息。
 - 每个线程有自己独立的Java虛拟机栈，线程私有
 - Xss设置每个线程堆栈大小
 - 每一个方法被调用到执行完成的过程，就对应着一个栈帧在虚拟机栈中入栈出栈过程。
 -  局部变量表存放了编译期可知的基本数据类型和数据引用类型。

 - **引发的异常**：栈的深度超过虚拟机规定深度，StackOverflowError异常；无法扩展内存，OutOfMemoryError 异常。


  </p>

**2.3 本地方法栈**
<p> 

 - 为虚拟机使用到的本地（Native）方法服务。
 - VM规范没有对本地方法栈做明显规定。

 - **引发的异常**：栈的深度超过虚拟机规定深度，StackOvetflowError异常；无法扩展内存，OutOfMemoryError 异常。


  </p>

## 线程共享内存

**2.4 堆**
<p>   

 - 虚拟机启动时创建,所有线程共享，占比最大。
 - 对象实例和数组都是在堆上分配内存。

 - 垃圾回收的主要区域。

 - 设置大小   -Xms初始堆值，-Xmx最大堆值。
 - 引发的异常：无法满足内存分配要求，OutOfMemoryError异常。



</p>

**2.5 方法区**
<p>  

 - 存储已被虚拟机加载的类型信息、常量（final）、静态变量（static）、即时编译器编译后的代码缓存、运行时常量池等数据，很少做垃圾回收。
 - JVM启动时创建，在《Java虚拟机规范》中把方法区描述为堆的逻辑部分，但是有一个别名叫“非堆”，目的用来区分开来。
 - 引发的异常：无法满足内存分配要求，OutOfMemoryError异常。


 </p>

**2.6 运行时常量池**
<p>   

 - 属于方法区的一部分。
 - 运行时常量池相对于Class文件常量池的一个重要特征—— **动态性** ：Java语言并不要求常量一定只有在编译期；产生比如String.intern方法。


 - 引发的异常：无法满足内存分配要求，OutOfMemoryError异常。


</p>

|名称  | 线程私有/共享 |功能   | 大小参数 | 异常情况 |
|--|--| - |--|--|
| 程序计数器 |  私有| 保存当前线程执行方法，选取下一条需要执行的字节码指令  | 通常固定大小 |无  |
|本地方法栈  |私有 | 为虚拟机使用到的本地（Native）方法服务。  |通常固定大小  |StackOvetflowError异常，OutOfMemoryError异常  |
| JVM栈 | 私有 |   方法的栈帧| -Xss |StackOvetflowError异常，OutOfMemoryError异常  |
|堆  |共享   |  存储实例对象和数组 | -Xms初始堆大小，-Xmx最大堆大小，-Xmn新生代大小，-XX:SurvivorRatio设置eden区/ from(to)的比例，      -XX:NewRatio设置老年代/新生代比例，-XX:+PrintGC/-XX:+PrintGCDetails 打印GC的过程信息 | OutOfMemoryError异常 |
| 方法区 | 共享  | 存储已被虚拟机加载的类型信息、常量（final）、静态变量（static）、即时编译器编译后的代码缓存、运行时常量池等数据  |-XX参数设置 | OutOfMemoryError异常 |
| 运行时常量池 |  共享 |  常量池运行时表示 | 从属于方法区 | OutOfMemoryError异常 |



**参考文献：[1] 周志明.深入了解Java虚拟机[M].机械工业出版社，2020-：.43-47 
，也可以观看陈良育老师的《Java核心技术(高阶)》**

