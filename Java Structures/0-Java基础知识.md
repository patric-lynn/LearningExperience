## Java基础知识汇总

### Java基础语法

​		**新冠肺炎疫情让我们的2020充满挑战。不过一年之计在于春，随着三月春日的来临，各大厂实习生招聘也已经开始啦，是时候翻开新的一页啦，快来一起温习下Java吧！**

​		Java 是一门纯粹的面向对象编程语言，它吸收了C++的各种优点，还努力摒弃了C++里难以理解的多继承、指针等概念，真正地实现了面向对象理论，因而具有功能强大和简单易用两个特征。

​		除了基础语法之外，Java还有许多必须弄懂的特性：**封装、继承、多态**。此外还有泛型、反射等特性，很多框架的技术都依赖它们，比如 Spring 核心的 IOC 和 AOP，都用到了反射。而且 Java 自身的动态代理也是利用反射实现的。

​		此外 Java 还有一些标准库也非常常见，比如集合、I/O、并发，这些类型与功能丰富的库几乎在 Web 开发中无处不在，也是面试经常会被问到的，所以在自学 Java 后端之前，不妨先打好这些基础，另外 Java8 的一些新特性我们也要重点关注，比如 Lambda 表达式、集合和 Stream 流操作、全新的 Date API 等等。



#### Java技术栈概览

##### **1. 语言基本知识**

###### 1.1 Java基本概念与语法

- Java基本语法
      - Java的类加载机制
      - Java虚拟机：JVM内存模型和结构,GC原理,性能调优
      - 泛型
      - 集合体系
      - 异常体系
      - IO体系
      - 多线程编程
      - 反射
      - Servlet

- 推荐书籍

  《Java编程思想》
  《Effective Java》
  《深入理解Java虚拟机》

###### 1.2 Python等脚本语言

- 介绍

  此类脚本语言的意义在于辅助后端开发，包括系统运维，工具集成（借助管道、重定向），文本处理，数据计算等工作，提升后端工作效率

- 核心知识点

  常用的linux操作和运维命令

  Python爬虫

- 推荐书籍

###### 1.3 SQL 语言

- 介绍

  通用于传统关系型数据库和大数据场景的结构化查询模型语言。传统数据库方面要精通mysql的sql语法；大数据场景主要掌握Hive SQL（Impala和Spark SQL等均在兼容Hive SQL）

- 核心知识点

  区分DDL和DML
      - 经典查询语句主要结构
      - 表连接（内连接，左外连接，右外连接，全连接）
      - 函数（普通函数，聚合函数）
      - 索引
      - 分区

- 适用场景

  传统数据库，传统SQL：用于事务性数据操作，通常用于系统的业务库；
  大数据场景下的NoSQL：用于数据分析，交互式查询，批量数据处理等。

- 推荐书籍

  MySQL：《高性能MySQL》
  Hive SQL：《Hive编程指南》


##### **2. 开发工具**

###### 2.1 开发IDE Intellij/Eclipse

- 核心知识点

  [配置](https://link.zhihu.com/?target=http%3A//www.cnblogs.com/zhangyinhua/p/7520823.html)良好代码风格模板，包括文件注释，类注释，方法注释，缩进换行等

      - 安装使用必要的插件：代码规范检查插件(Alibaba Java Coding Guidelines)，源码潜在bug检查插件(pmd）,字节码潜在bug检查插件（findbugs）等


###### 2.2 工程构建 MAVEN

- 核心知识点

  maven 仓库
      - maven pom.xml文件配置
      - maven 构建的生命周期
      - maven 插件

- 能力要求

  学会配置maven多级工程结构
      - 能够解决工程构建中常常出现的jar包冲突等问题
      - 熟练使用maven

- 推荐书籍

  《Maven实战》


###### 2.3 版本管理 GIT

- 介绍

    git是一个开源的分布式版本控制系统，通过规范化的工具和流程，使得开发者之间高效协同，保持产品稳定运行

- 推荐文章

    [Git && Git Flow 使用总结](https://zhuanlan.zhihu.com/p/25597166)


##### **3. 开发/测试框架**

###### 3.1 Spring框架体系 Spring MVC/Spring Boot

- 核心知识点

  Spring框架的模块组成
      - IOC和AOP的设计思想和实现原理
      - Bean的[作用域和生命周期](https://link.zhihu.com/?target=https%3A//www.cnblogs.com/zhanglei93/p/6231882.html)

- 参考说明

  有条件地阅读spring核心模块的代码，debug Spring框架的启动和Bean服务的过程是比较好的理解spring原理的方式

      - 最新的版本更新和参考文档可以直接浏览 [Spring官网](https://link.zhihu.com/?target=https%3A//spring.io/)


###### 3.2 orm框架 mybatis/hibernate

- mybatis介绍

  持久层框架，通过xml映射的方式，使得开发者主要关心sql实现即可，由框架实现从实体到java类的映射，数据访问层服务实现，访问优化等等

- 核心知识点

  熟练配置使用mybatis，掌握mapper xml的写法
      - 会使用mybatis-generator的mvn插件自动生成DAO层代码和xml配置
      - 理解mybatis的[一、二级缓存](https://link.zhihu.com/?target=http%3A//blog.csdn.net/u010643307/article/details/70148723),了解sql session创建过程和sql执行过程

- 推荐资料

  最新的版本更新和参考文档可以直接浏览 [Mybatis官网](https://link.zhihu.com/?target=http%3A//www.mybatis.org/mybatis-3/)


###### 3.3 测试框架 Junit/TestNG

- 核心能力要求

  熟练使用Junit/TestNG进行单元测试，接口测试
      - 学会借助Mockito解决外部系统，模块依赖的mock测试
      - 学会使用maven的surfire plugin和jacoco plugin等插件，度量测例的通过率，测试代码覆盖率等指标


##### **4. 服务器中间件**

###### 4.1 key-value存储系统

- 典型：redis/memcache

- 核心知识点

  redis配置策略（持久化策略，替换算法等）
      - redis的部署模式（standalone单机,sentinel哨兵,cluster集群），哨兵的本质是主从切换，cluster是真正的分布式部署可以线性扩展内存资源
      - redis支持的数据结构

- 适用场景分布式session存储，数据缓存，分布式锁，简单的消息队列，计数器（累加器）


###### 4.2 消息队列

- 典型：rabbitmq/kafka

- 核心知识

  AMQP协议对应消息模型中的主要概念](https://link.zhihu.com/?target=https%3A//www.cnblogs.com/frankyou/p/5283539.html)
      - 两种集群模式对比：[普通集群和镜像集群(HA)](https://link.zhihu.com/?target=https%3A//www.linuxidc.com/Linux/2017-10/147708.htm%3Fhmsr%3Dtoutiao.io)
      - kafka
      - 基于zk的分布式拓扑结构
      - 理解kafka支持的消息的写入流程，写入模式，消息的存储模式，消息的消费方式等
      - rabbitmq

- 适用场景

  消息中间件一般适用于：系统之间需要高度解耦；异步传输数据（非阻塞）；保证数据传输过程中数据可以持久化，解决生产端和消费端能力不一致的问题，避免峰值数据对下游服务节点的冲击等

  对比rabbitmq和kafka: rabbitmq主要适用数据可靠性，一致性要求比较高的场景；kafka主要适用数据吞吐量比较的场景，如日志数据的实时传输等


###### 4.3 分布式文件存储

- 典型：网易nos/阿里云oss

- 适用场景

    存储需要在分布式架构下共享的文件对象：如前端静态文件，图片资源，大文件资源等


###### 4.4 服务治理/RPC框架

- 典型：阿里dubbo

- 核心知识点

    注册中心，服务注册、服务发现、负载均衡、序列化、反序列化、编码、解码等核心概念

- 适用场景

    在相对复杂的应用服务网络中，扮演服务治理，服务解耦，负责均衡，简化服务调用方式的作用


###### 4.5 Java Web容器

- 典型：tomcat/jetty等

- 介绍

    本质上是一个servlet容器，即可以在其上运行Java Web的应用程序


###### 4.6 Nginx

- 介绍

    nginx本质上是一个的HTTP和反向代理服务器

- 适用场景

    http服务器，反向代理，软负载均衡器等


###### 4.7 关系型数据库

- 典型： MYSQL/ORACLE

- 核心知识点

  索引
      - 事务与事务隔离级别
      - 存储方法
      - 触发器
      - 不同关系型数据库的SQL差异

- 推荐书籍

  《高性能MySQL》

  《Oracle从入门到精通》

##### **5. 大数据存储和计算框架等**

- 核心能力要求

- 了解大数据体系整体的框架、组成
      - 熟悉各种大数据存储，计算框架，查询引擎等主要的架构、功能和适用场景
      - 能够在项目的架构过程中进行合理的大数据技术方案选型

##### **6. 安全与日志**

###### 6.1 权限相关

- 介绍：
    权限相关涉及到：身份认证，鉴权，权限管理等

- 核心知识点：

- RBAC（基于角色的权限控制）模型

  理解OAuth 2.0的设计思想和运行流程

###### 6.2 安全相关

- 介绍

- 在后端开发中需要关注很多的安全问题，这些安全问题可能来自恶意的攻击，也可能来自系统本身的安全性和健壮性不足。防止应用安全漏洞产生的前提是理解常见的漏洞概念、原理以及产生的原因

- 核心知识点

- 水平权限和垂直权限漏洞，以及在程序开发中怎样避免
      - SQL注入攻击（mybatis预编译可以避免）
      - 常用的限流策略
      - CSRF 攻击
      - XSS 攻击

###### 6.3 日志相关

- 核心知识点

- Log4j、Logback、SLF4J之间的关系，应用中不可直接使用日志系统（Log4j、Logback）中的API，而应依赖使用日志框架SLF4J中的API
      - 日志的等级（error/warn/info/debug）
      - 日志的保留时间

###### 6.4 异常处理相关

- 核心知识点

- 异常的分类，正确区分异常处理方式
      - 异常处理原则
      - 学会借助spring框架等，将异常转为对用户或者调用方更加友好的错误码和错误信息

###### 6.5 RestFul接口设计相关

- 介绍

    restful在目前是使用较多的接口设计和服务间依赖http接口设计的主要原则，基于良好的约定规范，可以使得接口简单易懂，提高团队协作的效率

- 推荐文章

    RESTful API 设计指南](https://link.zhihu.com/?target=http%3A//www.ruanyifeng.com/blog/2014/05/restful_api.html)


###### 6.6 线上问题排查流程和工具

- 核心知识点

  系统问题排查：学会使用w/uptime、top、df、netstat、iostat以及各种可视化监控工具排查本机的负载、磁盘、内存、网络等系统问题
      - 应用问题排查：学会依据应用系统错误日志以及jps、jinfo、jstat、jstack、jmap以及各种可视化监控工具，排查应用系统问题
      - 数据库问题排查：学会借助mysql慢查询日志、explain以及各种可视化工具排查mysql使用过程中的各种问题
      - 网络问题排查：学会借助telnet、ping、wget、nc等工具排查网络问题


###### 6.7 多线程编程

- 核心知识点

  Java线程相关接口和类的实现和关系
      - Java线程池
      - 线程同步，锁等


###### 6.8 网络相关

- 核心知识点

  掌握http协议消息体划分、连接和通信过程
      - 掌握http主要的返回状态码意义
      - 掌握https协议的连接和通信过程


###### 6.9 设计原则和设计模式

- 核心知识点

  单例模式
      - 策略模式
      - 代理模式
      - 观察者模式
      - 装饰模式
      - 适配器模式
      - 组合模式
      - 工厂模式
      - 模板方法模式
      - ...
      - 单一职责原则
      - 里氏替换原则
      - 依赖倒置原则
      - 接口隔离原则
      - 迪米特法则
      - 开闭原则
      - 组合/聚合复用原则
      - 七大设计原则
      - 一些常用的设计模式


###### 6.10 分布式系统设计相关

- 核心知识点

  Quartz，基于数据库锁的抢占式分布式定时器
      - 基于数据库
      - 基于缓存
      - 基于zookeeper
      - 一致性理论
      - 分布式事务解决方案
      - 一致性模型
      - CAP定理
      - BASE 理论
      - 强一致性
      - 弱一致性
      - 最终一致性
      - eBay 事件队列方案
      - TCC （Try-Confirm-Cancel）补偿模式
      - 缓存数据最终一致性
      - 强一致性（两段提交和三段提交模型, Paxos或者Raft算法）
      - 最终一致性
      - SOA
      - 微服务
      - Servless
      - 分布式系统架构设计
      - 分布式事务
      - 分布式锁
      - 分布式定时器

  


#### 面向对象思想

##### 面向过程和面向对象的区别

###### **面向过程**

-   优点：性能比面向对象好，因为类调用时需要实例化，开销比较大，比较消耗资源。

-   缺点：**不易维护、不易复用、不易扩展**

###### **面向对象**

-   优点：易维护、易复用、易扩展，由于面向对象有**封装、继承、多态**性的特性，可以设计出**低耦合**的系统，使系统更灵活、更易于维护

-   缺点：性能比面向过程差

##### 面向对象的三大特性

###### **封装**

-   隐藏对象的**属性和实现细节**，仅对外提供**公共访问方式**，将变化隔离，便于使用，提高复用性和安全性。


###### **继承**

-   提高代码**复用性**；继承是**多态的前提**。


###### **多态**

-   父类或接口定义的引用变量可以指向**子类或具体实现类的实例对象**。提高了程序的**拓展性**。


##### 面向对象五大原则

- ###### 单一职责原则SRP(Single Responsibility Principle)
  
  类的功能要单一，不能包罗万象，跟杂货铺似的。
      
  
- ###### 开放封闭原则OCP(Open－Close Principle)

    一个模块对于拓展是开放的，对于修改是封闭的，想要**增加功能热烈欢迎**，想要修改，哼，一万个不乐意。
        

- ###### 里式替换原则LSP(the Liskov Substitution Principle LSP)

    子类可以替换父类出现在父类能够出现的**任何地方**。比如你能代表你爸去你姥姥家干活。哈哈~~
        

- ###### 依赖倒置原则DIP(the Dependency Inversion Principle DIP)

    高层次的模块不应该依赖于低层次的模块，他们都应该**依赖于抽象**。抽象不应该依赖于具体实现，具体实现应该依赖于抽象。就是你出国要说你是中国人，而不能说你是哪个村子的。比如说中国人是抽象的，下面有具体的xx省，xx市，xx县。你要依赖的抽象是中国人，而不是你是xx村的。
        

- ###### 接口分离原则ISP(the Interface Segregation Principle ISP)

    设计时采用多个与特定客户类有关的接口比采用一个通用的接口要好。就比如一个手机拥有打电话，看视频，玩游戏等功能，把这几个功能**拆分成不同的接口**，比在一个接口里要好的多。
        
        

#### JVM、JRE和JDK

##### **Java工具包的基本构成**

![image-20200310151807287](/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310151807287.png)

##### **JVM**

​		Java Virtual Machine是Java虚拟机，Java程序需要**运行在虚拟机上**，不同的平台有自己的虚拟机，因此Java语言可以实现跨平台。

##### **JRE**

​		Java Runtime Environment包括**Java虚拟机**和Java程序所需的**核心类库**等。核心类库主要是**java.lang包**：包含了运行Java程序必不可少的系统类，如基本数据类型、基本数学函数、字符串处理、线程、异常处理类等，系统缺省加载这个包

​		如果想要运行一个开发好的Java程序，计算机中**只需要安装JRE即可**。

##### **JDK**

​		Java Development Kit是**提供给Java开发人员使用**的，其中包含了Java的开发工具，也包括了JRE。所以安装了JDK，就**无需再单独安装JRE**了。其中的开发工具：**编译工具**(javac.exe)，**打包工具**(jar.exe)等



#### Java标识符

##### **定义**

​		给**包,类,方法,变量**起名字的符号。

##### **组成规则**

​		标识符由**字母、数字、下划线、美元符号**组成。

##### **命名原则 (需严格遵守)**

###### 1.包名：全部小写，多级包用.隔开。

​		举例：com.jourwon

###### 类、接口：一个单词首字母大写，多个单词每个单词的首字母大写。

​		举例：Student,Car,HelloWorld

###### 3.方法和变量：一个单词首字母小写，多个单词从第二个单词开始每个单词的首字母大写。

​		举例：age,maxAge,show(),getAge()

###### 常量：如果是一个单词,所有字母大写，如果是多个单词,所有的单词大写,用下划线区分每个单词。

​		举例：DATE,MAX_AGE

###### 5.项目名：全部用小写字母，多个单词之间用横杆-分割。

​		举例：demo，spring-boot

##### **注意事项**

-   不能以数字开头
-   不能是Java中的关键字
-   Java标识符大小写敏感，长度无限制
-   标识符不能包含空格



#### Java关键字

##### **Java关键字/保留字(Java 8版本)**

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310151918898.png" alt="image-20200310151918898" style="zoom:55%;" />

##### **Java8以后新增关键字**

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200405110940779.png" alt="image-20200405110940779" style="zoom:50%;" />



#### Java注释

##### 单行注释

​		格式： // 注释文字

##### 多行注释

​		格式： /* 注释文字 */

##### 文档注释

​		格式：/** 注释文字 */



#### Java分隔符

##### **定义**

​		空格、逗号、分号以及行结束符都被称为**分隔符**，规定任意两个相邻标识符、数字、保留字或语句之间必须至少有一个分隔符，以便程序编译时能够识别。

##### **分类**

**,**     逗号在变量声明中，区分变量说明的各个变量。在f·or控制语句中，用来将圆括号内的语句连接起来

**;** 	分号用来**终止一个语句**
**{}**   花括号、大括号 用来包括**自动初始化的数组的值，**也用来**定义程序块、类、方法以及局部范围**
**[]**    方括号、中括号 用来**声明数组的类型**，也用来表示撤消对数组值的引用
**.**     原点 用来将软件包的名字与它的子包或类分隔。也用来将**引用变量与变量或方法分隔**
**()**    圆括号 在定义和调用方法时用来**容纳参数表**。在控制语句或强制类型转换组成的表达式中用来**表示执行或计算的优先权**

##### **注意事项**

​		必须都是**半角下的英文符号**。




#### Java转义字符

##### **定义**

​		转义字符是一种特殊的字符常量。转义字符以**反斜线"\\"开头**，后跟一个或几个字符。转义字符具有特定的含义，不同于字符原有的意义，故称“转义”字符。

##### **常见转义字符表**

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310152141800.png" alt="image-20200310152141800" style="zoom:40%;" />

##### **Java中需要转义的字符**

​		在Java中，不管是String.split()，还是正则表达式，有一些特殊字符需要转义，这些字符是( [ { / ^ - $ ¦ } ] ) ? * + .

​		转义方法为字符前面加上"\\"，这样在split、replaceAll时就不会报错。不过要注意，**String.contains()方法不需要转义**。



#### Java流程控制语句

##### **顺序结构**

​		顺序结构是程序中最简单最基本的流程控制，没有特定的语法结构，按照代码的先后顺序，依次执行，程序中大多数的代码都是这样执行的

##### **分支结构-if**

```Java
if(条件表达式){
执行的代码块；
}

if(条件表达式){
执行的代码块;
}else{
执行的代码块；
}

if(条件表达式){
执行的代码块;
}else if(条件表达式){
执行的代码块;
}…(此处省略N个else if)
else{
执行代码块;
}
```

##### **分支结构-switch**

-   首先，计算出表达式的值
-   其次，**和case依次比较**，一旦有对应的值，就会执行相应的语句，在执行的过程中，遇到break就会结束。
-   最后，如果所有的case都和表达式的值不匹配，**就会执行default语句体部分**，然后程序结束掉。

##### **循环结构-for**

```Java
for(初始化语句;判断条件语句;控制条件语句) {
循环体语句;
}
```

##### **循环结构-foreach（只可访问不可修改删除，基于迭代器）**

```Java
for (声明语句 : 表达式) {
　 //代码句子
}
```

```Java
public class Test {
   public static void main(String args[]) {
        int[] numbers = {10, 20, 30, 40, 50};
   			for ( int x : numbers ) {
        	System.out.print( x );
        	System.out.print(",");
    		}
    	 System.out.print();
    	 String[] names = {"James", "Larry", "Tom", "Lacy"};
    	 for ( String name : names ) {
          System.out.print( name );
          System.out.print(",");
  			}
		}
}
```

##### **循环语句-while**

```Java
基本格式
while(判断条件语句) {
循环体语句;
}

扩展格式
初始化语句;
while(判断条件语句) {
循环体语句;
控制条件语句;
}
```

##### **循环语句-do…while**

```Java
基本格式
do {
循环体语句;
}while((判断条件语句);

扩展格式
初始化语句;
do {
循环体语句;
控制条件语句;
} while((判断条件语句);
```



##### **跳转语句(控制循环结构)**

​		Java语言中提供了3种跳转语句，**分别是break语句、continue语句和return语句**。

-   break的作用：跳出**单层循环**。注意**带标签的break方法**。

-   continue的作用：**结束一次循环**，继续**下一次的循环**

-   return的作用：可以从一个方法返回，并把控制权**交给调用它的语句**。直接结束整个方法，从而结束循环。




#### Java运算符

##### 算术运算符

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310152315099.png" alt="image-20200310152315099" style="zoom:50%;" />

注意事项

- 1、/ 左右两端的类型需要一致；
- 2、%最后的符号和被模数相同；
- 3、前++；先+1，后运算 ，后++；先运算，后+1；
- 4、+：**当String字符串与其他数据类型只能做连接运算；并且结果为String类型；**

##### 比较运算符（关系运算符）

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310152332471.png" alt="image-20200310152332471" style="zoom:50%;" />

- 注意事项
    - 1、比较运算符的**两端都是boolean类型**，也就是说要么是true，要么是false；
    - 2、比较运算符的"==“与”="的作用是不同的，使用的时候需要小心。

##### 赋值运算符

- 基本的赋值运算符：=
- 扩展的赋值运算符：+=,-=,*=,/=,%=
    +=:	a+=20;相当于a = (a的数据类型)(a + 20);

##### 逻辑运算符（符号的两端都是boolean类型）

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310152348297.png" alt="image-20200310152348297" style="zoom:50%;" />

注意事项

1、& 与 &&以及|与||的区别：
&：左边无论真假，**右边都会进行运算**；
&&：如果左边为**假**，**则右边不进行运算**；
| 与 || 的区别同上；在使用的时候建议使用&&和||；
2、（^）与或（|）的不同之处是：当左右都为true时，结果为false。

##### 位运算符（两端都是数值型的数据）

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310152412191.png" alt="image-20200310152412191" style="zoom:50%;" />

##### 三元运算符（三目运算符）

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310152445704.png" alt="image-20200310152445704" style="zoom:50%;" />

- 注意事项
    - 1、表达式1与表达式2的**类型必须一致**；
    - 2、使用三元运算符的地方**一定可以使用if…else代替，反之不一定成立**；

##### 运算符优先级

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310152502026.png" alt="image-20200310152502026" style="zoom:50%;" />



#### Java常量

##### 定义

​		**常量定义**：在程序执行的过程中，其**值不可以发生改变的量**。常量不同于常量值，它可以在程序中用符号来代替常量值使用，因此在使用前必须先定义。	

​		**常量值定义**：常量和常量值是不同的概念，常量值又称为**字面常量**，它是通过**数据直接表示**的。		

​		**两者关系**：常量值是常量的具体和直观的表现形式，常量是形式化的表现。通常在程序中既可以直接使用常量值，也可以使用常量。

##### 分类

- 字符串常量	用双引号括起来的内容(“HelloWorld”)
- 整数常量	所有整数(12,-23)
- 小数常量	所有小数(12.34)
- 字符常量	用单引号括起来的内容(‘a’,’A’,’0’)
- 布尔常量	较为特有，只有true和false
- 空常量	null(数组部分讲解)

##### 举例

-   Java 语言使用 final 关键字来定义一个常量
-   final int COUNT=10;
-   final float HEIGHT=10.2F;

##### 注意事项

- 在定义常量时就需要对该常量**进行初始化**。
- final 关键字不仅可以用来修饰基本数据类型的常量，还可以用来修饰**对象的引用或者方法**。
- 为了与变量区别，常量取名一般都用**大写字符**。



#### Java变量

**一.按被声明的位置划分**

##### 1.成员变量

​		方法外部，类内部定义的变量

###### 1.类变量(静态变量)：独立于方法之外的变量，用 static 修饰。

1. 类变量也称为静态变量，在类中以**static关键字**声明，但必须在方法构造方法和语句块之外。无论一个类创建了多少个对象，类**只拥有类变量的一份拷贝**。

2. 静态变量除了被声明为常量外**很少使用**。常量是指声明为public/private，final和static类型的变量。常量初始化后不可改变。

3. 静态变量储存在**静态存储区**。经常被声明为常量，**很少单独使用static声明变量**。

4. 静态变量在**第一次**被访问时创建，在**程序结束时销毁**。

5. 与实例变量具有相似的可见性。但为了对类的使用者可见，大多数静态变量**声明为public类型**。

6. 默认值和实例变量相似。数值型变量默认值是0，布尔型默认值是false，引用类型默认值是null。变量的值可以在声明的时候指定，也可以在构造方法中指定。此外，静态变量**还可以在静态语句块中初始化**。

7. 静态变量可以通过：**ClassName.VariableName的方式访问**。（反射）

8. 类变量被声明为public static final类型时，**类变量名称一般建议使用大写字母**。如果静态变量不是public和final类型，其命名方式与实例变量以及局部变量的命名方式一致。

    

###### 2.实例变量(非静态变量)：独立于方法之外的变量，不过没有 static 修饰。

1. 实例变量**声明在一个类中**，但在方法、构造方法和语句块之外；

2. 当一个对象被实例化之后，每个实例变量的值就跟着确定；

3. 实例变量在对象创建的时候创建，在**对象被销毁的时候销毁**；

4. 实例变量的值**应该至少被一个方法、构造方法或者语句块引用**，使得外部能够通过这些方式获取实例变量信息；

5. 实例变量可以**声明在使用前或者使用后**；

6. 访问修饰符可以修饰实例变量；

7. 实例变量对于类中的方法、构造方法或者语句块是**可见**的。一般情况下应该把**实例变量设为私有**。通过使用访问修饰符可以使实例变量对子类可见；

8. 实例变量**具有默认值**。数值型变量的默认值是0，布尔型变量的默认值是false，引用类型变量的默认值是null。变量的值可以在声明时指定，也可以在构造方法中指定；

9. 实例变量可以直接通过**变量名访问**。但在静态方法以及其他类中，就应该使用完全限定名:ObejectReference.VariableName。

    

###### **3.类变量和实例变量的区别**

**调用方式**

​		静态变量也称为**类变量**，可以直接通过**类名调用**。也可以通过对象名调用。这个变量属于类。

​		成员变量也称为**实例变量**，只能通过对象名调用。这个变量属于对象。

**存储位置**

​		静态变量存储在方法区中的**静态区**。			

​		成员变量存储在**堆内存**。

**生命周期**

​		静态变量随着类的加载而存在，随着类的消失而消失。生命周期长。

​		成员变量随着对象的创建而存在，随着对象的消失而消失。



###### 4.与对象的相关性

​		静态变量是所有对象共享的数据。			

​		成员变量是每个对象所特有的数据。



##### 2.局部变量

-   局部变量：类的**方法中的变量**。访问修饰符**不能用于局部变量**；

-   局部变量声明在**方法、构造方法**或者**语句块**中；

-   局部变量**在方法、构造方法、或者语句块被执行的时候创建**，当它们执行完成后，变量将会被销毁；

-   局部变量**只在声明它的方法、构造方法或者语句块中可见**；

-   局部变量是**在栈上分配**的。

-   局部变量没有默认值，所以局部变量被声明后，**必须经过初始化**，才可以使用。




##### 3.成员变量和局部变量的区别

###### 作用域

-   成员变量：针对**整个类有效**。
-   局部变量：只在**某个范围内有效**。(一般指的就是方法,语句体内)

###### 存储位置

-   成员变量：随着对象的创建而存在，随着对象的消失而消失，存储在**堆内存中**。

-   局部变量：在方法被调用，或者语句被执行的时候存在，**存储在栈内存中**。当方法调用完，或者语句结束后，就自动释放。

###### 生命周期

-   成员变量：**随着对象**的创建而存在，随着对象的消失而消失
-   局部变量：当方法调用完，或者**语句结束后，就自动释放**。

###### 初始值

-   成员变量：**有**默认初始值。
-   局部变量：**没有**默认初始值，使用前必须赋值。

###### 使用原则

-   在使用变量时需要遵循的原则为：**就近原则**
-   首先在局部范围找，有就使用；接着在成员位置找。



**2.按所属的数据类型划分**

##### 基本数据类型变量

###### **基本数据类型**

-   byte,short,int,long,float,double,char,boolean  12484821


###### 变量的定义格式

-   数据类型 变量名 = 初始化值;


###### 注意

-   整数默认是int类型，定义long类型的数据时，要在数据后面加L。
-   浮点数默认是double类型，定义float类型的数据时，要在数据后面加F。



##### 引用数据类型变量

###### 定义格式

-   数据类型 变量名 = 初始化值;
-   注意：格式是固定的，记住格式，以不变应万变

​       举例

```Java
public class Variable{
    // 类变量
    static int allClicks=0;    
     // 实例变量
    String str="hello world";  
    public void method(){
		 	// 局部变量
        int i =0;  
    }
}
```

###### 注意事项：类外面不能有变量的声明



#### Java数据类型

##### 定义

- Java语言是**强类型语言**，对于每一种数据都定义了明确的具体的数据类型，在内存中分配了**不同大小的内存空间**。

##### 分类

![image-20200310152544126](/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310152544126.png)

- 基本数据类型
    - 数值型
        - 整数类型(byte,short,int,long)
        - 浮点类型(float,double)
    - 字符型(char)
    - 布尔型(boolean)
- 引用数据类型
    - 类(class)
    - 接口(interface)
    - 数组([])

##### **计算机存储单元**

​		定义：变量是内存中的**小容器**，用来存储数据。那么计算机内存是怎么存储数据的呢？无论是内存还是硬盘，计算机存储设备的最小信息单元叫“位（bit）”，我们又称之为“比特位”，通常用小写的字母b表示。而计算机最小的存储单元叫“字节（byte）”，通常用大写字母B表示，字节是由连续的8个位组成。

##### 数据类型转换

###### 1.定义

​		数据类型的转换是在所赋值的数值类型和被变量接收的数据类型不一致时发生的，它需要从一种数据类型转换成另一种数据类型。

###### 2.隐式转换

​		在运算过程中，由于不同的数据类型会转换成同一种数据类型，所以整型、浮点型以及字符型都可以参与混合运算。自动转换的规则是从低级类型数据转换成**高级类型数据**。

​		**转换规则**

​		数值型数据的转换：byte→short→int→long→float→double。

​		字符型转换为整型：char→int。

​		**转换条件**

​		自动类型转换的实现需要同时满足两个条件：①两种数据类型彼此兼容，②目标类型的取值范围大于源数据类型（低级类型数据转换成高级类型数据）。例如 byte 类型向 short 类型转换时，由于 short 类型的取值范围较大，会自动将 byte 转换为 short 类型。

###### 3.显式转换

​		当两种数据类型不兼容，或目标类型的取值范围小于源类型时，自动转换将无法进行，这时就需要进行强制类型转换。

​		**语法格式**

​		目标类型 变量名 = (目标类型) (被转换的数据);

​		举例：int b = (int) (a + b);

​		**注意**

​		如果超出了被赋值的数据类型的取值范围得到的结果会与你期望的结果不同不建议强制转换，因为会有精度的损失。



#### Java反射

##### 定义

​		Java**反射机制**是在运行状态中，对于任意一个类，都能够知道这个类的**所有属性和方法**；对于任意一个对象，都能够调用它的任意方法和属性。这种动态获取信息以及动态调用对象方法的功能称为Java语言的**反射机制**。

##### 用途

​		通过反射，Java 代码可以发现有关已加载类的字段，方法和构造函数的信息，并可以在安全限制内对这些字段，方法和构造函数进行操作。

​		很多人都认为反射在实际Java中开发应用中并不广泛，其实不然。当我们在使用 IDE（如 IDEA/Eclipse）时，当我们输入一个对象或者类并调用它的属性和方法时，一按 (“.”)点号，编译器就会自动列出她的属性或方法，这里就会用到反射。**反射最重要的用途就是开发各种通用框架。**很多框架（比如 Spring）都是**配置化的**（比如Spring 通过 XML 配置模式装载 Bean），为了保证框架的通用性，他们可能根据配置文件加载不同的对象或类，调用不同的方法，这个时候就必须用到反射——**运行时动态加载需要加载的对象**。对于框架开发人员来说，反射作用非常大，它是各种**容器实现的核心**。而对于一般的开发者来说，不深入框架开发反射用的就会少一点，不过了解一下框架的底层机制有助于丰富自己的编程思想，也是很有益的。Java反射框架提供以下功能：

- 在运行时判定任意一个**对象所属的类**
- 在运行时构造任意一个**类的对象**
- 在运行时判定任意一个类所具有的**成员变量和方法**
- 在运行时调用任意一个**对象的方法**

##### 反射的优缺点

###### 反射的优点

​		使用反射机制，代码可以在运行时装配，提高了程序的灵活性和扩展性，**降低耦合性**，提高自适应能力。它允许程序创建和控制任何类的对象，无需硬编码目标类

###### 反射的缺点

**性能问题**：使用反射基本上是一种**解释操作**，JVM无法对这些代码进行优化，因此，反射操作的效率要比那些非反射操作低得多。反射机制主要应用在对**灵活性和扩展性要求很高**的系统框架上，对性能要求高的程序中不建议使用

**安全限制**：使用反射技术要求程序必须在一个**没有安全限制**的环境中运行

**内部暴露**：由于反射允许代码执行一些在正常情况下**不被允许的操作**（比如访问私有的属性和方法），所以使用反射可能会导致意料之外的副作用——代码有功能上的错误，降低可移植性。反射代码破坏了抽象性，因此当平台发生改变的时候，代码的行为就有可能也随着变化。

##### 反射机制的相关类

​		与Java反射相关的类如下：

**类名					  用途**
Class类				 代表类的实体，在运行的Java应用程序中表示类和接口

Field类				  代表类的成员变量（成员变量也称为类的属性）

Method类			代表类的方法

Constructor类	 代表类的构造方法

###### 1.Class类

Class代表**类的实体**，在运行的Java应用程序中表示**类和接口**。在这个类中提供了很多有用的方法，这里对他们简单的分类介绍。

- **获得类相关的方法**
    方法	用途
    asSubclass(Class clazz)	把传递的类的对象转换成代表其子类的对象
    Cast	把对象转换成代表类或是接口的对象
    getClassLoader()	获得类的加载器
    getClasses()	返回一个数组，数组中包含该类中所有公共类和接口类的对象
    getDeclaredClasses()	返回一个数组，数组中包含该类中所有类和接口类的对象
    forName(String className)	根据类名返回类的对象
    getName()	获得类的完整路径名字
    newInstance()	创建类的实例
    getPackage()	获得类的包
    getSimpleName()	获得类的名字
    getSuperclass()	获得当前类继承的父类的名字
    getInterfaces()	获得当前类实现的类或是接口
- **获得类中属性相关的方法**
    方法	用途
    getField(String name)	获得某个公有的属性对象
    getFields()	获得所有公有的属性对象
    getDeclaredField(String name)	获得某个属性对象
    getDeclaredFields()	获得所有属性对象
- **获得类中注解相关的方法**
    方法	用途
    getAnnotation(Class annotationClass)	返回该类中与参数类型匹配的公有注解对象
    getAnnotations()	返回该类所有的公有注解对象
    getDeclaredAnnotation(Class annotationClass)	返回该类中与参数类型匹配的所有注解对象
    getDeclaredAnnotations()	返回该类所有的注解对象
- **获得类中构造器相关的方法**
    方法	用途
    getConstructor(Class…<?> parameterTypes)	获得该类中与参数类型匹配的公有构造方法
    getConstructors()	获得该类的所有公有构造方法
    getDeclaredConstructor(Class…<?> parameterTypes)	获得该类中与参数类型匹配的构造方法
    getDeclaredConstructors()	获得该类所有构造方法
- **获得类中方法相关的方法**
    方法	用途
    getMethod(String name, Class…<?> parameterTypes)	获得该类某个公有的方法
    getMethods()	获得该类所有公有的方法
    getDeclaredMethod(String name, Class…<?> parameterTypes)	获得该类某个方法
    getDeclaredMethods()	获得该类所有方法
- **类中其他重要的方法**
    方法	用途
    isAnnotation()	如果是注解类型则返回true
    isAnnotationPresent(Class<? extends Annotation> annotationClass)	如果是指定类型注解类型则返回true
    isAnonymousClass()	如果是匿名类则返回true
    isArray()	如果是一个数组类则返回true
    isEnum()	如果是枚举类则返回true
    isInstance(Object obj)	如果obj是该类的实例则返回true
    isInterface()	如果是接口类则返回true
    isLocalClass()	如果是局部类则返回true
    isMemberClass()	如果是内部类则返回true

###### 2.Field类

- **Field代表类的成员变量**（成员变量也称为类的属性）。

    方法	用途
    equals(Object obj)	属性与obj相等则返回true
    get(Object obj)	获得obj中对应的属性值
    set(Object obj, Object value)	设置obj中对应属性值

###### 3.Method类

- **代表类的方法**。

    方法	用途
    invoke(Object obj, Object… args)	传递object对象及参数调用该对象对应的方法

###### 4.Constructor类

- **Constructor代表类的构造方法**。

    方法	用途
    newInstance(Object… initargs)	根据传递的参数创建类的对象

**通过反射获取后三者，即私有属性，方法和构造方法时，需要进行暴力反射，设置setAccessible(true)**。否则会报错说无法获取私有属性，方法和构造方法



#### Java注解

##### 为什么要引入注解？

​		使用【注解】之前(甚至在使用之后)，【XML】被广泛的应用于**描述元数据**，得到各大框架的青睐，它以**松耦合**的方式完成了框架中几乎所有的配置，但是随着项目越来越庞大，【XML】的内容也越来越复杂，一些应用开发人员和架构师发现维护成本变高。他们希望使用一些**和代码紧耦合的东西**，于是就有人提出来一种**标记式高耦合的配置方式【注解】**。方法上可以进行注解，类上也可以注解，字段属性上也可以注解，反正几乎需要配置的地方都可以进行注解。下面我们通过一个例子来理解这两者的区别。

​		假如你想为应用设置很多的常量或参数，这种情况下，【XML】是一个很好的选择，因为它不会同特定的代码耦合。如果你想把**某个方法声明为服务**，那么使用【注解】会更好一些，因为这种情况下需要注解和方法紧密耦合起来，开发人员也必须认识到这点。同时，【注解】定义了一种标准的描述元数据的方式。

​		关于【注解】和【XML】两种不同的配置模式，争论了好多年，各有各的优劣，注解可以提供更大的便捷性，易于维护修改，但耦合度高，而 【XML】 相对于注解则是相反的。追求低耦合就要抛弃高效率，追求效率必然会遇到耦合。目前，许多框架将【XML】和【注解】两种方式结合使用，平衡两者之间的利弊。

##### 什么是注解

​		注解也叫**元数据**，即一种描述数据的数据。例如我们常见的**@Override**和**@Deprecated**，注解是JDK1.5版本开始引入的一个特性，用于对代码进行说明，可以对包、类、接口、字段、方法参数、局部变量等进行注解。Annotation接口中有下面这句话来描述注解：**Annotation 是所有注解继承的公共接口**

​		The common interface extended by all annotation types.注解的本质就是一个继承了 Annotation 接口的接口。有关这一点，你可以去反编译任意一个注解类得到结果。**一个注解准确意义上来说，只不过是一种特殊的注释而已，如果没有解析它的代码，它可能连注释都不如。**而解析一个类或者方法的注解往往有两种形式，一种是**编译期直接的扫描**，一种是**运行期反射**。反射的事情我们先不讨论，而编译器的扫描指的是编译器在对 Java 代码编译成字节码的过程中会检测到某个类或者方法被一些注解修饰，这时它就会对于这些注解进行某些处理。

```java
@Override
public String toString() {
    return "Hello Annotation";
}
```


​		上面的代码中，我重写了toString()方法并使用了@Override注解。但是，即使我不使用@Override注解标记代码，程序也能够正常执行。那么，该注解表示什么？这么写有什么好处吗？事实上，**@Override告诉编译器这个方法是一个重写方法(描述方法的元数据)，如果父类中不存在该方法，编译器便会报错，提示该方法没有重写父类中的方法**。如果我不小心拼写错误，例如将toString()写成了toStrring()，而且我也没有使用@Override注解，那程序依然能编译运行。但运行结果会和我期望的大不相同。现在我们了解了什么是注解，并且使用注解有助于提高代码的可读性。

##### 注解的用途

- **生成文档**，通过代码里标识的元数据生成javadoc文档。
- **编译检查**，通过代码里标识的元数据让编译器在编译期间进行检查验证。
- **编译时动态处理**，编译时通过代码里标识的元数据动态处理，例如动态生成代码。
- **运行时动态处理**，运行时通过代码里标识的元数据动态处理，例如使用反射注入实例

##### 注解的分类

- Java自带的**标准注解**，包括@Override（标明重写某个方法）、@Deprecated（标明某个类或方法过时）和@SuppressWarnings（标明要忽略的警告），使用这些注解后编译器就会进行检查

- 自定义注解，可以根据自己的需求定义注解

- 元注解，元注解是用于**定义注解的注解**，包括**@Retention**（标明注解被保留的阶段）、**@Target**（标明注解使用的范围）、**@Inherited**（标明注解可继承）、**@Documented**（标明是否生成javadoc文档）、**@Repeatable**.

    **元注解具体介绍**

    ​		要想真正掌握怎么使用注解，还需要先学习一下元注解。

    ​		元注解是用于修饰注解的注解

    ​		元注解有 @Retention、@Documented、@Target、@Inherited、@Repeatable 5 种。

    ###### 1.@Retention

    Retention 的英文意为**保留期**的意思。当 @Retention 应用到一个注解上的时候，它解释说明了这个注解的的存活时间。它的取值如下：

    - RetentionPolicy.SOURCE 注解**只在源码阶段保留**，在编译器进行编译时它将被丢弃忽视。
    - RetentionPolicy.CLASS 注解只被**保留到编译进行的时候**，它并不会被加载到 JVM 中。如Java内置注解，@Override、@Deprecated、@SuppressWarnning等
    - RetentionPolicy.RUNTIME 注解可以**保留到程序运行的时候**，它会被加载进入到 JVM 中，所以在程序运行时可以获取到它们。如SpringMvc中的@Controller、@Autowired、@RequestMapping等。

    ###### 2.@Documented

    ​		顾名思义，这个元注解肯定是和文档有关。它的作用是能够将注解中的元素包含到 Javadoc 中去。

    ###### 3.@Target

    ​		Target 是目标的意思，@Target 指定了注解运用的地方。

    ​		你可以这样理解，当一个注解被 @Target 注解时，这个注解就被限定了运用的场景。类比到标签，原本标签是你想张贴到哪个地方就到哪个地方，但是因为 @Target 的存在，它张贴的地方就非常具体了，比如只能张贴到方法上、类上、方法参数上等等。@Target 有下面的取值

    - ElementType.ANNOTATION_TYPE 可以给一个注解进行注解
    - ElementType.CONSTRUCTOR 可以给构造方法进行注解
    - ElementType.FIELD 可以给属性进行注解
    - ElementType.LOCAL_VARIABLE 可以给局部变量进行注解
    - ElementType.METHOD 可以给方法进行注解
    - ElementType.PACKAGE 可以给一个包进行注解
    - ElementType.PARAMETER 可以给一个方法内的参数进行注解
    - ElementType.TYPE 可以给一个类型进行注解，比如类、接口、枚举

    ###### 4.@Inherited

    ​		Inherited 是继承的意思，但是它并不是说注解本身可以继承，而是说如果一个超类使用了@Inherited 注解，那么如果它的子类没有被任何注解应用的话，那么这个子类就继承了超类的注解。

    ###### @Repeatable

    ​		Repeatable 自然是可重复的意思。@Repeatable 是 Java 1.8 才加进来的，所以算是一个新的特性。

    ​		Repeatable使用场景：在需要对同一种注解多次使用时，往往需要借助@Repeatable。

##### 注解的属性

​		注解的属性也叫做**成员变量**。注解只有成员变量，**没有方法**。注解的成员变量在注解的定义中以“无形参的方法”形式来声明，其方法名定义了该成员变量的名字，其返回值定义了该成员变量的类型。

```Java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface TestAnnotation{
int id();
String msg();
}
```

​		上面代码定义了 @TestAnnotation 这个注解中拥有 id 和 msg 两个属性。在使用的时候，我们应该给它们进行赋值。赋值的方式是在注解的括号内以 value="" 形式，多个属性之前用 ，隔开。

```Java
@TestAnnotation(id=3, msg="hello annotation")
public class Test {
}
```

##### 快捷方式

​		所谓的快捷方式就是注解中定义了**名为value的元素**，并且在使用该注解时，如果该元素是唯一需要赋值的一个元素，那么此时无需使用key=value的语法，而只需在括号内给出value元素所需的值即可。这可以应用于任何合法类型的元素，记住，这限制了元素名必须为value，简单案例如下

```Java
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@interface IntegerVaule{
   int value() default 0;
   String name() default "";
}

public class QuicklyWay {

   @IntegerVaule(20)
   public int age;

   @IntegerVaule(value = 10000, name = "MONEY")
   public int money;

}
```

##### 注解不支持继承

​		注解是不支持继承的，因此不能使用关键字extends来继承某个@interface，但注解在编译后，编译器会自动继承java.lang.annotation.Annotation接口

##### 声明注解

​		这里总共定义了4个注解来演示注解的声明

1. 定义一个可以注解在Class，interface，enum上的注解
2. 定义一个可以注解在METHOD上的注解
3. 定义一个可以注解在FIELD上的注解
4. 定义一个可以注解在PARAMETER上的注解

##### Java预置的注解

​		学习了上面相关的知识，我们已经可以自己定义一个注解了。其实 Java 语言本身已经提供了几个现成的注解。

@Deprecated

这个元素是用来标记过时的元素，想必大家在日常开发中经常碰到。编译器在编译阶段遇到这个注解时会发出提醒警告，告诉开发者正在调用一个过时的元素比如**过时的方法、过时的类、过时的成员变量**。

@Override

这个大家应该很熟悉了，用于标明**此方法覆盖了父类的方法**。

@SuppressWarnings

用于有选择的**关闭编译器对类、方法、成员变量、变量初始化的警告**。之前说过调用被 @Deprecated 注解的方法后，编译器会警告提醒，而有时候开发者会忽略这种警告，他们可以在调用的地方通过 @SuppressWarnings 达到目的。

@SafeVarargs

**参数安全类型注解**。它的目的是提醒开发者不要用参数做一些不安全的操作，它的存在会阻止编译器产生 unchecked 这样的警告。它是在 Java 1.7 的版本中加入的。

@FunctionalInterface

**函数式接口注解**，这个是 Java 1.8 版本引入的新特性。函数式编程很火，所以 Java 8 也及时添加了这个特性。函数式接口 (Functional Interface) 就是一个具有一个方法的普通接口。我们进行线程开发中常用的 Runnable 就是一个典型的函数式接口，从源码可以看到它使用了@FunctionalInterface 注解。




#### Java语法糖

​		语法糖（Syntactic Sugar），也称糖衣语法，是由英国计算机学家 **Peter.J.Landin（彼得·兰丁）** 发明的一个术语，指在计算机语言中添加的某种语法，这种语法对语言的功能并没有影响，但是更方便程序员使用。**语法糖就是对现有语法的一个封装**。简而言之，**语法糖让程序更加简洁，有更高的可读性**。我们所熟知的编程语言中几乎都有语法糖。很多人说Java是一个“低糖语言”，其实从Java 7开始，Java语言层面上一直在添加各种糖，主要是在**Project Coin**项目下研发。尽管如此有人还是认为现在的Java是低糖，未来还会持续向着**高糖**的方向发展。

​		Java 中最常用的语法糖主要有**switch语句支持String与枚举**、**泛型和类型擦除**、**自动装箱与拆箱**、**方法边长参数**、**枚举**、**内部类**、**条件编译**、**断言**、**数值字面量**、**增强for循环**、**try-with-resources语句**、**Lambda表达式**等。接下来我们分析一下这些语法糖背后的原理。一步一步剥去糖衣，看看其本质。

​		前面提到过，语法糖的存在主要是方便开发人员使用。但是，**Java虚拟机并不支持这些语法糖**。**这些语法糖在编译阶段就会被还原成简单的基础语法结构，这个过程就是解语法糖**。Java中的语法糖**只存在于编译期**，在编译器将 .java 源文件编译成 .class 字节码时，如果你去看com.sun.tools.javac.main.JavaCompiler的源码，你会发现在compile()中有一个步骤就是**调用desugar()**，这个方法就是负责解语法糖而实现的。



##### 糖块一、switch支持String与枚举

从Java 7 开始，Java语言中的语法糖在逐渐丰富，其中一个比较重要的就是Java 7中switch语句开始支持String。

在开始coding之前先科普下，Java中的swith自身**原本就支持基本类型**。比如int、char等。

对于int类型，直接进行数值的比较。对于char类型则是比较其ascii码。

所以，对于编译器来说，switch中其实只能使用整型，任何类型的比较都要转换成整型。比如byte。short，char(ackii码是整型)以及int。那么接下来看下switch对String的支持，有以下代码：

```java
public class SwitchDemoString {
    public static void main(String[] args) {
        String str = "world";
        switch (str) {
        case "hello":
            System.out.println("hello");
            break;
        case "world":
            System.out.println("world");
            break;
        default:
            break;
        }
    }
}
```

​		反编译后内容如下：

```java
public class SwitchStringDemo {

    public static void main(String[] args) {
        String str;
        String string = str = "world";
        int n = -1;
        switch (string.hashCode()) {
            case 99162322: {
                if (!string.equals("hello")) break;
                n = 0;
                break;
            }
            case 113318802: {
                if (!string.equals("world")) break;
                n = 1;
            }
        }
        switch (n) {
            case 0: {
                System.out.println("hello");
                break;
            }
            case 1: {
                System.out.println("world");
                break;
            }
        }
    }

}
```

​		看到这个代码，你知道原来字符串的switch是**通过equals()和hashCode()方法来实现的**。还好hashCode()方法返回的是int，而不是long。仔细看下可以发现，进行switch比较的**实际是哈希值**，然后通过使用equals方法比较进行安全检查，这个检查是必要的，因为**哈希可能会发生碰撞**。因此它的性能是不如使用枚举进行switch或者使用纯整数常量，但这也不是很差。



##### 糖块二、泛型和类型擦除

我们都知道，很多语言都是支持泛型的，但是很多人不知道的是，不同的编译器对于泛型的处理方式是不同的。

通常情况下，一个编译器处理泛型有两种方式：Code specialization和Code sharing。

C++和C#是使用Code specialization的处理机制，而Java使用的是Code sharing的机制。

Code sharing方式**为每个泛型类型创建唯一的字节码表示**，并且将该泛型类型的**实例都映射到这个唯一的字节码表示上**。将多种泛型类形实例映射到唯一的字节码表示**是通过类型擦除（type erasue）实现的**。

也就是说，对于Java虚拟机来说，他根本不认识Map<String, String> map这样的语法。需要在编译阶段通过类型擦除的方式进行解语法糖。

类型擦除的主要过程如下：

- 将所有的泛型参数**用其最左边界（最顶级的父类型）类型替换**。
- **移除所有的类型参数**。

以下代码：

```java
Map<String, String> map = new HashMap<String, String>();  
map.put("name", "JourWon");  
map.put("wechat", "JourWon");  
map.put("blog", "https://blog.csdn.net/ThinkWon");  
```

解语法糖之后会变成：

```java
Map map = new HashMap();  
map.put("name", "JourWon");  
map.put("wechat", "JourWon");  
map.put("blog", "https://blog.csdn.net/ThinkWon");   
```

以下代码：

```java
public static <A extends Comparable<A>> A max(Collection<A> xs) {
    Iterator<A> xi = xs.iterator();
    A w = xi.next();
    while (xi.hasNext()) {
        A x = xi.next();
        if (w.compareTo(x) < 0)
            w = x;
    }
    return w;
}
```

类型擦除后会变成：

```java
public static Comparable max(Collection xs) {
    Iterator xi = xs.iterator();
    Comparable w = (Comparable) xi.next();
    while (xi.hasNext()) {
        Comparable x = (Comparable) xi.next();
        if (w.compareTo(x) < 0)
            w = x;
    }
    return w;
}
```

虚拟机中没有泛型，只有**普通类和普通方法**，所有泛型类的类型参数在编译时都会被擦除，泛型类并没有自己独有的Class类对象。



##### 糖块三、自动装箱与拆箱

自动装箱就是Java自动将原始类型值转换成对应的对象，比如将int的变量转换成Integer对象，这个过程叫做**装箱**，反之将Integer对象转换成int类型值，这个过程叫做**拆箱**。因为这里的装箱和拆箱是自动进行的非人为转换，所以就称作为自动装箱和拆箱。

原始类型byte, short, char, int, long, float, double 和 boolean 对应的封装类为Byte, Short, Character, Integer, Long, Float, Double, Boolean。

先来看个**自动装箱**的代码：

```java
public static void main(String[] args) {
    int i = 10;
    Integer n = i;
}
```

反编译后代码如下:

```java
public static void main(String args[]) {
    int i = 10;
    Integer n = Integer.valueOf(i);
}
```

再来看个**自动拆箱**的代码：

```java
public static void main(String[] args) {
    Integer i = 10;
    int n = i;
}
```

反编译后代码如下：

```java
public static void main(String args[]) {
    Integer i = Integer.valueOf(10);
    int n = i.intValue();
}
```

从反编译得到内容可以看出，在装箱的时候自动调用的是Integer的valueOf(int)方法。而在拆箱的时候自动调用的是Integer的intValue方法。

所以，装箱过程是通过调用包装器的valueOf方法实现的，而拆箱过程是通过调用包装器的 xxxValue方法实现的。



##### 糖块四 、方法变长参数

可变参数(variable arguments)是在Java 1.5中引入的一个特性。它允许一个方法**把任意数量的值作为参数**。

看下以下可变参数代码，其中print方法接收可变参数：

```java
public static void main(String[] args) {
    print("CSDN-ThinkWon", "简书-JourWon", "博客:https://blog.csdn.net/ThinkWon");
}

public static void print(String... strs) {
    for (int i = 0; i < strs.length; i++) {
        System.out.println(strs[i]);
    }
}
```

反编译后代码：

```java
public static void main(String[] args) {
    print(new String[]{"CSDN-ThinkWon", "\u7b80\u4e66-JourWon", "\u535a\u5ba2:https://blog.csdn.net/ThinkWon"});
}

public static void print(String[] strs) {
    for (int i = 0; i < strs.length; i++)
        System.out.println(strs[i]);
}
```

从反编译后代码可以看出，可变参数在被使用的时候，他**首先会创建一个数组**，数组的长度就是调用该方法时**传递实参的个数**，然后再把参数值全部放到这个数组当中，再把这个数组作为参数传递到被调用的方法中。



##### 糖块五 、枚举

Java SE5提供了一种新的类型-Java的**枚举类型**，关键字enum可以将一组具名的值的有限集合创建为一种新的类型，而这些具名的值可以作为常规的程序组件使用，这是一种非常有用的功能。要想看源码，首先得有一个类吧，那么枚举类型到底是什么类呢？是enum吗？

答案很明显不是，enum就和class一样，只是一个关键字，他并不是一个类。

那么枚举是由什么类维护的呢，我们简单的写一个枚举：

```java
public enum T {
    SPRING,SUMMER;
}
```

然后我们使用反编译，看看这段代码到底是怎么实现的，反编译后代码内容如下：

```java
public final class T extends Enum<T> {
    public static final /* enum */ T SPRING = new T("SPRING", 0);
    public static final /* enum */ T SUMMER = new T("SUMMER", 1);
    private static final /* synthetic */ T[] $VALUES;

    public static T[] values() {
        return (T[])$VALUES.clone();
    }

    public static T valueOf(String name) {
        return Enum.valueOf(T.class, name);
    }

    private T(String string, int n) {
        super(string, n);
    }

    static {
        $VALUES = new T[]{SPRING, SUMMER};
    }
}
```

通过反编译后代码我们可以看到，public final class T extends Enum，说明，**该类是继承了Enum类的**，同时final关键字告诉我们，**这个类也是不能被继承的**。

当我们使用enmu来定义一个枚举类型的时候，编译器会自动帮我们创建一个final类型的类继承Enum类，所以枚举类型不能被继承。



##### 糖块六 、内部类

​		内部类又称为嵌套类，可以把内部类理解为外部类的一个普通成员。内部类之所以也是语法糖，是因为它仅仅是一个编译时的概念。outer.java里面定义了一个内部类inner，一旦编译成功，就会生成两个完全不同的.class文件了，分别是outer.class和outer$inner.class。所以内部类的名字完全可以和它的外部类名字相同。

```java
public class OutterClass {

    private String userName;

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public static void main(String[] args) {

    }

    class InnerClass {
        private String name;

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }
    }

}
```

​		以上代码编译后会生成两个class文件：OutterClass$InnerClass.class 、OutterClass.class 。

​		当我们尝试使用jad对OutterClass.class文件进行反编译的时候，命令行会打印以下内容：

```java
Parsing OutterClass.class...
Parsing inner class OutterClass$InnerClass.class...
Generating OutterClass.jad
```

​		它会把两个文件全部进行反编译，然后一起生成一个OutterClass.jad文件。文件内容如下：		

```java
public class OutterClass {
    class InnerClass {
        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        private String name;
        final OutterClass this$0;

        InnerClass() {
            this.this$0 = OutterClass.this;
            super();
        }
    }

    public OutterClass() {
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public static void main(String args1[]) {
    }

    private String userName;
}
```



##### 糖块七 、条件编译

​		—般情况下，程序中的每一行代码都要参加编译。但有时候出于**对程序代码优化的考虑**，希望只对其中一部分内容进行编译，此时就需要在程序中**加上条件**，让编译器只对满足条件的代码进行编译，将不满足条件的代码舍弃，这就是条件编译。如在C或C++中，可以**通过预处理语句来实现条件编译**。其实在Java中也可实现条件编译。我们先来看一段代码：

```java
public class ConditionalCompilation {
    public static void main(String[] args) {
        final boolean DEBUG = true;
        if(DEBUG) {
            System.out.println("Hello, DEBUG!");
        }

        final boolean ONLINE = false;

        if(ONLINE){
            System.out.println("Hello, ONLINE!");
        }
    }
}
```

​		反编译后代码如下：

```java
public class ConditionalCompilation {
    public static void main(String[] args) {
    boolean DEBUG = true;
    
    System.out.println("Hello, DEBUG!");
    
    boolean ONLINE = false;
}
```

​		首先，我们发现，在反编译后的代码中没有System.out.println(“Hello, ONLINE!”);，这其实就是条件编译。当if(ONLINE)为false的时候，编译器就没有对其内的代码进行编译。

​		所以，Java语法的条件编译，**是通过判断条件为常量的if语句实现的**。根据if判断条件的真假，**编译器直接把分支为false的代码块消除**。通过该方式实现的条件编译，**必须在方法体内实现**，而无法在整个Java类的结构或者类的属性上进行条件编译。这与C/C++的条件编译相比，确实更有局限性。在Java语言设计之初并没有引入条件编译的功能，虽有局限，但是总比没有更强。



##### 糖块八 、断言

​		在Java中，assert关键字是从JAVA SE 1.4 引入的，为了避免和老版本的Java代码中使用了assert关键字导致错误，Java在执行的时候默认是不启动断言检查的（这个时候，所有的断言语句都将忽略）。

​		如果要开启断言检查，则需要用开关-enableassertions或-ea来开启。

​		看一段包含断言的代码：

```java
public class AssertTest {
    public static void main(String args[]) {
        int a = 1;
        int b = 1;
        assert a == b;
        System.out.println("CSDN-ThinkWon");
        assert a != b : "ThinkWon";
        System.out.println("博客:https://blog.csdn.net/ThinkWon");
    }
}
```

​		反编译后代码如下：

```java
public class AssertTest {
	static final /* synthetic */ boolean $assertionsDisabled;

    public static void main(String[] args) {
        boolean a = true;
        boolean b = true;
        if (!$assertionsDisabled && a != b) {
            throw new AssertionError();
        }
        System.out.println("CSDN-ThinkWon");
        if (!$assertionsDisabled && a == b) {
            throw new AssertionError((Object)"ThinkWon");
        }
        System.out.println("\u535a\u5ba2:https://blog.csdn.net/ThinkWon");
    }
    static {
        $assertionsDisabled = !AssertTest.class.desiredAssertionStatus();
    }
}
```

​		很明显，反编译之后的代码要比我们自己的代码复杂的多。所以，使用了assert这个语法糖我们节省了很多代码。

​		其实断言的底层实现就是**if语言**，如果断言结果为true，则什么都不做，程序继续执行，如果断言结果为false，则程序抛出AssertError来打断程序的执行。-enableassertions会设置$assertionsDisabled字段的值。



##### 糖块九 、数值字面量

​		在java 7中，数值字面量，不管是整数还是浮点数，都允许在数字之间插入任意多个下划线。这些下划线不会对字面量的数值产生影响，目的就是方便阅读。

```java
public class Test {
    public static void main(String[] args) {
        int i = 10_000;
        System.out.println(i);
    }
}
```

​		反编译后：

```java
public class Test {
    public static void main(String[] args) {
        int i = 10000;
        System.out.println(i);
    }
}
```

​		反编译后就是把删除了`_`。也就是说编译器并不认识在数字字面量中的`_`，需要在编译阶段把他去掉。



##### 糖块十 、增强for循环

​		增强for循环（for-each）相信大家都不陌生，日常开发经常会用到的，他会比for循环要少写很多代码，那么这个语法糖背后是如何实现的呢？

```java
public static void main(String args[]) {
    String[] strs = {"CSDN-ThinkWon", "简书-JourWon", "博客:https://blog.csdn.net/ThinkWon"};
    for (String s : strs) {
        System.out.println(s);
    }
    System.out.println();
    List<String> strList = Arrays.asList(strs);
    for (String s : strList) {
        System.out.println(s);
    }
}
```

​		反编译后代码如下：

```java
public static void main(String args[]) {
    String[] strs;
    String[] arrstring = strs = new String[]{"CSDN-ThinkWon", "\u7b80\u4e66-JourWon", "\u535a\u5ba2:https://blog.csdn.net/ThinkWon"};
    int n = arrstring.length;
    for (int i = 0; i < n; ++i) {
        String s = arrstring[i];
        System.out.println(s);
    }
    System.out.println();
    List<String> strList = Arrays.asList(strs);
    Iterator<String> iterator = strList.iterator();
    while (iterator.hasNext()) {
        String s = iterator.next();
        System.out.println(s);
    }
}
```

​		代码很简单，for-each的实现原理其实就是使用了普通的for循环和迭代器。



##### 糖块十一 、try-with-resource语句

​		Java里，对于文件操作IO流、数据库连接等开销非常昂贵的资源，用完之后必须及时通过close方法将其关闭，否则资源会一直处于打开状态，可能会导致内存泄露等问题。

​		关闭资源的常用方式就是在finally块里释放，即调用close方法。比如，我们经常会写这样的代码：

```java
public static void main(String args[]) {
    BufferedReader br = null;
    try {
        String line;
        br = new BufferedReader(new FileReader("d:\\hello.xml"));
        while ((line = br.readLine()) != null) {
            System.out.println(line);
        }
    } catch (IOException e) {
        // handle exception
    } finally {
        try {
            if (br != null) {
                br.close();
            }
        } catch (IOException ex) {
            // handle exception
        }
    }
}
```

​		从Java 7开始，jdk提供了一种更好的方式关闭资源，使用try-with-resources语句，改写一下上面的代码，效果如下：

```java
public static void main(String args[]) {
    try (BufferedReader br = new BufferedReader(new FileReader("d:\\ hello.xml"))) {
        String line;
        while ((line = br.readLine()) != null) {
            System.out.println(line);
        }
    } catch (IOException e) {
        // handle exception
    }
}
```

​		看，这简直是一大福音啊，虽然我之前一般使用IOUtils去关闭流，并不会使用在finally中写很多代码的方式，但是这种新的语法糖看上去好像优雅很多呢。

​		反编译以上代码，看下他的背后原理：

```java
public static void main(String[] args) {
    try {
        BufferedReader br = new BufferedReader(new FileReader("d:\\ hello.xml"));
        Throwable throwable = null;
        try {
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }
        } catch (Throwable line) {
            throwable = line;
            throw line;
        } finally {
            if (br != null) {
                if (throwable != null) {
                    try {
                        br.close();
                    } catch (Throwable line) {
                        throwable.addSuppressed(line);
                    }
                } else {
                    br.close();
                }
            }
        }
    } catch (IOException br) {
        // empty catch block
    }
}
```

​		其实背后的原理也很简单，那些我们没有做的关闭资源的操作，编译器都帮我们做了。

​		所以，再次印证了，语法糖的作用就是方便程序员的使用，但最终还是要转成编译器认识的语言。



##### 糖块十二、Lambda表达式

​		关于Labmda表达式，有人可能会有质疑，因为网上有人说他并不是语法糖。其实我想纠正下这个说法。Labmda表达式不是匿名内部类的语法糖，但是他也是一个语法糖。实现方式其实是依赖了几个JVM底层提供的Labmda相关api。

​		先来看一个简单的Labmda表达式。遍历一个list：

```java
public static void main(String[] args) {
    List<String> strList = new ArrayList<>();
    strList.add("CSDN-ThinkWon");
    strList.add("简书-JourWon");
    strList.add("博客:https://blog.csdn.net/ThinkWon");

    strList.forEach(s -> System.out.println(s));
}
```

​		为啥说他并不是内部类的语法糖呢，前面讲内部类我们说过，内部类在编译之后会有两个class文件，但是，包含Labmda表达式的类编译后只有一个文件。

​		反编译后代码如下:

```java
public static void main(String[] args) {
    ArrayList<String> strList = new ArrayList<String>();
    strList.add("CSDN-ThinkWon");
    strList.add("\u7b80\u4e66-JourWon");
    strList.add("\u535a\u5ba2:https://blog.csdn.net/ThinkWon");
    strList.forEach((Consumer<String>)LambdaMetafactory.metafactory(null, null, null, (Ljava/lang/Object;)V, lambda$main$0(java.lang.String ), (Ljava/lang/String;)V)());
}
		private static /* synthetic */ void lambda$main$0(String s) {
    System.out.println(s);
    }
}
```

​		可以看到，在forEach方法中，其实是调用了java.lang.invoke.LambdaMetafactory#metafactory方法，该方法的第四个参数implMethod指定了方法实现。可以看到这里其实是调用了一个lambda\$main$0方法进行了输出。

​		再来看一个稍微复杂一点的，先对List进行过滤，然后再输出：

```java
public static void main(String[] args) {
    List<String> strList = new ArrayList<>();
    strList.add("CSDN-ThinkWon");
    strList.add("简书-JourWon");
    strList.add("博客:https://blog.csdn.net/ThinkWon");
    List list = strList.stream().filter(string -> string.contains("CSDN-ThinkWon")).collect(Collectors.toList());
    list.forEach(s -> {
        System.out.println(s);
    });
}
```

​		反编译后代码如下：

```java
public static void main(String[] args) {
    ArrayList<String> strList = new ArrayList<String>();
    strList.add("CSDN-ThinkWon");
    strList.add("\u7b80\u4e66-JourWon");
    strList.add("\u535a\u5ba2:https://blog.csdn.net/ThinkWon");
    List<Object> list = strList.stream().filter((Predicate<String>)LambdaMetafactory.metafactory(null, null, null, (Ljava/lang/Object;)Z, lambda$main$0(java.lang.String ), (Ljava/lang/String;)Z)()).collect(Collectors.toList());
    list.forEach((Consumer<Object>)LambdaMetafactory.metafactory(null, null, null, (Ljava/lang/Object;)V, lambda$main$1(java.lang.Object ), (Ljava/lang/Object;)V)());
}

private static /* synthetic */ void lambda$main$1(Object s) {
    System.out.println(s);
}

private static /* synthetic */ boolean lambda$main$0(String string) {
    return string.contains("CSDN-ThinkWon");
}
```

​		两个Labmda表达式分别调用了lambda$main1和lambda1和*l**a**m**b**d**a*main$0两个方法。

​		所以，Labmda表达式的实现其实是依赖了一些底层的api，在编译阶段，编译器会把Labmda表达式进行解糖，转换成调用内部api的方式。



##### 可能遇到的坑

###### 泛型-当泛型遇到重载

```java
public class GenericTypes {

    public static void method(List<String> list) {  
        System.out.println("invoke method(List<String> list)");  
    }  
    public static void method(List<Integer> list) {  
        System.out.println("invoke method(List<Integer> list)");  
    }  
}  
```

​		上面这段代码，有两个重载的函数，因为他们的参数类型不同，但是，这段代码是编译通不过的。因为我们前面讲过，参数List和List编译之后都被擦除了，变成了一样的原生类型List，擦除动作导致这两个方法的特征签名变得一模一样。

###### 泛型-当泛型遇到catch

​		泛型的类型参数不能用在Java异常处理的catch语句中。因为异常处理是由JVM在运行时刻来进行的。由于类型信息被擦除，JVM是无法区分两个异常类型MyException和MyException的

###### 泛型-当泛型内包含静态变量

```java
public class StaticTest{
    public static void main(String[] args) {
        GT<Integer> gti = new GT<Integer>();
        gti.var = 1;
        GT<String> gts = new GT<String>();
        gts.var = 2;
        System.out.println(gti.var);
    }
    class GT<T> {
        public static int var = 0;
        public void nothing(T x) {
        }
    }
}
```

​		这段代码编译都无法通过，因为**泛型里面不能引用静态变量**。由于经过类型擦除，所有的泛型类实例都关联到同一份字节码上，泛型类的所有静态变量是共享的。

###### 自动装箱与拆箱-对象相等比较

```java
public static void main(String[] args) {
    Integer a = 1000;
    Integer b = 1000;
    Integer c = 100;
    Integer d = 100;
    System.out.println("a == b is " + (a == b));
    System.out.println(("c == d is " + (c == d)));
}
```

​		输出结果：

```java
a == b is false
c == d is true
```

​		在Java 5中，在Integer的操作上引入了一个新功能来节省内存和提高性能。整型对象通过使用相同的对象引用实现了**缓存和重用**。

> 适用于整数值区间-128 至 +127。
>
> 只适用于自动装箱。使用构造函数创建对象不适用。

###### 增强for循环

```java
for (Student stu : students) {    
    if (stu.getId() == 2) {
        students.remove(stu);  
    }           
}
```

​		会抛出ConcurrentModificationException异常。Iterator是**工作在一个独立的线程中**，并且拥有一个 **mutex 锁**。 Iterator被创建之后会建立**一个指向原来对象的单链索引表**，当原来的对象数量发生变化时，这个索引表的内容不会同步改变，所以当索引指针往后移动的时候就找不到要迭代的对象，所以按照 fail-fast 原则 Iterator 会马上抛出java.util.ConcurrentModificationException异常。**所以 Iterator 在工作的时候是不允许被迭代的对象被改变的**。但你可以使用 **Iterator 本身的方法remove()**来删除对象，Iterator.remove() 方法**会在删除当前迭代对象的同时维护索引的一致性**。

##### 总结

​		前面介绍了12种Java中常用的语法糖。所谓语法糖就是提供给开发人员便于开发的一种语法而已。但是这种语法只有开发人员认识。要想被执行，需要进行**解糖**，即转成JVM认识的语法。当我们把语法糖解糖之后，你就会发现其实我们日常使用的这些方便的语法，其实都是一些**其他更简单的语法构成**的。有了这些语法糖，我们在日常开发的时候可以大大提升效率，但是同时**也要避免过度使用**。使用之前最好了解下原理，避免掉坑。



#### Json简介

##### 概述

JSON(JavaScript Object Notation JavaScript 对象表示法)是一种轻量级的数据交换格式，它基于JavaScript的一个子集，易于人的编写和阅读，也易于机器解析。 JSON采用完全独立于语言的文本格式，但是也使用了类似于C语言家族的习惯（包括C, C++, C#, Java, JavaScript, Perl, Python等）。 这些特性使JSON成为理想的数据交换语言。

##### 结构组成

JSON由两种结构组成：

- 键值对的无序集合——对象(或者叫记录、结构、字典、哈希表、有键列表或关联数组等)
- 值的有序列表——数组

##### 语法规则

- 数据在名称/值对中
- 数据由逗号分隔
- 大括号保存对象
- 中括号保存数组

##### JSON的形式

###### 对象

一个无序键值对的集合，以"{“开始，同时以”}“结束，键值对之间以”:“相隔，不同的键值对之间以”,"相隔，举例

```json
{"key1" : 1, "key2" : "string" } 
```

###### 数组

值（value）的有序集合。一个数组以“[”（左中括号）开始，“]”（右中括号）结束。值之间使用“,”（逗号）分隔。

```json
[ "Google", "Runoob", "Taobao" ]
```

值（value）可以是双引号括起来的字符串（string）、数值(number)、true、false、 null、对象（object）或者数组（array）。这些结构可以嵌套。

字符串（string）是由双引号包围的任意数量Unicode字符的集合，使用反斜线转义。一个字符（character）即一个单独的字符串（character string）。



#### XML简介

##### 什么是 XML

XML 是可扩展标记语言（EXtensible Markup Language）。
XML 是一种很像HTML的标记语言。
XML 的设计宗旨是传输数据，而不是显示数据。
XML 标签没有被预定义。您需要自行定义标签。
XML 被设计为具有自我描述性。
XML 是 W3C 的推荐标准。

##### XML 用途

XML 应用于 Web 开发的许多方面，常用于简化数据的存储和传输。

###### 存放数据栗子

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persons>
    <person id="p001">
        <name>张三</name>
    </person>
    <person id="p002">
        <name>李四</name>
    </person>
</persons>
```

###### 配置文件栗子

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5">
    <servlet>
        <servlet-name>HelloMyServlet</servlet-name>
        <servlet-class>com.jourwon.HelloMyServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>HelloMyServlet</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
</web-app>
```

##### XML 语法

XML文件主要由XML文档声明，元素，属性，注释，转义字符，CDATA区，处理指令组成。

###### 文档声明

<?xml version="1.0" encoding="UTF-8"?>
文档声明必须为<?xml开头，以?>结束；
文档声明必须从文档的0行0列位置开始；
文档声明只有三个属性：
versioin：指定XML文档版本。必须属性，因为我们不会选择1.1，只会选择1.0；
encoding：指定当前文档的编码。可选属性，默认值是utf-8；
standalone：指定文档独立性。可选属性，默认值为yes，表示当前文档是独立文档。如果为no表示当前文档不是独立的文档，会依赖外部文件。

###### 元素

<servlet>
元素是XML文档中最重要的组成部分，
普通元素的结构开始标签、元素体、结束标签组成。例如：大家好
元素体：元素体可以是元素，也可以是文本，例如：你好
空元素：空元素只有开始标签，而没有结束标签，但元素必须自己闭合，例如：
元素命名：
a) 区分大小写
b) 不能使用空格，不能使用冒号:
c) 不建议以XML、xml、Xml开头
良好的XML文档，必须有一个根元素。

###### 属性

<web-app version="2.5">
属性是元素的一部分，它必须出现在元素的开始标签中
属性的定义格式：属性名=属性值，其中属性值必须使用单引或双引
一个元素可以有0~N个属性，但一个元素中不能出现同名属性
属性名不能使用空格、冒号等特殊字符，且必须以字母开头

###### 注释

XML 文件中注释采用："" 这样的格式

XML 声明之前不能有注释

注释不能嵌套，比如下面不合规范：

###### 转义字符

XML中的转义字符与HTML一样。
因为很多符号已经被XML文档结构所使用，所以在元素体或属性值中想使用这些符号就必须使用转义字符，例如：“<”、“>”、“’”、“””、“&”。
![image-20200304153621248](/Users/xiaoxiangyuzhu/Library/Application Support/typora-user-images/image-20200304153621248.png)

###### CDATA区

<![CDATA[任意内容]]>

当大量的转义字符出现在xml文档中时，会使xml文档的可读性大幅度降低。这时如果使用CDATA段就会好一些。
在CDATA段中出现的“<”、“>”、“””、“’”、“&”，都无需使用转义字符。这可以提高xml文档的可读性。
在CDATA段中不能包含“]]>”，即CDATA段的结束定界符。

###### 处理指令

用来解析引擎如何解析 XML 文档内容

比如：在 XML 文档中可以使用 xml-stylesheet 指令，通知 XML 解析引擎，应用 CSS 文件显示 XML 文档内容

<?xml-stylesheet type="text/css" href="a.css"?>
处理指令必须以<? 开头，以 > 结尾

##### XML的解析

开发中比较常见的解析方式有三种

###### DOM：要求解析器把整个XML文档装载到内存，并解析成一个Document对象。

a) 优点：元素与元素之间保留结构关系，故可以进行增删改查操作。
b) 缺点：XML文档过大，可能出现内存溢出显现。

###### SAX：是一种速度更快，更有效的方法。它逐行扫描文档，一边扫描一边解析。并以事件驱动的方式进行具体解析，每执行一行，都将触发对应的事件。（了解）

a) 优点：处理速度快，可以处理大文件
b) 缺点：只能读，逐行后将释放资源。

###### PULL：Android内置的XML解析方式，类似SAX。（了解）

##### 常见的解析开发包

- JAXP：sun公司提供支持DOM和SAX开发包
- JDom：dom4j兄弟
- jsoup：一种处理HTML特定解析开发包
- dom4j：比较常用的解析开发包，hibernate底层采用。



































### Java集合综述

#### Java集合基础概念

##### 1.简介

**集合框架**：用于存储数据的容器。

-   集合框架是为表示和操作集合而规定的一种统一的标准的体系结构。
-   任何集合框架都包含三大块内容：**对外的接口**、**接口的实现**和**对集合运算的算法**。

**接口**：表示集合的抽象数据类型。接口允许我们操作集合时不必关注具体实现，从而达到“多态”。在面向对象编程语言中，接口通常用来形成规范。

**实现**：集合接口的具体实现，是重用性很高的数据结构。

**算法**：在一个实现了某个集合框架中的接口的对象身上完成某种有用的计算的方法，例如查找、排序等。这些算法通常是多态的，因为相同的方法可以在同一个接口被多个类实现时有不同的表现。事实上，算法是可复用的函数。它减少了程序设计的辛劳。

​		集合框架通过提供有用的数据结构和算法使你能集中注意力于你的程序的重要部分上，而不是为了让程序能正常运转而将注意力于低层设计上。通过这些在无关API之间的简易的互用性，使你免除了为改编对象或转换代码以便联合这些API而去写大量的代码。 它提高了程序速度和质量。

##### **2.特点**

- 对象封装数据，对象多了也需要存储。集合用于**存储对象**。


- 对象的个数**确定可以使用数组**，对象的个数**不确定的可以用集合**。因为集合是可变长度的。

**集合和数组的区别**

- 数组是**固定长度的**；集合**可变长度的**。


- 数组可以存储**基本数据类型**，也可以存储**引用数据类型**；     集合**只能存储引用数据类型**。


- 数组存储的元素必须**是同一个数据类型**；    集合存储的对象**可以是不同数据类型**。


**数据结构**：就是容器中存储数据的方式。

-   对于集合容器，有很多种。因为每一个容器的自身特点不同，其实原理在于每个容器的内部数据结构不同。


-   集合容器在不断向上抽取过程中，出现了集合体系。在使用一个体系的原则：参阅顶层内容。建立底层对象。


**使用集合框架的好处**

- 容量自增长；
- 提供了高性能的数据结构和算法，使编码更轻松，提高了**程序速度和质量**；
- 允许不同 API 之间的互操作，API之间可以来回**传递集合**；
- 可以方便地扩展或改写集合，提高代码**复用性和可操作性**。
- 通过使用JDK自带的集合类，可以**降低代码维护和学习新API成本**。

##### 3.Iterator接口

Iterator接口，用于**遍历集合元素的接口**。

在Iterator接口中定义了三个方法：

**修饰与类型	方法与描述**

**boolean**		**hasNext()** 如果仍有元素可以迭代，则返回true。

**E**					 **next()** 返回迭代的下一个元素。

**void**			   **remove()** 从迭代器指向的 collection 中移除迭代器返回的最后一个元素（可选操作）。

​		每一个集合都有自己的数据结构(就是容器中**存储数据的方式**)，都有特定的取出自己内部元素的方式。为了便于操作所有的容器，取出元素。将容器内部的取出方式按照一个统一的规则向外提供，这个规则就是Iterator接口，使得对容器的遍历操作与其具体的底层实现相隔离，达到解耦的效果。也就说，只要通过该接口就可以取出Collection集合中的元素，至于每一个具体的容器依据自己的数据结构，如何实现的具体取出细节，这个不用关心，这样就降低了取出元素和具体集合的耦合性。

​		**使用迭代器遍历集合元素**

```java
public static void main(String[] args) {
    List<String> list1 = new ArrayList<>();
    list1.add("abc0");
    list1.add("abc1");
    list1.add("abc2");
// while循环方式遍历
Iterator it1 = list1.iterator();
while (it1.hasNext()) {
    System.out.println(it1.next());
	}
// for循环方式遍历
for (Iterator it2 = list1.iterator(); it2.hasNext(); ) {
    System.out.println(it2.next());
	}
}
```

​		**使用Iterator迭代器删除集合元素，则不会出现并发修改异常**。因为：在执行remove操作时，同样先执行checkForComodification()，

然后会执行**ArrayList的remove()方法**，该方法会将modCount值加1，这里我们将expectedModCount=modCount，使之保持统一。

##### 4.ListIterator接口

​		ListIterator是一个**功能更加强大的迭代器**， 它继承于Iterator接口，只能用于**各种List类型的访问**。可以通过调用listIterator()方法产生

一个指向List开始处的ListIterator， 还可以调用listIterator(n)方法创建一个一开始就指向列表索引为n的元素处的ListIterator。

**特点**

- 允许我们**向前、向后两个方向遍历 List**；
- 在遍历时修改 List 的元素；
- 遍历时获取迭代器当前游标所在位置。

**常用API**

修饰与类型	方法与描述
void				**add(E e)** 将指定的元素插入到列表 （可选操作）。
boolean		 **hasNext()** 如果此列表迭代器在前进方向还有更多的元素时，返回 true。
boolean		 **hasPrevious()** 如果此列表迭代器在相反方向还有更多的元素时，返回 true。
E					  **next()** 返回列表中的下一个元素和光标的位置向后推进。
int				   **nextIndex()** 返回调用 next()后返回的元素索引。
E					  **previous()** 返回列表中的上一个元素和光标的位置向前移动。
int					**previousIndex()** 返回调用previous() 后返回的元素索引 。
void				 remove() **删除列表中调用next()或previous()的返回最后一个元素**。
void				 **set(E e)** 用指定元素替换列表中调用next()或previous()的返回最后一个元素。



##### 5.Collection接口(单列集合)

​		所有集合类都位于**java.util**包下。Java的集合类主要由两个接口派生而出：Collection和Map，Collection和Map是Java集合框架的根接口，这两个接口又包含了一些子接口或实现类。

- Collection一次存一个元素，是**单列集合**；


- Map一次存一对元素，是**双列集合**。Map存储的一对元素：键–值，键（key）与值(value)间有对应(映射)关系。

**单列集合继承关系图**

![单列集合](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL0pvdXJXb24vaW1hZ2UvbWFzdGVyL0phdmElRTklOUIlODYlRTUlOTAlODglRTYlQTElODYlRTYlOUUlQjYvY29sbGVjdGlvbi5wbmc)

**Collection集合主要有List和Set两大接口**

-   List：有序(元素存入集合的顺序和取出的顺序一致)，元素都有索引。元素可以重复。


-   Set：无序(存入和取出顺序有可能不一致)，不可以存储重复元素。必须保证元素唯一性。


###### List集合

- List是元素**有序并且可以重复**的集合。


- List的主要实现：ArrayList， LinkedList， Vector。


**ArrayList、LinkedList、Vector 的区别**

![image-20200304162128807](/Users/xiaoxiangyuzhu/Library/Application Support/typora-user-images/image-20200304162128807.png)

总结：ArrayList 和 Vector **基于数组实现**，对于随机访问get和set，ArrayList优于LinkedList，因为LinkedList要移动指针。

- LinkedList 不会出现扩容的问题，所以比较适合随机位置增、删。但是其基于链表实现，所以在**定位时需要线性扫描，效率比较低**。
- 当操作是在一列数据的后面添加数据而不是在前面或中间，并且需要随机地访问其中的元素时，使用ArrayList会提供比较好的性能；
- 当你的操作是在**一列数据的前面或中间添加或删除数据，并且按照顺序访问其中的元素时，就应该使用LinkedList了**。

**遍历时操作元素**

```java
/**
  * Description: for循环遍历
  * 输出结果：
  * [a, b, c, d, e]
  * 由结果可知，第二个元素b并未删除，原因是当第一个元素b被删除后，它后面所有的元素都向前移动了一个单位，循环时导致第二个元素b漏掉了
    */
    public static void remove(List<String> list) {
    for (int i = 0; i < list.size(); i++) {
        String s = list.get(i);
        if (s.equals("b")) {
            list.remove(s);
        }
    	}
    }

/**
  * Description: foreach循环遍历
    *
  * 会报错：java.util.ConcurrentModificationException。这是因为在这里，foreach循环遍历容器本质上是使用迭代器进行遍历的，会对修改次数modCount进行检查，不允许集合进行更改操作
    */
    public static void remove2(List<String> list) {
    for (String s : list) {
        if (s.equals("b")) {
            list.remove(s);
        }
        System.out.println(s);
   	 }
    }

/**

  * Description: 使用迭代器遍历，允许删除
    */
    public static void remove3(List<String> list) {
    Iterator<String> it = list.iterator();
    while (it.hasNext()) {
        String s = it.next();
        if (s.equals("b")) {
            it.remove();
        }
    	}
   }
```

​		使用迭代器遍历删除时，能够避免方法二中出现的问题。这是因为：在ArrayList中，modCount是指集合的修改次数，当进行add或者delete时，modCount会+1；expectedModCount是指集合的迭代器的版本号，初始值是modCount，但是当集合进行add或者delete操作时，modCount会+1，而expectedModCount不会改变，所以方法二中会抛出异常。**但是it.remove操作时，会同步expectedModCount的值**，把modCount的值赋予expectedModCount。所以不会抛出异常。

**测试方法**

```java
public static void main(String[] args) {
    List<String> arrayList = new ArrayList<String>();
    arrayList.add("a");
    arrayList.add("b");
    arrayList.add("b");
    arrayList.add("c");
    arrayList.add("d");
    arrayList.add("e");
    // remove(arrayList);
    // remove2(arrayList);
    remove3(arrayList);
    System.out.println(arrayList);

}
```


总结：如果想正确的循环遍历删除（增加）元素，需要使用方法三，也就是迭代器遍历删除（增加）的方法。



###### Set集合

​		Set集合元素无序(存入和取出的顺序不一定一致)，并且没有重复对象。

​		Set的主要实现类：**HashSet**， **TreeSet**。

**HashSet、TreeSet、LinkedHashSet的区别**

![image-20200304162550706](/Users/xiaoxiangyuzhu/Library/Application Support/typora-user-images/image-20200304162550706.png)

**HashSet如何检查重复**

​		当你把对象加入HashSet时，HashSet会先计算对象的**hashcode值来判断对象加入的位置**，同时也会与其他加入的对象的hashcode值作比较，如果没有相符的hashcode，HashSet会假设对象没有重复出现。但是如果发现有相同hashcode值的对象，这时会调用equals()方法来检查hashcode相等的对象是否真的相同。如果两者相同，HashSet就不会让加入操作成功。

**hashCode()与equals()的相关规定：**

- 如果两个对象相等，则hashcode一定也是相同的（充分条件）
- 两个对象相等，equals方法返回true 
- 两个对象有相同的hashcode值，它们也不一定是相等的 （必要条件）
- 综上，equals方法被覆盖过，则hashCode方法也必须被覆盖 （必要条件）
- **hashCode()的默认行为是对堆上的对象产生独特值**。如果没有重写hashCode()，则该class的两个对象无论如何都不会相等（即使这两个对象指向相同的数据）。

**总结：**

​		HashSet是一个**通用功能的Set**，而LinkedHashSet 提供元素**插入顺序保证**，TreeSet是一个**SortedSet实现**，由Comparator 或者 Comparable指定的元素顺序存储元素。



##### 6.Map接口(双列集合)

​		Map 是一种把键对象和值对象映射的集合，它的每一个元素都包含一对键对象和值对象。 Map没有继承于Collection接口，从Map集合中检索元素时，只要给出键对象，就会返回对应的值对象。

​		Map 的常用实现类：**HashMap**、TreeMap、HashTable、LinkedHashMap、**ConcurrentHashMap**

**双列集合继承关系图**

![双列集合](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL0pvdXJXb24vaW1hZ2UvbWFzdGVyL0phdmElRTklOUIlODYlRTUlOTAlODglRTYlQTElODYlRTYlOUUlQjYvbWFwLnBuZw)

###### **HashMap、HashTable、TreeMap的区别**

- TreeMap：基于红黑树实现。
- HashMap：基于哈希表实现。
- HashTable：和 HashMap 类似，但它是线程安全的，这意味着同一时刻多个线程可以同时写入 HashTable 并且不会导致数据不一致。**它是遗留类，不应该去使用它**。现在可以使用 **ConcurrentHashMap** 来支持线程安全，并且 ConcurrentHashMap 的效率会更高，因为 ConcurrentHashMap 引入了分段锁。
- LinkedHashMap：使用**双向链表来维护元素的顺序**，顺序为插入顺序或者最近最少使用（LRU）顺序。
    ![image-20200304163233608](/Users/xiaoxiangyuzhu/Library/Application Support/typora-user-images/image-20200304163233608.png)

###### **HashMap在JDK1.7和JDK1.8中有哪些不同**

![image-20200304163313037](/Users/xiaoxiangyuzhu/Library/Application Support/typora-user-images/image-20200304163313037.png)

###### **ConcurrentHashMap在JDK1.7和JDK1.8中有哪些不同**

​		在JDK1.7中ConcurrentHashMap采用了**数组+Segment+分段锁**的方式实现。ConcurrentHashMap中的分段锁称为Segment，它即类似于HashMap的结构，即内部拥有一个**Segment数组**和多个**Entry数组**，数组中的每个元素又是一个链表,同时又是一个**ReentrantLock**（Segment继承了ReentrantLock）。ConcurrentHashMap使用分段锁技术，将数据**分成一段一段的存储**，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据的时候，其他段的数据也能被其他线程访问，能够实现**真正的并发访问**。ConcurrentHashMap定位一个元素的过程需要**进行两次Hash操作**。第一次Hash**定位到Segment**，第二次Hash定位到**元素所在的链表的头部**。

​		具体来讲，ConcurrentHashMap的数据结构是由**一个Segment数组和多个HashEntry组成**,Segment数组的意义就是将一个大的table分割成**多个小的table来进行加锁**，也就是**锁分离技术**，而每一个Segment元素存储的是**HashEntry数组+链表**，这个**和HashMap的数据存储结构一样**.ConcurrentHashMap的初始化是会通过位与运算来初始化Segment的大小，用ssize来表示.Segment的大小取值都是**2的N次方**，无关concurrencyLevel的取值。concurrencyLevel最大只能用16位的二进制来表示，即65536，换句话说，Segment的大小最多65536个，没有指定concurrencyLevel的元素初始化，Segment的大小ssize默认为16。

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200405165052495.png" alt="image-20200405165052495" style="zoom:40%;" />

​		JDK8中ConcurrentHashMap参考了**JDK8 HashMap的实现**，采用了**数组+链表+红黑树的实现方式**来设计，内部大量采用**CAS操作**。Java8 ConcurrentHashMap结构基本上和Java8的HashMap一样，不过保证线程安全性。**1.数据结构**：取消了Segment分段锁的数据结构，取而代之的是数组+链表+红黑树的结构。**2.保证线程安全机制**：JDK1.7采用segment的分段锁机制实现线程安全，其中segment继承自ReentrantLock。JDK1.8采用**CAS+Synchronized保证线程安全**。**3.锁的粒度**：原来是对需要进行数据操作的Segment加锁，现调整为对每个数组**元素加锁**（Node）。**4.链表转化为红黑树**:定位结点的hash算法简化会带来弊端,**Hash冲突加剧**,因此在链表节点数量大于8时，会将链表转化为红黑树进行存储。**5.查询时间复杂度**：从原来的遍历链表O(n)，变成遍历红黑树O(logN)。整个结构看起来就像是**优化过且线程安全的HashMap**，虽然在JDK1.8中还能看到Segment的数据结构，但是已经简化了属性，只是**为了兼容旧版本**。说明：ConcurrentHashMap的数据结构（数组+链表+红黑树），桶中的结构可能是链表，也可能是红黑树，**红黑树是为了提高查找效率**。

-   Node是ConcurrentHashMap存储结构的**基本单元**，继承于HashMap中的**Entry**，用于存储数据, 数据结构很简单，就构成一个**链表**，但是只允许对数据进行查找，不允许进行修改；
-   TreeNode继承于Node，但是数据结构换成了二叉树结构，它是**红黑树的数据的存储结构**，用于红黑树中存储数据，当链表的节点数大于8时会转换成红黑树的结构，他就是通过TreeNode作为存储结构代替Node来转换成红黑树。
-   TreeBin从字面含义中可以理解为**存储树形结构的容器**，而树形结构就是指TreeNode，所以TreeBin就是封装TreeNode的容器，它提供转换黑红树的一些条件和锁的控制。

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200405165355267.png" alt="image-20200405165355267" style="zoom:40%;" />



##### 7.集合工具类Collections

Collections：集合工具类，方便对集合的操作。这个类不需要创建对象，内部提供的都是静态方法。

静态方法：

```java
Collections.sort(list);//list集合进行元素的自然顺序排序。
Collections.sort(list,new ComparatorByLen());//按指定的比较器方法排序。
class ComparatorByLen implements Comparator<String>{
  public int compare(String s1,String s2){
     int temp = s1.length()-s2.length();
     return temp==0?s1.compareTo(s2):temp;
  }
}
Collections.max(list);//返回list中字典顺序最大的元素。
int index = Collections.binarySearch(list,"zz");//二分查找，返回角标。
Collections.reverseOrder();//逆向反转排序。
Collections.shuffle(list);//随机对list中的元素进行位置的置换。

//将非同步集合转成同步集合的方法：Collections中的  XXX synchronizedXXX(XXX);  
//原理：定义一个类，将集合所有的方法加同一把锁后返回。
List synchronizedList(list);
Map synchronizedMap(map);
```


**Collection 和 Collections的区别**

  Collections是个**java.util下的类**，是针对集合类的一个工具类,提供一系列静态方法,实现对集合的**查找、排序、替换、线程安全化**（将非同步的集合转换成同步的）等操作。

  Collection是个**java.util下的接口**，它是**各种集合结构的父接口**，继承于它的接口主要有Set和List,提供了关于集合的一些操作,如插入、删除、判断一个元素是否其成员、遍历等。



##### 8.数组工具类 Arrays

用于操作数组对象的工具类，里面都是静态方法。

**数组 -> 集合：asList方法，将数组转换成list集合。**

```java
String[] arr ={"abc","kk","qq"};
List<String> list =Arrays.asList(arr);//将arr数组转成list集合。
```


将数组转换成集合，有什么好处呢？用aslist方法，将数组变成集合；

- 可以通过list集合中的方法来操作数组中的元素：**isEmpty()、contains、indexOf、set**；


- 注意（局限性）：数组是固定长度，不可以使用集合对象增加或者删除等，会改变数组长度的功能方法。比如add、remove、clear。（会报不支持操作异常UnsupportedOperationException）；


- 如果数组中存储的引用数据类型，直接作为集合的元素可以直接用集合方法操作。


- 如果数组中存储的是基本数据类型，asList会将数组实体作为集合元素存在。

**集合 -> 数组：用的是Collection接口中的toArray()方法;**

- 如果给toArray传递的指定类型的数据长度小于了集合的size，那么toArray方法，会自定再创建一个该类型的数据，长度为集合的size。

- 如果传递的指定的类型的数组的长度大于了集合的size，那么toArray方法，就不会创建新数组，直接使用该数组即可，并将集合中的元素存储到数组中，其他为存储元素的位置默认值null。


- 所以，在传递指定类型数组时，最好的方式就是指定的长度和size相等的数组。

将集合变成数组后有什么好处？限定了对集合中的元素进行增删操作，只要获取这些元素即可。

**用基本数据类型的数组转换ArrayList，ArrayList的size有问题**

```java
public static void main(String[] args) {
    int[] arr1 = { 1, 2, 3, 4, 5 };
    List<int[]> intList = Arrays.asList(arr1);
    // intList size: 1
    System.out.println(String.format("intList size: %s", intList.size()));
  
		Integer[] arr2 = { 1, 2, 3, 4, 5 };
		List<Integer> integerList = Arrays.asList(arr2);
		// integerList size: 5
		System.out.println(String.format("integerList size：%s", integerList.size()));
}
```

asList方法接受的参数是一个**泛型的变长参数**，我们知道**基本数据类型是无法泛型化的**，也就是说基本类型是无法作为asList方法的参数的， 要想作为泛型参数就必须使用其所对应的包装类型。但是这个这个实例中为什么没有出错呢？因为该实例是**将int 类型的数组当做其参数**，而在Java中**数组是一个对象**，它是**可以泛型化的**。所以该例子是不会产生错误的。既然例子是将整个int 类型的数组当做泛型参数，那么经过asList转换就只有一个int 的列表了.

**结论：**在使用asList()时**尽量不要将基本数据类型数组转List**. 

**asList转换得到的ArrayList不是java.util.ArrayList**

```java
public static void main(String[] args) {
    String[] arr = {"abc", "kk", "qq"};
    List<String> list = Arrays.asList(arr);
    // 添加一个元素,抛出异常UnsupportedOperationException
    list.add("bb");
}
```

**原因：**
此处ArrayList是Arrays的内部类,并没有add方法,**add方法是父类AbstractList的**,但是没有具体实现,
而是直接抛出UnsupportedOperationException异常.

**正确操作**

```java
public static void main(String[] args) {
    String[] arr = {"abc", "kk", "qq"};
	// 使用new ArrayList包裹一层
    List<String> list = new ArrayList<>(Arrays.asList(arr));
    list.add("bb");
}
```

##### 9.如何选用集合?

主要根据集合的特点来选用，比如我们需要根据键值获取到元素值时就选用Map接口下的集合，需要排序时选择TreeMap,**不需要排序时就选择HashMap**,需要**保证线程安全就选用ConcurrentHashMap**.当我们只需要存放元素值时，就选择实现Collection接口的集合，需要保证元素唯一时选择实现**Set接口的集合比如TreeSet或HashSet**，不需要就选择实现**List接口的比如ArrayList或LinkedList**，然后再根据实现这些接口的集合的特点来选用。





#### ArrayList(JDK1.8)源码解析

既然是看源码，那我们要怎么看一个类的源码呢？这里我推荐的方法是：

1）看继承结构

	看这个类的层次结构，处于一个什么位置，可以在自己心里有个大概的了解。

2）看构造方法

	在构造方法中，看做了哪些事情，跟踪方法中里面的方法。

3）看常用的方法

	跟构造方法一样，这个方法实现功能是如何实现的

注：既然是源码，为什么要这样设计类，有这样的继承关系。这就要说到设计模式的问题了。所以我们要了解常用的设计模式，才能更深刻的去理解这个类。

##### 1.简介

ArrayList 是 Java 集合框架中 List 接口的一个实现类。底层是数组，**相当于动态数组**。与 Java 中的数组相比，它的**容量能动态增长**。

ArrayList是Vector的翻版，区别在于ArrayList是**线程不安全**的，而Vector则是**线程安全**的。但是Vector是一个较老的集合，具有很多缺点，不建议使用，这里我们就不对其进行分析了。

ArrayList 可以说是我们使用最多的 List 集合，它有以下特点：

- 它是基于数组实现的List类
- **可以动态地调整容量**
- **有序的**（元素输出顺序与输入顺序一致）
- **元素可以为 null**
- **不同步，非线程安全，效率高**
- 查询快，增删慢
- **占用空间更小，对比 LinkedList，不用占用额外空间维护链表结构**

ArrayList 为什么有这些优点呢？我们通过源码来分析分析。在阅读源码前先来看看ArrayList继承关系。

![ArrayList继承关系图](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL0pvdXJXb24vaW1hZ2UvbWFzdGVyL0phdmElRTklOUIlODYlRTUlOTAlODglRTYlQkElOTAlRTclQTAlODElRTglQUYlQTYlRTglQTclQTMlRTQlQjklOEJBcnJheUxpc3QvQXJyYXlMaXN0JUU3JUJCJUE3JUU2JTg5JUJGJUU1JTg1JUIzJUU3JUIzJUJCJUU1JTlCJUJFLnBuZw)

可以看到，ArrayList是AbstractList的子类，同时实现了List接口。除此之外，它还实现了三个标识型接口，这几个接口都没有任何方法，仅作为标识表示实现类具备某项功能。RandomAccess表示实现类支持快速随机访问，Cloneable表示实现类支持克隆，具体表现为重写了clone方法，java.io.Serializable则表示支持序列化，如果需要对此过程自定义，可以重写writeObject与readObject方法。

##### 2.成员变量

```java
// 序列号
private static final long serialVersionUID = 8683452581122892189L;
// 数组初始容量为 10
private static final int DEFAULT_CAPACITY = 10;
// 空对象数组
private static final Object[] EMPTY_ELEMENTDATA = {};
// 缺省空对象数组
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
// 底层数据结构，数组
transient Object[] elementData;
// 数组元素个数，默认为0
private int size;
// 最大数组容量
private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;
```

##### 3.构造方法

```java
//默认构造方法，初始为空数组。
//只有插入一条数据后才会扩展为10，而实际上默认是空的
 public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}
//根据指定容量创建对象数组
public ArrayList(int initialCapacity) {
    if (initialCapacity > 0) {
        //创建initialCapacity大小的数组
        this.elementData = new Object[initialCapacity];
    } else if (initialCapacity == 0) {
        //创建空数组
        this.elementData = EMPTY_ELEMENTDATA;
    } else {
        throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
    }
}
/**
 * 构造一个包含指定集合的元素的列表，按照它们由集合的迭代器返回的顺序。
 */
public ArrayList(Collection<? extends E> c) {
    //转换最主要的是toArray()，这在Collection中就定义了
    elementData = c.toArray();
    if ((size = elementData.length) != 0) {
        // c.toArray 有可能不返回一个 Object 数组
        if (elementData.getClass() != Object[].class)
            //使用 Arrays.copy 方法拷创建一个 Object 数组
            elementData = Arrays.copyOf(elementData, size, Object[].class);
    } else {
        // 替换为空数组
        this.elementData = EMPTY_ELEMENTDATA;
    }
}
```

以无参数构造方法创建 ArrayList 时，实际上初始化赋值的**是一个空数组**。当真正对数组进行添加元素操作时，才真正分配容量。即向数组中添加第一个元素时，数组容量扩为10。

##### 4.内部类

```java
(1)private class Itr implements Iterator<E>  
(2)private class ListItr extends Itr implements ListIterator<E>  
(3)private class SubList extends AbstractList<E> implements RandomAccess  
(4)static final class ArrayListSpliterator<E> implements Spliterator<E>  
```

ArrayList有四个内部类，其中的Itr是实现了Iterator接口，同时重写了里面的hasNext()， next()， remove() 等方法；其中的ListItr 继承 Itr，实现了ListIterator接口，同时重写了hasPrevious()， nextIndex()， previousIndex()， previous()， set(E e)， add(E e) 等方法，所以这也可以看出了 Iterator和ListIterator的区别：ListIterator在Iterator的基础上增加了**添加对象**，**修改对象**，**逆向遍历**等方法，这些是Iterator不能实现的。


##### 5.核心方法

###### add()方法

增和删是`ArrayList`最重要的部分，这部分代码需要我们细细研究

```java
//添加一个特定的元素到list的末尾
public boolean add(E e) {
    //先确保elementData数组的长度足够，size是数组中数据的个数，因为要添加一个元素，所以size+1，先判断size+1的这个个数数组能否放得下，在这个方法中去判断数组长度是否够用
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    //在数据中正确的位置上放上元素e，并且size++
    elementData[size++] = e;
    return true;
}

//在指定位置添加一个元素
public void add(int index, E element) {
    rangeCheckForAdd(index);

    //先确保elementData数组的长度足够
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    //将数据整体向后移动一位，空出位置之后再插入，效率不太好
    System.arraycopy(elementData, index, elementData, index + 1,
                         size - index);
    elementData[index] = element;
    size++;
}

// 校验插入位置是否合理
private void rangeCheckForAdd(int index) {
    //插入的位置肯定不能大于size 和小于0
    if (index > size || index < 0)   
        //如果是，就报越界异常
        throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
}

//添加一个集合
public boolean addAll(Collection<? extends E> c) {
    //把该集合转为对象数组
    Object[] a = c.toArray();
    int numNew = a.length;
    //增加容量
    ensureCapacityInternal(size + numNew);  // Increments modCount
    //挨个向后迁移
    System.arraycopy(a, 0, elementData, size, numNew);
    size += numNew;
    //新数组有元素，就返回 true
    return numNew != 0;
}

//在指定位置，添加一个集合
public boolean addAll(int index, Collection<? extends E> c) {
    rangeCheckForAdd(index);

    Object[] a = c.toArray();
    int numNew = a.length;
    ensureCapacityInternal(size + numNew);  // Increments modCount

    int numMoved = size - index;
    //原来的数组挨个向后迁移
    if (numMoved > 0)
        System.arraycopy(elementData, index, elementData, index + numNew,
                         numMoved);
    //把新的集合数组 添加到指定位置
    System.arraycopy(a, 0, elementData, index, numNew);
    size += numNew;
    return numNew != 0;
}
```

**数组容量调整**

```java
//确保内部容量够用
private void ensureCapacityInternal(int minCapacity) {
    ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
}

//计算容量。判断初始化的elementData是不是空的数组,如果是空的话，返回默认容量10与minCapacity=size+1的较大值者。
private static int calculateCapacity(Object[] elementData, int minCapacity) {
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        return Math.max(DEFAULT_CAPACITY, minCapacity);
    }
    return minCapacity;
}

//确认实际的容量，这个方法就是真正的判断elementData是否够用
private void ensureExplicitCapacity(int minCapacity) {
    modCount++;

    //minCapacity如果大于了实际elementData的长度，那么就说明elementData数组的长度不够用，不够用那么就要增加elementData的length。这里有的小伙伴就会模糊minCapacity到底是什么呢，这里解释一下

/**
     * 当我们要 add 进第1个元素到 ArrayList 时，elementData.length 为0 （因为还是一个空的 list），因为执行了 `ensureCapacityInternal()` 方法 ，所以 minCapacity 此时为10。此时，`minCapacity - elementData.length > 0 `成立，所以会进入 `grow(minCapacity)` 方法。
     * 当add第2个元素时，minCapacity 为2，此时e lementData.length(容量)在添加第一个元素后扩容成 10 了。此时，`minCapacity - elementData.length > 0 ` 不成立，所以不会进入 （执行）`grow(minCapacity)` 方法。
     * 添加第3、4···到第10个元素时，依然不会执行grow方法，数组容量都为10。
     * 直到添加第11个元素，minCapacity(为11)比elementData.length（为10）要大。进入grow方法进行扩容。
     */
    // overflow-conscious code
    if (minCapacity - elementData.length > 0)
        //ArrayList能自动扩展大小的关键方法就在这里了
        grow(minCapacity);
}

//扩容核心方法
private void grow(int minCapacity) {
    //将扩充前的elementData大小给oldCapacity
    // overflow-conscious code
    int oldCapacity = elementData.length;
    //新容量newCapacity是1.5倍的旧容量oldCapacity
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    //这句话就是适应于elementData就空数组的时候，length=0，那么oldCapacity=0，newCapacity=0，所以这个判断成立，在这里就是真正的初始化elementData的大小了，就是为10。
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    //如果newCapacity超过了最大的容量限制，就调用hugeCapacity，也就是将能给的最大值给newCapacity
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    //新的容量大小已经确定好了，就copy数组，改变容量大小。
    // minCapacity is usually close to size, so this is a win:
    elementData = Arrays.copyOf(elementData, newCapacity);
}

//这个就是上面用到的方法，很简单，就是用来赋最大值。
private static int hugeCapacity(int minCapacity) {
    if (minCapacity < 0) // overflow
        throw new OutOfMemoryError();
    //如果minCapacity都大于MAX_ARRAY_SIZE，那么就Integer.MAX_VALUE返回，反之将MAX_ARRAY_SIZE返回。因为maxCapacity是三倍的minCapacity，可能扩充的太大了，就用minCapacity来判断了。
	//Integer.MAX_VALUE:2147483647   MAX_ARRAY_SIZE：2147483639  也就是说最大也就能给到第一个数值。还是超过了这个限制，就要溢出了。相当于arraylist给了两层防护。
    return (minCapacity > MAX_ARRAY_SIZE) ?
        Integer.MAX_VALUE :
    MAX_ARRAY_SIZE;
}
```

至此，我们彻底明白了`ArrayList`的扩容机制了。首先创建一个空数组**elementData**，第一次插入数据时直接扩充至10，然后如果**elementData**的长度不足，就扩充至1.5倍，如果扩充完还不够，就使用需要的长度作为**elementData**的长度。

**大数据插入问题**

这样的方式显然比我们例子中好一些，但是在遇到大量数据时还是会频繁的拷贝数据。那么如何缓解这种问题呢，ArrayList为我们提供了两种可行的方案：

- 使用ArrayList(int initialCapacity)这个有参构造，在创建时就声明一个较大的大小，这样解决了频繁拷贝问题，但是需要我们提前预知数据的数量级，也会一直占有较大的内存。
- 除了添加数据时可以自动扩容外，我们还可以在插入前先进行一次扩容。只要提前预知数据的数量级，就可以在需要时直接一次扩充到位，与ArrayList(int initialCapacity)相比的好处在于不必一直占有较大内存，同时数据拷贝的次数也大大减少了。这个方法就是ensureCapacity(int minCapacity)，其内部就是调用了ensureCapacityInternal(int minCapacity)。

我们这里使用ensureCapacity()方法测试

```java
public class EnsureCapacityTest {
	public static void main(String[] args) {
        ArrayList<Object> list = new ArrayList<Object>();
        final int N = 10000000;
        long startTime = System.currentTimeMillis();
        for (int i = 0; i < N; i++) {
            list.add(i);
        }
        long endTime = System.currentTimeMillis();
        System.out.println("使用ensureCapacity方法前：" + (endTime - startTime));

        list = new ArrayList<Object>();
        long startTime1 = System.currentTimeMillis();
        list.ensureCapacity(N);
        for (int i = 0; i < N; i++) {
            list.add(i);
        }
        long endTime1 = System.currentTimeMillis();
        System.out.println("使用ensureCapacity方法后：" + (endTime1 - startTime1));
    }
}
```

运行结果

```
使用ensureCapacity方法前：2700
使用ensureCapacity方法后：1054
```

通过运行结果，我们可以很明显的看出向 ArrayList 添加大量元素之前最好先使用`ensureCapacity` 方法，以减少增量重新分配的次数

###### remove()方法

```java
//根据索引删除指定位置的元素
public E remove(int index) {
    //检查index的合理性
    rangeCheck(index);
	//这个作用很多，比如用来检测快速失败的一种标志。
    modCount++;
    //通过索引直接找到该元素
    E oldValue = elementData(index);

    //计算要移动的位数。
    int numMoved = size - index - 1;
    if (numMoved > 0)
        //移动元素，挨个往前移一位。
        System.arraycopy(elementData, index+1, elementData, index,
                         numMoved);
    //将--size上的位置赋值为null，让gc(垃圾回收机制)更快的回收它。
    elementData[--size] = null; // clear to let GC do its work
	//返回删除的元素。
    return oldValue;
}

//从此列表中删除指定元素的第一个匹配项，如果存在，则删除。通过元素来删除该元素，就依次遍历，如果有这个元素，就将该元素的索引传给fastRemobe(index)，使用这个方法来删除该元素，fastRemove(index)方法的内部跟remove(index)的实现几乎一样，这里最主要是知道arrayList可以存储null值
public boolean remove(Object o) {
    if (o == null) {
        //挨个遍历找到目标
        for (int index = 0; index < size; index++)
            if (elementData[index] == null) {
                //快速删除
                fastRemove(index);
                return true;
            }
    } else {
        for (int index = 0; index < size; index++)
            if (o.equals(elementData[index])) {
                fastRemove(index);
                return true;
            }
    }
    return false;
}

//内部方法，“快速删除”，就是把重复的代码移到一个方法里
private void fastRemove(int index) {
    modCount++;
    int numMoved = size - index - 1;
    if (numMoved > 0)
        System.arraycopy(elementData, index+1, elementData, index,
                         numMoved);
    elementData[--size] = null; // clear to let GC do its work
}

//删除或者保留指定集合中的元素
//用于两个方法，一个removeAll()：它只清除指定集合中的元素，retainAll()用来测试两个集合是否有交集。　
private boolean batchRemove(Collection<?> c, boolean complement) {
    //将原集合，记名为A
    final Object[] elementData = this.elementData;
    //r用来控制循环，w是记录有多少个交集
    int r = 0, w = 0;
    boolean modified = false;
    try {
        //遍历 ArrayList 集合
        for (; r < size; r++)
            //参数中的集合c一次检测集合A中的元素是否有
            if (c.contains(elementData[r]) == complement)
                //有的话，就给集合A
                elementData[w++] = elementData[r];
    } finally {
        //发生了异常，直接把 r 后面的复制到 w 后面
        if (r != size) {
            //将剩下的元素都赋值给集合A
            System.arraycopy(elementData, r,
                             elementData, w,
                             size - r);
            w += size - r;
        }
        if (w != size) {
            //这里有两个用途，在removeAll()时，w一直为0，就直接跟clear一样，全是为null。
            //retainAll()：没有一个交集返回true，有交集但不全交也返回true，而两个集合相等的时候，返回false，所以不能根据返回值来确认两个集合是否有交集，而是通过原集合的大小是否发生改变来判断，如果原集合中还有元素，则代表有交集，而元集合没有元素了，说明两个集合没有交集。
            // 清除多余的元素，clear to let GC do its work
            for (int i = w; i < size; i++)
                elementData[i] = null;
            modCount += size - w;
            size = w;
            modified = true;
        }
    }
    return modified;
}


//保留公共的
public boolean retainAll(Collection<?> c) {
    Objects.requireNonNull(c);
    return batchRemove(c, true);
}

//将elementData中每个元素都赋值为null，等待垃圾回收将这个给回收掉
public void clear() {
    modCount++;
    //并没有直接使数组指向 null,而是逐个把元素置为空，下次使用时就不用重新 new 了
    for (int i = 0; i < size; i++)
        elementData[i] = null;

    size = 0;
}
```

总结：根据索引删除指定位置的元素，此时会把指定下标到数组末尾的元素挨个向前移动一个单位，并且会把数组最后一个元素设置为null，这样是为了方便之后将整个数组不被使用时，会被GC，可以作为小的技巧使用。

###### get()方法

```java
public E get(int index) {
    // 检验索引是否合法
    rangeCheck(index);

    return elementData(index);
}

private void rangeCheck(int index) {
    if (index >= size)
        throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
}
```

###### set()方法

```java
//设定指定下标索引的元素值
public E set(int index, E element) {
    // 检验索引是否合法
    rangeCheck(index);
    // 旧值
    E oldValue = elementData(index);
    // 赋新值
    elementData[index] = element;
    // 返回旧值
    return oldValue;
}
```

###### indexOf()方法

```java
// 从首开始查找数组里面是否存在指定元素
public int indexOf(Object o) {
    // 查找的元素为空
    if (o == null) { 
        // 遍历数组，找到第一个为空的元素，返回下标
        for (int i = 0; i < size; i++) 
            if (elementData[i]==null)
                return i;
    } else { 
        // 查找的元素不为空
        // 遍历数组，找到第一个和指定元素相等的元素，返回下标
        for (int i = 0; i < size; i++) 
            if (o.equals(elementData[i]))
                return i;
    } 
    // 没有找到，返回空
    return -1;
}

//返回列表中指定元素最后一次出现的索引，倒着遍历
public int lastIndexOf(Object o) {
    if (o == null) {
        for (int i = size-1; i >= 0; i--)
            if (elementData[i]==null)
                return i;
    } else {
        for (int i = size-1; i >= 0; i--)
            if (o.equals(elementData[i]))
                return i;
    }
    return -1;
}
```

说明：从头开始查找与指定元素相等的元素，需要注意的是可以查找null元素，意味着ArrayList中可以存放null元素的。与此函数对应的lastIndexOf，表示从尾部开始查找。

###### contains()方法

```java
//判断是否含有某个元素
public boolean contains(Object o) {
    return indexOf(o) >= 0;
}
```

###### toArray()方法

```java
/**
     以正确的顺序返回一个包含此列表中所有元素的数组（从第一个到最后一个元素）; 返回的数组的运行时类型是指定数组的运行时类型。 
     */
public Object[] toArray() {
    //elementData：要复制的数组；size：要复制的长度
    return Arrays.copyOf(elementData, size);
}

public <T> T[] toArray(T[] a) {
    //如果只是要把一部分转换成数组
    if (a.length < size)
        // Make a new array of a's runtime type, but my contents:
        return (T[]) Arrays.copyOf(elementData, size, a.getClass());
    //全部元素拷贝到 数组 a
    System.arraycopy(elementData, 0, a, 0, size);
    if (a.length > size)
        a[size] = null;
    return a;
}
```



#### LinkedList(JDK1.8)源码解析

##### 概述

​		LinkedList 是 Java 集合框架中一个重要的实现，其底层采用的**双向链表结构**。和 ArrayList 一样，LinkedList 也支持空值和重复值。由于 LinkedList 基于链表实现，存储元素过程中，无需像 ArrayList 那样进行扩容。但有得必有失，LinkedList 存储元素的节点需要额外的空间存储前驱和后继的引用。另一方面，LinkedList 在链表头部和尾部插入效率比较高，但在**指定位置进行插入时，效率一般**。原因是，在指定位置插入需要定位到该位置处的节点，此操作的时间复杂度为`O(N)`。最后，LinkedList 是非线程安全的集合类，并发环境下，多个线程同时操作 LinkedList，**会引发不可预知的错误**。

##### 继承体系

​		LinkedList 的继承体系较为复杂，继承自 AbstractSequentialList，同时又实现了 List 和 Deque 接口。LinkedList 继承自 AbstractSequentialList，AbstractSequentialList 又是什么呢？从实现上，AbstractSequentialList 提供了一套基于顺序访问的接口。通过继承此类，子类仅需实现部分代码即可拥有完整的一套访问某种序列表（比如链表）的接口。深入源码，AbstractSequentialList 提供的方法基本上都是通过 ListIterator 实现的，比如：

```java
public E get(int index) {
    try {
        return listIterator(index).next();
    } catch (NoSuchElementException exc) {
        throw new IndexOutOfBoundsException("Index: "+index);
    }
}

public void add(int index, E element) {
    try {
        listIterator(index).add(element);
    } catch (NoSuchElementException exc) {
        throw new IndexOutOfBoundsException("Index: "+index);
    }
}

// 留给子类实现
public abstract ListIterator<E> listIterator(int index);
```

​		所以只要继承类实现了 listIterator 方法，它不需要再额外实现什么即可使用。对于随机访问集合类一般建议继承 AbstractList 而不是 AbstractSequentialList。LinkedList 和其父类一样，也是基于顺序访问。所以 LinkedList 继承了 AbstractSequentialList，但 LinkedList 并没有直接使用父类的方法，而是重新实现了一套的方法。

​		另外，LinkedList **还实现了 Deque (double ended queue)，Deque 又继承自 Queue 接口**。这样 LinkedList 就具备了队列的功能。比如，我们可以这样使用：

```java
Queue<T> queue = new LinkedList<>();
```

​		除此之外，我们基于 LinkedList 还可以实现一些其他的数据结构，比如栈，以此来替换 Java 集合框架中的 Stack 类（该类实现的不好，《Java 编程思想》一书的作者也对此类进行了吐槽）。

##### 源码分析

###### 查找

​		LinkedList 底层基于链表结构，无法向 ArrayList 那样随机访问指定位置的元素。LinkedList 查找过程要稍麻烦一些，需要从链表头结点（或尾节点）向后查找，时间复杂度为 `O(N)`。相关源码如下：

```java
public E get(int index) {
    checkElementIndex(index);
    return node(index).item;
}

Node<E> node(int index) {
    /*
     * 则从头节点开始查找，否则从尾节点查找
     * 查找位置 index 如果小于节点数量的一半，
     */    
    if (index < (size >> 1)) {
        Node<E> x = first;
        // 循环向后查找，直至 i == index
        for (int i = 0; i < index; i++)
            x = x.next;
        return x;
    } else {
        Node<E> x = last;
        for (int i = size - 1; i > index; i--)
            x = x.prev;
        return x;
    }
}
```

​		上面的代码比较简单，主要是通过遍历的方式定位目标位置的节点。获取到节点后，取出节点存储的值返回即可。这里面有个小优化，即通过比较 index 与节点数量 size/2 的大小，决定从头结点还是尾节点进行查找。查找操作的代码没什么复杂的地方，这里先讲到这里。

###### 遍历

​		链表的遍历过程也很简单，和上面查找过程类似，我们从头节点往后遍历就行了。但对于 LinkedList 的遍历还是需要注意一些，不然可能会导致代码效率低下。通常情况下，我们会使用 foreach 遍历 LinkedList，而 foreach 最终转换成迭代器形式。所以分析 LinkedList 的遍历的核心就是它的迭代器实现，相关代码如下：

```java
public ListIterator<E> listIterator(int index) {
    checkPositionIndex(index);
    return new ListItr(index);
}

private class ListItr implements ListIterator<E> {
    private Node<E> lastReturned;
    private Node<E> next;
    private int nextIndex;
    private int expectedModCount = modCount;

    /** 构造方法将 next 引用指向指定位置的节点 */
    ListItr(int index) {
        // assert isPositionIndex(index);
        next = (index == size) ? null : node(index);
        nextIndex = index;
    }

    public boolean hasNext() {
        return nextIndex < size;
    }

    public E next() {
        checkForComodification();
        if (!hasNext())
            throw new NoSuchElementException();

        lastReturned = next;
        next = next.next;    // 调用 next 方法后，next 引用都会指向他的后继节点
        nextIndex++;
        return lastReturned.item;
    }
    
    // 省略部分方法
}
```

​		上面的方法很简单，大家应该都能很快看懂，这里就不多说了。下面来说说遍历 LinkedList 需要注意的一个点。我们都知道 LinkedList 不擅长随机位置访问，如果大家用随机访问的方式遍历 LinkedList，效率会很差。比如下面的代码：

```java
List<Integet> list = new LinkedList<>();
list.add(1)
list.add(2)
......
for (int i = 0; i < list.size(); i++) {
    Integet item = list.get(i);
    // do something
}
```

​		当链表中存储的元素很多时，上面的遍历方式对于效率来说就是灾难。原因在于，通过上面的方式每获取一个元素，LinkedList 都需要从头节点（或尾节点）进行遍历，效率不可谓不低。在电脑配置如下（MacBook Pro Early 2015, 2.7 GHz Intel Core i5）实测10万级的数据量，耗时约7秒钟。20万级的数据量耗时达到了约34秒的时间。50万级的数据量耗时约250秒。从测试结果上来看，上面的遍历方式在大数据量情况下，效率很差。大家在日常开发中应该尽量避免这种用法。

###### 插入

​		LinkedList 除了实现了 List 接口相关方法，还实现了 Deque 接口的很多方法，所以我们有很多种方式插入元素。但这里，我只打算分析 List 接口中相关的插入方法，其他的方法大家自己看吧。LinkedList 插入元素的过程实际上就是链表链入节点的过程，学过数据结构的同学对此应该都很熟悉了。这里简单分析一下，先看源码吧：

```java
/** 在链表尾部插入元素 */
public boolean add(E e) {
    linkLast(e);
    return true;
}

/** 在链表指定位置插入元素 */
public void add(int index, E element) {
    checkPositionIndex(index);

    // 判断 index 是不是链表尾部位置，如果是，直接将元素节点插入链表尾部即可
    if (index == size)
        linkLast(element);
    else
        linkBefore(element, node(index));
}

/** 将元素节点插入到链表尾部 */
void linkLast(E e) {
    final Node<E> l = last;
    // 创建节点，并指定节点前驱为链表尾节点 last，后继引用为空
    final Node<E> newNode = new Node<>(l, e, null);
    // 将 last 引用指向新节点
    last = newNode;
    // 判断尾节点是否为空，为空表示当前链表还没有节点
    if (l == null)
        first = newNode;
    else
        l.next = newNode;    // 让原尾节点后继引用 next 指向新的尾节点
    size++;
    modCount++;
}

/** 将元素节点插入到 succ 之前的位置 */
void linkBefore(E e, Node<E> succ) {
    // assert succ != null;
    final Node<E> pred = succ.prev;
    // 1. 初始化节点，并指明前驱和后继节点
    final Node<E> newNode = new Node<>(pred, e, succ);
    // 2. 将 succ 节点前驱引用 prev 指向新节点
    succ.prev = newNode;
    // 判断尾节点是否为空，为空表示当前链表还没有节点    
    if (pred == null)
        first = newNode;
    else
        pred.next = newNode;   // 3. succ 节点前驱的后继引用指向新节点
    size++;
    modCount++;
}
```

​		上面是插入过程的源码，我对源码进行了比较详细的注释，应该不难看懂。上面两个 add 方法只是对操作链表的方法做了一层包装，核心逻辑在 linkBefore 和 linkLast 中。这里以 linkBefore 为例，它的逻辑流程如下：

1. 创建新节点，并指明新节点的前驱和后继
2. 将 succ 的前驱引用指向新节点
3. 如果 succ 的前驱不为空，则将 succ 前驱的后继引用指向新节点

###### 总结

通过上面的分析，大家对 LinkedList 的底层实现应该很清楚了。总体来看 LinkedList 的源码并不复杂，大家耐心看一下，一般都能看懂。



#### HashMap(JDK1.8)源码解析

##### 简介

​		在JDK1.8之前，HashMap采用数组+链表实现，即使用链表处理冲突，同一hash值的节点都存储在一个链表里。但是当位于一个桶中的元素较多，即hash值相等的元素较多时，通过key值依次查找的效率较低。而JDK1.8中，为了解决hash碰撞过于频繁的问题，HashMap采用**数组+链表+红黑树实现**，当链表长度超过阈值（8）时，将链表(查询时间复杂度为O(n))转换为红黑树(时间复杂度为O(lg n))，极大的提高了查询效率。以下没有特别说明的均为JDK1.8中的HashMap。

##### 特点

​		HashMap 可以说是我们使用最多的 Map 集合，它有以下特点：

- 键不可重复，值可以重复
- 底层哈希表
- 线程不安全
- 允许key为null，value也可以为null

##### 数据结构

​		在Java中，保存数据有两种比较简单的数据结构：数组和链表。数组的特点是：寻址容易，插入和删除困难；链表的特点是：寻址困难，但插入和删除容易；所以我们将数组和链表结合在一起，发挥两者各自的优势，使用一种叫做拉链法的方式可以解决哈希冲突。

**JDK1.8之前**

​		JDK1.8之前采用的是拉链法。拉链法：将链表和数组相结合。也就是说创建一个链表数组，数组中每一格就是一个链表。若遇到哈希冲突，则将冲突的值加到链表中即可。

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200305231409179.png" alt="image-20200305231409179" style="zoom:50%;" />

**JDK1.8之后**

​		相比于之前的版本，jdk1.8在解决哈希冲突时有了较大的变化，当**链表长度大于阈值（默认为8）时**，数组初始化容量**默认为16**，将链表转化为红黑树，以减少搜索时间。

![image-20200305231352753](/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200305231352753.png)

**JDK1.7 VS JDK1.8 比较**

JDK1.8主要解决或优化了一下问题：

- resize 扩容优化
- 引入了红黑树，目的是避免单条链表过长而影响查询效率，红黑树算法请参考
- 解决了多线程死循环问题，但仍是非线程安全的，多线程时可能会造成数据丢失问题。

![image-20200305231518724](/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200305231518724.png)

##### 继承关系图

![HashMap继承关系图](/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/format,png.png)

HashMap继承抽象类AbstractMap，实现Map接口。除此之外，它还实现了两个标识型接口，这两个接口都没有任何方法，仅作为标识表示实现类具备某项功能。`Cloneable`表示实现类支持克隆，`java.io.Serializable`则表示支持序列化。

##### 成员变量

```java
//默认初始化Node数组容量16
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4;
//最大的数组容量
static final int MAXIMUM_CAPACITY = 1 << 30;
//默认负载因子0.75
static final float DEFAULT_LOAD_FACTOR = 0.75f;
//由链表转红黑树的临界值
static final int TREEIFY_THRESHOLD = 8;
//由红黑树转链表的临界值
static final int UNTREEIFY_THRESHOLD = 6;
//桶转化为树形结构的最小容量
static final int MIN_TREEIFY_CAPACITY = 64;
//HashMap结构修改的次数，结构修改是指更改HashMap中的映射数或以其他方式修改其内部结构(例如，rehash的修改)。该字段用于在Collection-views上快速生成迭代器。
transient int modCount;  
//Node数组下一次扩容的临界值，第一次为16*0.75=12（容量*负载因子）
int threshold;
//负载因子
final float loadFactor;
//map中包含的键值对的数量
transient int size;
//表数据，即Node键值对数组，Node是单向链表，它实现了Map.Entry接口，总是2的幂次倍
//Node<K,V>是HashMap的内部类，实现Map.Entry<K,V>接口，HashMap的哈希桶数组中存放的键值对对象就是Node<K,V>。类中维护了一个next指针指向链表中的下一个元素。值得注意的是，当链表中的元素数量超过TREEIFY_THRESHOLD后会HashMap会将链表转换为红黑树，此时该下标的元素将成为TreeNode<K,V>,继承于LinkedHashMap.Entry<K,V>，而LinkedHashMap.Entry<K,V>是Node<K,V>的子类，因此HashMap的底层数组数据类型即为Node<K,V>。
transient Node<K,V>[] table;
//存放具体元素的集,可用于遍历map集合
transient Set<Map.Entry<K,V>> entrySet;
```

capacity、threshold和loadFactor之间的关系：

- capacity table的容量，默认容量是16
- threshold table扩容的临界值
- loadFactor 负载因子，一般 threshold = capacity * loadFactor，默认的负载因子0.75是对空间和时间效率的一个平衡选择，建议大家不要修改。

##### 构造方法

```java
//初始化容量以及负载因子
public HashMap(int initialCapacity, float loadFactor) {
    //判断初始化数组的容量大小
    if (initialCapacity < 0)
        throw new IllegalArgumentException("Illegal initial capacity: " +
                                           initialCapacity);
    if (initialCapacity > MAXIMUM_CAPACITY)
        initialCapacity = MAXIMUM_CAPACITY;
    //判断初始化的负载因子大小和是否为浮点型
    if (loadFactor <= 0 || Float.isNaN(loadFactor))
        throw new IllegalArgumentException("Illegal load factor: " +
                                           loadFactor);
    //初始化负载因子
    this.loadFactor = loadFactor;
    this.threshold = tableSizeFor(initialCapacity);
}  

//初始化容量
public HashMap(int initialCapacity) {  
    this(initialCapacity, DEFAULT_LOAD_FACTOR);  
}  

//默认构造方法
public HashMap() {  
    this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted  
}  
 
//把另一个Map的值映射到当前新的Map中
public HashMap(Map<? extends K, ? extends V> m) {  
    this.loadFactor = DEFAULT_LOAD_FACTOR;  
    putMapEntries(m, false);  
}  
```

其中主要有两种形式：

​		定义初始容量大小（table数组的大小，缺省值为16），定义负载因子（缺省值为0.75）的形式，直接拷贝别的HashMap的形式，在此不作讨论。值得注意的是，当我们自定义HashMap初始容量大小时，构造函数并非直接把我们定义的数值当做HashMap容量大小，而是把该数值当做参数调用方法tableSizeFor，然后把返回值作为HashMap的初始容量大小

##### 核心方法

###### hash()算法

​		JDK1.8 之前 HashMap 底层是 数组和链表 结合在一起使用。HashMap 通过 key 的 hashCode 经过扰动函数处理过后得到 hash 值，然后通过 (n - 1) & hash 判断当前元素存放的位置（这里的 n 指的是数组的长度），如果当前位置存在元素的话，就判断该元素与要存入的元素的 hash 值以及 key 是否相同，如果相同的话，直接覆盖，不相同就通过拉链法解决冲突。

​		所谓扰动函数指的就是 HashMap 的 hash 方法。使用 hash 方法是为了防止一些实现比较差的 hashCode() 方法，换句话说使用扰动函数之后可以减少碰撞。

JDK 1.8 HashMap 的 hash 方法源码:

JDK 1.8 的 hash方法 相比于 JDK 1.7 hash 方法更加简化，但是原理不变。

```java
// 取key的hashCode值、高位运算、取模运算
// 在JDK1.8的实现中，优化了高位运算的算法，
// 通过hashCode()的高16位异或低16位实现的：(h = k.hashCode()) ^ (h >>> 16)，
// 主要是从速度、功效、质量来考虑的，这么做可以在数组table的length比较小的时候，
// 也能保证考虑到高低Bit都参与到Hash的计算中，同时不会有太大的开销。
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

（1）首先获取对象的hashCode()值，然后将hashCode值右移16位，然后将右移后的值与原来的hashCode做异或运算，返回结果。（其中h>>>16，在JDK1.8中，优化了高位运算的算法，使用了零扩展，无论正数还是负数，都在高位插入0）。

（2）在putVal源码中，通过(n-1)&hash获取该对象的键在hashmap中的位置。（其中hash的值就是（1）中获得的值）其中n表示的是hash桶数组的长度，并且该长度为2的n次方，这样(n-1)&hash就等价于hash%n。因为&运算的效率高于%运算。

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                boolean evict) {
    ...

    if ((p = tab[i = (n - 1) & hash]) == null)//获取位置
        tab[i] = newNode(hash, key, value, null);
    ...
}
```

​		tab即是table，n是map集合的容量大小，hash是上面方法的返回值。因为通常声明map集合时不会指定大小，或者初始化的时候就创建一个容量很大的map对象，所以这个通过容量大小与key值进行hash的算法在开始的时候只会对低位进行计算，虽然容量的2进制高位一开始都是0，但是key的2进制高位通常是有值的，因此先在hash方法中将key的hashCode右移16位在与自身异或，使得高位也可以参与hash，更大程度上减少了碰撞率。

下面举例说明下，n为table的长度。


![image-20200305232326677](/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200305232326677.png)

相比于 JDK1.8 的 hash 方法 ，JDK 1.7 的 hash 方法的性能会稍差一点点，因为毕竟扰动了 4 次。

###### put()方法

​		当我们put的时候，首先计算 key的hash值，这里调用了 hash方法，hash方法实际是让key.hashCode()与key.hashCode()>>>16进行异或操作，高16bit补0，一个数和0异或不变，所以 hash 函数大概的作用就是：高16bit不变，低16bit和高16bit做了一个异或，目的是减少碰撞。按照函数注释，因为bucket数组大小是2的幂，计算下标index = (table.length - 1) & hash，如果不做 hash 处理，相当于散列生效的只有几个低 bit 位，为了减少散列的碰撞，设计者综合考虑了速度、作用、质量之后，使用高16bit和低16bit异或来简单处理减少碰撞，而且JDK8中用了复杂度 O（logn）的树结构来提升碰撞下的性能。

**putVal方法执行流程图**
![image-20200305232726735](/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200305232726735.png)

###### get()方法

说明：HashMap同样并没有直接提供getNode接口给用户调用，而是提供的get方法，而get方法就是通过getNode来取得元素的。

```java
public V get(Object key) {
    Node<k,v> e;
    return (e = getNode(hash(key), key)) == null ? null : e.value;
}
```

###### remove()方法

```Java 
/**
* 从HashMap中删除掉指定key对应的键值对，并返回被删除的键值对的值
* 如果返回空，说明key可能不存在，也可能key对应的值就是null
* 如果想确定到底key是否存在可以使用containsKey方法
*/
public V remove(Object key) {
    Node<K,V> e; // 定义一个节点变量，用来存储要被删除的节点（键值对）
    return (e = removeNode(hash(key), key, null, false, true)) == null ?
        null : e.value; // 调用removeNode方法
}
```

###### 遍历

HashMap的四种遍历方式

```Java 
//HashMap的四种遍历方式
public static void main(String[] args) {
    Map<String, String> map = new HashMap<String, String>();
    map.put("1", "value1");
    map.put("2", "value2");
    map.put("3", "value3");
    map.put("4", "value4");

    //第一种   通过Map.entrySet遍历,推荐使用,尤其是容量大时
    System.out.println("通过Map.entrySet遍历key和value: ");
    for (Map.Entry<String, String> entry : map.entrySet()) {
        System.out.println("Key: " + entry.getKey() + " - Value: " + entry.getValue());
    }

    //第二种   通过Map.entrySet使用iterator遍历
    System.out.println("\n通过Map.entrySet使用iterator遍历key和value: ");
    Iterator map1it = map.entrySet().iterator();
    while (map1it.hasNext()) {
        Map.Entry<String, String> entry = (Map.Entry<String, String>) map1it.next();
        System.out.println("Key: " + entry.getKey() + " - Value: " + entry.getValue());
    }

    //第三种   通过Map.keySet遍历,二次取值
    System.out.println("\n通过Map.keySet遍历key和value: ");
    for (String key : map.keySet()) {
        System.out.println("Key: " + key + " - Value: " + map.get(key));
    }

    //第四种   通过Map.values()遍历
    System.out.println("\n通过Map.values()遍历所有的value,但不能遍历key: ");
    for (String v : map.values()) {
        System.out.println("The value is " + v);
    }
}
```



#### LinkedHashMap(JDK1.8)源码解析

##### 概述

​		LinkedHashMap 继承自 HashMap，在 HashMap 基础上，通过维护**一条双向链表**，解决了 HashMap 不能随时保持遍历顺序和插入顺序一致的问题。除此之外，LinkedHashMap 对访问顺序也提供了相关支持。在一些场景下，该特性很有用，比如缓存。在实现上，LinkedHashMap **很多方法直接继承自 HashMap**，仅为维护双向链表覆写了部分方法。所以，要看懂 LinkedHashMap 的源码，需要先看懂 HashMap 的源码。

​		本篇文章的结构与我之前关于 Java 集合类的源码分析文章不同，本文将不再分析集合类的基本操作（查找、遍历、插入、删除），而是把重点放在双向链表的维护上。包括链表的建立过程，删除节点的过程，以及访问顺序维护的过程等。

##### 原理

​		上一章说了 LinkedHashMap 继承自 HashMap，所以它的底层仍然是**基于拉链式散列结构**。该结构由数组和链表或红黑树组成.每当有新键值对节点插入，新节点最终会接在 tail 引用指向的节点后面。而 tail 引用则会移动到新的节点上，这样一个双向链表就建立起来了。

##### 链表的建立过程

​		链表的建立过程是在插入键值对节点时开始的，初始情况下，让 LinkedHashMap 的 head 和 tail 引用同时指向新节点，链表就算建立起来了。随后不断有新节点插入，通过将新节点接在 tail 引用指向节点的后面，即可实现链表的更新。

​		Map 类型的集合类是通过 put(K,V) 方法插入键值对，LinkedHashMap 本身并没有覆写父类的 put 方法，而是直接使用了父类的实现。但在 HashMap 中，put 方法插入的是 HashMap 内部类 Node 类型的节点，该类型的节点并不具备与 LinkedHashMap 内部类 Entry 及其子类型节点组成链表的能力。那么，LinkedHashMap 是怎样建立链表的呢？在展开说明之前，我们先看一下 LinkedHashMap 插入操作相关的代码：

```java
// HashMap 中实现
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}

// HashMap 中实现
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    if ((tab = table) == null || (n = tab.length) == 0) {...}
    // 通过节点 hash 定位节点所在的桶位置，并检测桶中是否包含节点引用
    if ((p = tab[i = (n - 1) & hash]) == null) {...}
    else {
        Node<K,V> e; K k;
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        else if (p instanceof TreeNode) {...}
        else {
            // 遍历链表，并统计链表长度
            for (int binCount = 0; ; ++binCount) {
                // 未在单链表中找到要插入的节点，将新节点接在单链表的后面
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    if (binCount >= TREEIFY_THRESHOLD - 1) {...}
                    break;
                }
                // 插入的节点已经存在于单链表中
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        }
        if (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null) {...}
            afterNodeAccess(e);    // 回调方法，后续说明
            return oldValue;
        }
    }
    ++modCount;
    if (++size > threshold) {...}
    afterNodeInsertion(evict);    // 回调方法，后续说明
    return null;
}

// HashMap 中实现
Node<K,V> newNode(int hash, K key, V value, Node<K,V> next) {
    return new Node<>(hash, key, value, next);
}

// LinkedHashMap 中覆写
Node<K,V> newNode(int hash, K key, V value, Node<K,V> e) {
    LinkedHashMap.Entry<K,V> p =
        new LinkedHashMap.Entry<K,V>(hash, key, value, e);
    // 将 Entry 接在双向链表的尾部
    linkNodeLast(p);
    return p;
}

// LinkedHashMap 中实现
private void linkNodeLast(LinkedHashMap.Entry<K,V> p) {
    LinkedHashMap.Entry<K,V> last = tail;
    tail = p;
    // last 为 null，表明链表还未建立
    if (last == null)
        head = p;
    else {
        // 将新节点 p 接在链表尾部
        p.before = last;
        last.after = p;
    }
}
```

##### 链表节点的删除过程

​		与插入操作一样，LinkedHashMap 删除操作相关的代码也是直接用父类的实现。在删除节点时，父类的删除逻辑并不会修复 LinkedHashMap 所维护的双向链表，这不是它的职责。那么删除及节点后，被删除的节点该如何从双链表中移除呢？当然，办法还算是有的。上一节最后提到 HashMap 中三个回调方法运行 LinkedHashMap 对一些操作做出响应。所以，在删除及节点后，回调方法 `afterNodeRemoval` 会被调用。LinkedHashMap 覆写该方法，并在该方法中完成了移除被删除节点的操作。相关源码如下：

```Java 
// HashMap 中实现
public V remove(Object key) {
    Node<K,V> e;
    return (e = removeNode(hash(key), key, null, false, true)) == null ?
        null : e.value;
}

// HashMap 中实现
final Node<K,V> removeNode(int hash, Object key, Object value,
                           boolean matchValue, boolean movable) {
    Node<K,V>[] tab; Node<K,V> p; int n, index;
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (p = tab[index = (n - 1) & hash]) != null) {
        Node<K,V> node = null, e; K k; V v;
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            node = p;
        else if ((e = p.next) != null) {
            if (p instanceof TreeNode) {...}
            else {
                // 遍历单链表，寻找要删除的节点，并赋值给 node 变量
                do {
                    if (e.hash == hash &&
                        ((k = e.key) == key ||
                         (key != null && key.equals(k)))) {
                        node = e;
                        break;
                    }
                    p = e;
                } while ((e = e.next) != null);
            }
        }
        if (node != null && (!matchValue || (v = node.value) == value ||
                             (value != null && value.equals(v)))) {
            if (node instanceof TreeNode) {...}
            // 将要删除的节点从单链表中移除
            else if (node == p)
                tab[index] = node.next;
            else
                p.next = node.next;
            ++modCount;
            --size;
            afterNodeRemoval(node);    // 调用删除回调方法进行后续操作
            return node;
        }
    }
    return null;
}

// LinkedHashMap 中覆写
void afterNodeRemoval(Node<K,V> e) { // unlink
    LinkedHashMap.Entry<K,V> p =
        (LinkedHashMap.Entry<K,V>)e, b = p.before, a = p.after;
    // 将 p 节点的前驱后后继引用置空
    p.before = p.after = null;
    // b 为 null，表明 p 是头节点
    if (b == null)
        head = a;
    else
        b.after = a;
    // a 为 null，表明 p 是尾节点
    if (a == null)
        tail = b;
    else
        a.before = b;
}
```

删除的过程并不复杂，上面这么多代码其实就做了三件事：

1. 根据 hash 定位到桶位置
2. 遍历链表或调用红黑树相关的删除方法
3. 从 LinkedHashMap 维护的双链表中移除要删除的节点

##### 基于 LinkedHashMap 实现缓存

​		前面介绍了 LinkedHashMap 是如何维护插入和访问顺序的，大家对 LinkedHashMap 的原理应该有了一定的认识。本节我们来写一些代码实践一下，这里通过继承 LinkedHashMap 实现了一个简单的 LRU 策略的缓存。在写代码之前，先介绍一下前置知识。

在3.1节分析链表建立过程时，我故意忽略了部分源码分析。本节就把忽略的部分补上，先看源码吧：

```java
void afterNodeInsertion(boolean evict) { // possibly remove eldest
    LinkedHashMap.Entry<K,V> first;
    // 根据条件判断是否移除最近最少被访问的节点
    if (evict && (first = head) != null && removeEldestEntry(first)) {
        K key = first.key;
        removeNode(hash(key), key, null, false, true);
    }
}

// 移除最近最少被访问条件之一，通过覆盖此方法可实现不同策略的缓存
protected boolean removeEldestEntry(Map.Entry<K,V> eldest) {
    return false;
}
```

​		上面的源码的核心逻辑在一般情况下都不会被执行，所以之前并没有进行分析。上面的代码做的事情比较简单，就是通过一些条件，判断是否移除最近最少被访问的节点。看到这里，大家应该知道上面两个方法的用途了。当我们基于 LinkedHashMap 实现缓存时，通过覆写`removeEldestEntry`方法可以实现自定义策略的 LRU 缓存。比如我们可以根据节点数量判断是否移除最近最少被访问的节点，或者根据节点的存活时间判断是否移除该节点等。本节所实现的缓存是基于判断节点数量是否超限的策略。在构造缓存对象时，传入最大节点数。当插入的节点数超过最大节点数时，移除最近最少被访问的节点。实现代码如下：

```java
public class SimpleCache<K, V> extends LinkedHashMap<K, V> {

    private static final int MAX_NODE_NUM = 100;

    private int limit;

    public SimpleCache() {
        this(MAX_NODE_NUM);
    }

    public SimpleCache(int limit) {
        super(limit, 0.75f, true);
        this.limit = limit;
    }

    public V save(K key, V val) {
        return put(key, val);
    }

    public V getOne(K key) {
        return get(key);
    }

    public boolean exists(K key) {
        return containsKey(key);
    }
    
    /**
     * 判断节点数是否超限
     * @param eldest
     * @return 超限返回 true，否则返回 false
     */
    @Override
    protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
        return size() > limit;
    }
}
```

测试代码如下：

```java
public class SimpleCacheTest {

    @Test
    public void test() throws Exception {
        SimpleCache<Integer, Integer> cache = new SimpleCache<>(3);

        for (int i = 0; i < 10; i++) {
            cache.save(i, i * i);
        }

        System.out.println("插入10个键值对后，缓存内容：");
        System.out.println(cache + "\n");

        System.out.println("访问键值为7的节点后，缓存内容：");
        cache.getOne(7);
        System.out.println(cache + "\n");

        System.out.println("插入键值为1的键值对后，缓存内容：");
        cache.save(1, 1);
        System.out.println(cache);
    }
}
```

输出结果

```java
插入10个键值对后，缓存内容：
{7=49, 8=64, 9=81}

访问键值为7的节点后，缓存内容：
{8=64, 9=81, 7=49}

插入键值为1的键值对后，缓存内容：
{9=81, 7=49, 1=1}
```

​		在测试代码中，设定缓存大小为3。在向缓存中插入10个键值对后，只有最后3个被保存下来了，其他的都被移除了。然后通过访问键值为7的节点，使得该节点被移到双向链表的最后位置。当我们再次插入一个键值对时，键值为7的节点就不会被移除。



#### ConcurrentHashMap(JDK1.8)源码解析

##### put操作

```Java
  public  V put(K key, V value) {  

  return  putVal(key, value,  false    );  

  }  

  /** Implementation for put and putIfAbsent */  

  final  V putVal(K key, V value,  boolean  onlyIfAbsent) {  

  if  (key ==  null  || value ==  null    )  throw  new  NullPointerException();  

  int  hash = spread(key.hashCode());  //两次hash，减少hash冲突，可以均匀分布  

  int  binCount =  0    ;  

  for  (Node<K,V>[] tab = table;;) {  //对这个table进行迭代  

  Node<K,V> f;  int  n, i, fh;  

  //这里就是上面构造方法没有进行初始化，在这里进行判断，为null就调用initTable进行初始化，属于懒汉模式初始化  

  if  (tab ==  null  || (n = tab.length) ==  0    )  

  tab = initTable();  

  else  if  ((f = tabAt(tab, i = (n -  1    ) & hash)) ==  null    ) {    //如果i位置没有数据，就直接无锁插入  

  if  (casTabAt(tab, i,  null    ,  

  new  Node<K,V>(hash, key, value,  null    )))  

  break    ;  // no lock when adding to empty bin  

  }  

  else  if  ((fh = f.hash) == MOVED)    //如果在进行扩容，则先进行扩容操作  

  tab = helpTransfer(tab, f);  

  else  {  

  V oldVal =  null    ;  

  //如果以上条件都不满足，那就要进行加锁操作，也就是存在hash冲突，锁住链表或者红黑树的头结点  

  synchronized  (f) {  

  if  (tabAt(tab, i) == f) {  

  if  (fh >=  0    ) {  //表示该节点是链表结构  

  binCount =  1    ;  

  for  (Node<K,V> e = f;; ++binCount) {  

  K ek;  

  //这里涉及到相同的key进行put就会覆盖原先的value  

  if  (e.hash == hash &&  

  ((ek = e.key) == key ||  

  (ek !=  null  && key.equals(ek)))) {  

  oldVal = e.val;  

  if  (!onlyIfAbsent)  

  e.val = value;  

  break    ;  

  }  

  Node<K,V> pred = e;  

  if  ((e = e.next) ==  null    ) {  //插入链表尾部  

  pred.next =  new  Node<K,V>(hash, key,  

  value,  null    );  

  break    ;  

  }  

  }  

  }  

  else  if  (f  instanceof  TreeBin) {    //红黑树结构  

  Node<K,V> p;  

  binCount =  2    ;  

  //红黑树结构旋转插入  

  if  ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,  

  value)) !=  null    ) {  

  oldVal = p.val;  

  if  (!onlyIfAbsent)  

  p.val = value;  

  }  

  }  

  }  

  }  

  if  (binCount !=  0    ) {  //如果链表的长度大于8时就会进行红黑树的转换  

  if  (binCount >= TREEIFY_THRESHOLD)  

  treeifyBin(tab, i);  

  if  (oldVal !=  null    )  

  return  oldVal;  

  break    ;  

  }  

  }  

  }  

  addCount(1L, binCount);    //统计size，并且检查是否需要扩容  

  return  null    ;  

  }  


```

**put的过程很清晰，对当前的table进行无条件自循环直到put成功，可以分成以下六步流程来概述**

1.  如果没有初始化就先调用initTable（）方法来进行初始化过程
2.  如果**没有hash冲突就直接CAS插入**
3.  如果还在进行扩容操作**就先进行扩容**
4.  如果存在hash冲突，就**加锁来保证线程安全**，这里有两种情况，一种是链表形式就直接遍历到尾端插入，一种是红黑树就**按照红黑树结构插入**，
5.  最后一个如果该链表的数量大于阈值8，就要先转换成黑红树的结构，break再一次进入循环
6.  如果添加成功就调用addCount（）方法统计size，并且检查是否需要扩容

##### get操作

```Java
  public  V get(Object key) {  

  Node<K,V>[] tab; Node<K,V> e, p;  int  n, eh; K ek;  

  int  h = spread(key.hashCode());  //计算两次hash  

  if  ((tab = table) !=  null  && (n = tab.length) >  0  &&  

  (e = tabAt(tab, (n -  1    ) & h)) !=  null    ) {    //读取首节点的Node元素  

  if  ((eh = e.hash) == h) {  //如果该节点就是首节点就返回  

  if  ((ek = e.key) == key || (ek !=  null  && key.equals(ek)))  

  return  e.val;  

  }  

  //hash值为负值表示正在扩容，这个时候查的是ForwardingNode的find方法来定位到nextTable来  

  //查找，查找到就返回  

  else  if  (eh <  0    )  

  return  (p = e.find(h, key)) !=  null  ? p.val :  null    ;  

  while  ((e = e.next) !=  null    ) {    //既不是首节点也不是ForwardingNode，那就往下遍历  

  if  (e.hash == h &&  

  ((ek = e.key) == key || (ek !=  null  && key.equals(ek))))  

  return  e.val;  

  }  

  }  

  return  null    ;  

  }  

```

​		ConcurrentHashMap的get操作的流程很简单，也很清晰，可以分为三个步骤来描述

1.  计算hash值，定位到该table索引位置，如果是首节点符合就返回
2.  如果遇到扩容的时候，会调用标志正在扩容节点ForwardingNode的find方法，查找该节点，匹配就返回
3.  以上都不符合的话，就往下遍历节点，匹配就返回，否则最后就返回null

##### 总结与思考

​		其实可以看出JDK1.8版本的ConcurrentHashMap的数据结构已经接近HashMap，相对而言，ConcurrentHashMap只是**增加了同步的操作来控制并发**，从JDK1.7版本的**ReentrantLock+Segment+HashEntry**，到JDK1.8版本中**synchronized+CAS+HashEntry+红黑树**。

1.  JDK1.8的实现**降低锁的粒度**，JDK1.7版本锁的粒度是**基于Segment的**，**包含多个HashEntry**，而JDK1.8锁的粒度就是**HashEntry（首节点）**
2.  JDK1.8版本的数据结构变得更加简单，使得操作也更加清晰流畅，因为已经使用synchronized来进行同步，所以**不需要分段锁的概念**，也就不需要Segment这种数据结构了，由于粒度的降低，实现的复杂度也增加了
3.  JDK1.8使用红黑树来优化链表，基于长度很长的链表的遍历是一个很漫长的过程，而红黑树的遍历效率是很快的，代替一定阈值的链表，这样形成一个最佳拍档
4.  JDK1.8为什么使用**内置锁synchronized来代替重入锁ReentrantLock**，我觉得有以下几点
    1.  因为**粒度降低了**，在相对而言的低粒度加锁方式，synchronized并不比ReentrantLock差，在粗粒度加锁中ReentrantLock可能通过Condition来控制各个低粒度的边界，更加的灵活，而在低粒度中，Condition的优势就没有了
    2.  JVM的开发团队从来都没有放弃synchronized，而且**基于JVM的synchronized优化空间更大**，使用内嵌的关键字比使用API更加自然
    3.  在大量的数据操作下，对于JVM的内存压力，**基于API的ReentrantLock会开销更多的内存**，虽然不是瓶颈，但是也是一个选择依据



#### TreeMap(JDK1.8)源码解析

##### 简介

`TreeMap`最早出现在`JDK 1.2`中，是 Java 集合框架中比较重要一个的实现。TreeMap 底层基于`红黑树`实现，可保证在`log(n)`时间复杂度内完成 **containsKey、get、put 和 remove 操作**，效率很高。另一方面，由于 TreeMap 基于红黑树实现，这为 TreeMap 保持键的**有序性**打下了基础。总的来说，TreeMap 的核心是红黑树，其很多方法也是对红黑树增删查基础操作的一个包装。所以只要弄懂了红黑树，TreeMap 就没什么秘密了。

##### 概览

`TreeMap`继承自`AbstractMap`，并实现了 `NavigableMap`接口。NavigableMap 接口继承了`SortedMap`接口，SortedMap 最终继承自`Map`接口，同时 AbstractMap 类也实现了 Map 接口。

##### 查找

`TreeMap`基于红黑树实现，而红黑树是一种自平衡二叉查找树，所以 TreeMap 的查找操作流程和二叉查找树一致。二叉树的查找流程是这样的，先将目标值和根节点的值进行比较，如果目标值小于根节点的值，则再和根节点的左孩子进行比较。如果目标值大于根节点的值，则继续和根节点的右孩子比较。在查找过程中，如果目标值和二叉树中的某个节点值相等，则返回 true，否则返回 false。TreeMap 查找和此类似，只不过在 TreeMap 中，节点（Entry）存储的是键值对。在查找过程中，比较的是键的大小，返回的是值，如果没找到，则返回`null`。TreeMap 中的查找方法是`get`，具体实现在`getEntry`方法中，相关源码如下：

```java
public V get(Object key) {
    Entry<K,V> p = getEntry(key);
    return (p==null ? null : p.value);
}

final Entry<K,V> getEntry(Object key) {
    // Offload comparator-based version for sake of performance
    if (comparator != null)
        return getEntryUsingComparator(key);
    if (key == null)
        throw new NullPointerException();
    @SuppressWarnings("unchecked")
        Comparable<? super K> k = (Comparable<? super K>) key;
    Entry<K,V> p = root;
    
    // 查找操作的核心逻辑就在这个 while 循环里
    while (p != null) {
        int cmp = k.compareTo(p.key);
        if (cmp < 0)
            p = p.left;
        else if (cmp > 0)
            p = p.right;
        else
            return p;
    }
    return null;
}
```

查找操作的核心逻辑就是`getEntry`方法中的`while`循环，大家对照上面的说的流程，自己看一下吧，比较简单，就不多说了。

##### 遍历

遍历操作也是大家使用频率较高的一个操作，对于`TreeMap`，使用方式一般如下：

```java
for(Object key : map.keySet()) {
    // do something
}
```

或

```java
for(Map.Entry entry : map.entrySet()) {
    // do something
}
```

从上面代码片段中可以看出，大家一般都是对 TreeMap 的 key 集合或 Entry 集合进行遍历。上面代码片段中用 foreach 遍历keySet 方法产生的集合，在编译时会转换成用迭代器遍历，等价于：

```java
Set keys = map.keySet();
Iterator ite = keys.iterator();
while (ite.hasNext()) {
    Object key = ite.next();
    // do something
}
```

##### 插入

相对于前两个操作，插入操作明显要复杂一些。当往 TreeMap 中放入新的键值对后，可能会破坏红黑树的性质。这里为了描述方便，把 Entry 称为节点。并把新插入的节点称为`N`，N 的父节点为`P`。P 的父节点为`G`，且 P 是 G 的左孩子。P 的兄弟节点为`U`。在往红黑树中插入新的节点 N 后（新节点为红色），会产生下面5种情况：

1. N 是根节点
2. N 的父节点是黑色
3. N 的父节点是红色，叔叔节点也是红色
4. N 的父节点是红色，叔叔节点是黑色，且 N 是 P 的右孩子
5. N 的父节点是红色，叔叔节点是黑色，且 N 是 P 的左孩子

上面5种情况中，情况2不会破坏红黑树性质，所以无需处理。情况1 会破坏红黑树性质2（根是黑色），情况3、4、和5会破坏红黑树性质4（每个红色节点必须有两个黑色的子节点）。这个时候就需要进行调整，以使红黑树重新恢复平衡。至于怎么调整，可以参考红黑树详细分析，这里不再重复说明。接下来分析一下插入操作相关源码：

```java
public V put(K key, V value) {
    Entry<K,V> t = root;
    // 1.如果根节点为 null，将新节点设为根节点
    if (t == null) {
        compare(key, key);
        root = new Entry<>(key, value, null);
        size = 1;
        modCount++;
        return null;
    }
    int cmp;
    Entry<K,V> parent;
    // split comparator and comparable paths
    Comparator<? super K> cpr = comparator;
    if (cpr != null) {
        // 2.为 key 在红黑树找到合适的位置
        do {
            parent = t;
            cmp = cpr.compare(key, t.key);
            if (cmp < 0)
                t = t.left;
            else if (cmp > 0)
                t = t.right;
            else
                return t.setValue(value);
        } while (t != null);
    } else {
        // 与上面代码逻辑类似，省略
    }
    Entry<K,V> e = new Entry<>(key, value, parent);
    // 3.将新节点链入红黑树中
    if (cmp < 0)
        parent.left = e;
    else
        parent.right = e;
    // 4.插入新节点可能会破坏红黑树性质，这里修正一下
    fixAfterInsertion(e);
    size++;
    modCount++;
    return null;
}
```

put 方法代码如上，逻辑和二叉查找树插入节点逻辑一致。重要的步骤我已经写了注释，并不难理解。插入逻辑的复杂之处在于插入后的修复操作，对应的方法`fixAfterInsertion`。

到这里，插入操作就讲完了。接下来，来说说 TreeMap 中最复杂的部分，也就是删除操作了。

##### 删除

删除操作是红黑树最复杂的部分，原因是该操作可能会破坏红黑树性质5（从任一节点到其每个叶子的所有简单路径都包含相同数目的黑色节点），修复性质5要比修复其他性质（性质2和4需修复，性质1和3不用修复）复杂的多。当删除操作导致性质5被破坏时，会出现8种情况。为了方便表述，这里还是先做一些假设。我们把`最终被删除`的节点称为 X，X 的替换节点称为 N。N 的父节点为`P`，且 N 是 P 的左孩子。N 的兄弟节点为`S`，S 的左孩子为 SL，右孩子为 SR。这里特地强调 X 是 `最终被删除` 的节点，是原因**二叉查找树会把要删除有两个孩子的节点的情况转化为删除只有一个孩子的节点的情况**，该节点是欲被删除节点的前驱和后继。

接下来，简单列举一下删除节点时可能会出现的情况，先列举较为简单的情况：

1. 最终被删除的节点 X 是红色节点
2. X 是黑色节点，但该节点的孩子节点是红色

比较复杂的情况：

1. 替换节点 N 是新的根
2. N 为黑色，N 的兄弟节点 S 为红色，其他节点为黑色。
3. N 为黑色，N 的父节点 P，兄弟节点 S 和 S 的孩子节点均为黑色。
4. N 为黑色，P 是红色，S 和 S 孩子均为黑色。
5. N 为黑色，P 可红可黑，S 为黑色，S 的左孩子 SL 为红色，右孩子 SR 为黑色
6. N 为黑色，P 可红可黑，S 为黑色，SR 为红色，SL 可红可黑

上面列举的8种情况中，前两种处理起来比较简单，后6种情况中情况26较为复杂。接下来我将会对情况26展开分析，删除相关的源码如下：

```java
public V remove(Object key) {
    Entry<K,V> p = getEntry(key);
    if (p == null)
        return null;

    V oldValue = p.value;
    deleteEntry(p);
    return oldValue;
}

private void deleteEntry(Entry<K,V> p) {
    modCount++;
    size--;

    /* 
     * 1. 如果 p 有两个孩子节点，则找到后继节点，
     * 并把后继节点的值复制到节点 P 中，并让 p 指向其后继节点
     */
    if (p.left != null && p.right != null) {
        Entry<K,V> s = successor(p);
        p.key = s.key;
        p.value = s.value;
        p = s;
    } // p has 2 children

    // Start fixup at replacement node, if it exists.
    Entry<K,V> replacement = (p.left != null ? p.left : p.right);

    if (replacement != null) {
        /*
         * 2. 将 replacement parent 引用指向新的父节点，
         * 同时让新的父节点指向 replacement。
         */ 
        replacement.parent = p.parent;
        if (p.parent == null)
            root = replacement;
        else if (p == p.parent.left)
            p.parent.left  = replacement;
        else
            p.parent.right = replacement;

        // Null out links so they are OK to use by fixAfterDeletion.
        p.left = p.right = p.parent = null;

        // 3. 如果删除的节点 p 是黑色节点，则需要进行调整
        if (p.color == BLACK)
            fixAfterDeletion(replacement);
    } else if (p.parent == null) { // 删除的是根节点，且树中当前只有一个节点
        root = null;
    } else { // 删除的节点没有孩子节点
        // p 是黑色，则需要进行调整
        if (p.color == BLACK)
            fixAfterDeletion(p);

        // 将 P 从树中移除
        if (p.parent != null) {
            if (p == p.parent.left)
                p.parent.left = null;
            else if (p == p.parent.right)
                p.parent.right = null;
            p.parent = null;
        }
    }
}
```

从源码中可以看出，`remove`方法只是一个简单的保证，核心实现在`deleteEntry`方法中。deleteEntry 主要做了这么几件事：

1. 如果待删除节点 P 有两个孩子，则先找到 P 的后继 S，然后将 S 中的值拷贝到 P 中，并让 P 指向 S
2. 如果最终被删除节点 P（P 现在指向最终被删除节点）的孩子不为空，则用其孩子节点替换掉
3. 如果最终被删除的节点是黑色的话，调用 fixAfterDeletion 方法进行修复

上面说了 replacement 不为空时，deleteEntry 的执行逻辑。上面说的略微啰嗦，如果简单说的话，7个字即可总结：`找后继 -> 替换 -> 修复`。这三步中，最复杂的是修复操作。













### Java异常综述

#### 异常概念与关键字

##### Java异常简介

​		Java异常是Java提供的一种识别及响应错误的**一致性机制**。
​		Java异常机制可以使程序中**异常处理代码**和**正常业务代码**分离，保证程序代码更加优雅，并提高程序健壮性。在有效使用异常的情况下，异常能清晰的回答what, where, why这3个问题：异常类型回答了“什么”被抛出，异常堆栈跟踪回答了“在哪”抛出，异常信息回答了“为什么”会抛出。

##### Java异常架构

![image-20200309181306175](/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200309181306175.png)

1. ###### Throwable

Throwable 是 Java 语言中所有**错误**与**异常**的超类。

Throwable 包含两个子类：**Error**（错误）和 **Exception**（异常），它们通常用于指示发生了异常情况。

Throwable 包含了其线程创建时线程执行堆栈的快照，它提供了 **printStackTrace() 等接口**用于获取堆栈跟踪数据等信息。

2. ###### Error（错误）

    ​	定义：Error 类及其子类。程序中无法处理的错误，表示运行应用程序中出现了**严重的错误**。

    ​	特点：此类错误一般表示代码运行时 **JVM 出现问题**。通常有 Virtual MachineError（虚拟机运行错误）、NoClassDefFoundError（类定义错误）等。比如 OutOfMemoryError：内存不足错误；StackOverflowError：栈溢出错误。此类错误发生时，JVM 将终止线程。

    ​	这些错误是**不受检异常**，非代码性错误。因此，当此类错误发生时，**应用程序不应该去处理此类错误**。按照Java惯例，我们是不应该实现任何新的Error子类的！

3. ###### Exception（异常）

    ​	程序**本身可以捕获并且可以处理的异常**。Exception 这种异常又分为两类：**运行时异常**和**编译时异常**。

**运行时异常**

​		定义：RuntimeException 类及其子类，表示 JVM 在运行期间可能出现的异常。

​		特点：Java 编译器不会检查它。也就是说，当程序中可能出现这类异常时，倘若既"没有通过throws声明抛出它"，也"没有用try-catch语句捕获它"，还是会编译通过。比如**NullPointerException**空指针异常、**ArrayIndexOutBoundException**数组下标越界异常、**ClassCastException**类型转换异常、**ArithmeticExecption**算术异常。此类异常属于**不受检异常**，一般是由程序逻辑错误引起的，**在程序中可以选择捕获处理，也可以不处理**。虽然 Java 编译器不会检查运行时异常，但是我们**也可以通过 throws 进行声明抛出**，也可以通过 try-catch 对它进行捕获处理。如果产生运行时异常，则需要**通过修改代码来进行避免**。例如，若会发生除数为零的情况，则需要通过代码避免该情况的发生！RuntimeException 异常会由 Java 虚拟机**自动抛出并自动捕获**（就算我们没写异常捕获语句运行时**也会抛出错误**！！！），此类异常的出现绝大数情况是代码本身有问题应该从逻辑上去解决并改进代码。

**编译时异常**

​		定义: Exception 中除 RuntimeException 及其子类之外的异常。

​		特点: Java 编译器会检查它。如果程序中出现此类异常，比如 **ClassNotFoundException**（没有找到指定的类异常），**IOException**（IO流异常），**要么通过throws进行声明抛出**，**要么通过try-catch进行捕获处理**，否则不能通过编译。在程序中，**通常不会自定义该类异常**，而是直接使用系统提供的异常类。该异常我们必须手动在代码里添加捕获语句来处理该异常。

4. ###### 受检异常与非受检异常

    ​	Java 的所有异常可以分为受检异常（checked exception）和非受检异常（unchecked exception）。

**受检异常**

​		编译器要求必须处理的异常。正确的程序在运行过程中，经常容易出现的、符合预期的异常情况。一旦发生此类异常，就必须采用某种方式进行处理。除 RuntimeException 及其子类外，其他的 Exception 异常都属于受检异常。编译器会检查此类异常，也就是说当编译器检查到应用中的某处可能会此类异常时，将会提示你处理本异常——要么使用try-catch捕获，**要么使用方法签名中用 throws 关键字抛出，否则编译不通过。**

**非受检异常**

​		编译器不会进行检查并且不要求必须处理的异常，也就说当程序中出现此类异常时，即使我们没有try-catch捕获它，也没有使用throws抛出该异常，编译也会正常通过。该类异常包括**运行时异常**（RuntimeException极其子类）和**错误**（Error）。

##### Java异常关键字

• try – 用于监听。将要被监听的代码(可能抛出异常的代码)放在try语句块之内，当try语句块内发生异常时，异常就被抛出。
• catch – 用于捕获异常。catch用来捕获try语句块中发生的异常。
• finally – finally语句块总是会被执行。它主要用于回收在try块里打开的物力资源(如数据库连接、网络连接和磁盘文件)。只有finally块，执行完成之后，才会回来执行try或者catch块中的return或者throw语句，如果finally中使用了return或者throw等终止方法的语句，则就不会跳回执行，直接停止。
• throw – 用于抛出异常。
• throws – 用在方法签名中，用于声明该方法可能抛出的异常。



#### Java异常处理流程

![image-20200309182750070](/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200309182750070.png)

​		Java 通过面向对象的方法进行异常处理，一旦方法抛出异常，系统自动根据该异常对象寻找合适异常处理器（Exception Handler）来处理该异常，把各种不同的异常进行分类，并提供了良好的接口。在 Java 中，每个异常都是一个对象，它是 Throwable 类或其子类的实例。当一个方法出现异常后**便抛出一个异常对象**，该对象中包含有异常信息，调用这个对象的方法可以捕获到这个异常并可以对其进行处理。Java 的异常处理是通过 5 个关键词来实现的：try、catch、throw、throws 和 finally。在Java应用中，异常的处理机制分为**声明异常**，**抛出异常**和**捕获异常**。

##### 声明异常

​		通常，应该捕获那些知道如何处理的异常，将不知道如何处理的异常继续传递下去。传递异常可以在方法签名处使用 throws 关键字声明可能会抛出的异常。

注意

- 非检查异常（Error、RuntimeException 或它们的子类）不可使用 throws 关键字来声明要抛出的异常。
- 一个方法出现编译时异常，就需要 try-catch/ throws 处理，否则会导致编译错误。

##### 抛出异常

​		如果你觉得解决不了某些异常问题，且不需要调用者处理，那么你可以抛出异常。

​		throw关键字作用是在方法内部抛出一个Throwable类型的异常。任何Java代码都可以通过throw语句抛出异常。

##### 捕获异常

​		程序通常在运行之前不报错，但是运行后可能会出现某些未知的错误，但是还不想直接抛出到上一级，那么就需要通过try…catch…的形式进行异常捕获，之后根据不同的异常情况来进行相应的处理。

##### 如何选择异常类型

​		可以根据下图来选择是捕获异常，声明异常还是抛出异常

![image-20200309183047208](/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200309183047208.png)

#### 常见异常处理方式

##### 直接抛出异常

​		通常，应该捕获那些知道如何处理的异常，将不知道如何处理的异常继续传递下去。传递异常可以在方法签名处使用 throws 关键字声明可能会抛出的异常。

```Java
private static void readFile(String filePath) throws IOException {
    File file = new File(filePath);
    String result;
    BufferedReader reader = new BufferedReader(new FileReader(file));
    while((result = reader.readLine())!=null) {
        System.out.println(result);
    }
    reader.close();
}
```

##### 封装异常再抛出

​		有时我们会从 catch 中抛出一个异常，目的是为了改变异常的类型。多用于在多系统集成时，当某个子系统故障，异常类型可能有多种，可以用统一的异常类型向外暴露，不需暴露太多内部异常细节。

```Java
private static void readFile(String filePath) throws MyException {    
    try {
        // code
    } catch (IOException e) {
        MyException ex = new MyException("read file failed.");
        ex.initCause(e);
        throw ex;
    }
}
```

##### 捕获异常

​		在一个 try-catch 语句块中可以捕获多个异常类型，并对不同类型的异常做出不同的处理

```Java
private static void readFile(String filePath) {
    try {
        // code
    } catch (FileNotFoundException e) {
        // handle FileNotFoundException
    } catch (IOException e){
        // handle IOException
    }
}
```

##### 自定义异常

​		习惯上，定义一个异常类应包含两个构造函数，一个无参构造函数和一个带有详细描述信息的构造函数（Throwable 的 toString 方法会打印这些详细信息，调试时很有用）

```Java
public class MyException extends Exception {
    public MyException(){ }
    public MyException(String msg){
        super(msg);
    }
    // ...
}
```

##### try-catch-finally

​		当方法中发生异常，异常处之后的代码不会再执行，如果之前获取了一些本地资源需要释放，则需要在方法正常结束时和 catch 语句中都调用释放本地资源的代码，显得代码比较繁琐，finally 语句可以解决这个问题。

```Java
private static void readFile(String filePath) throws MyException {
    File file = new File(filePath);
    String result;
    BufferedReader reader = null;
    try {
        reader = new BufferedReader(new FileReader(file));
        while((result = reader.readLine())!=null) {
            System.out.println(result);
        }
    } catch (IOException e) {
        System.out.println("readFile method catch block.");
        MyException ex = new MyException("read file failed.");
        ex.initCause(e);
        throw ex;
    } finally {
        System.out.println("readFile method finally block.");
        if (null != reader) {
            try {
                reader.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```


​		调用该方法时，读取文件时若发生异常，代码会进入 catch 代码块，之后进入 finally 代码块；若读取文件时未发生异常，则会跳过 catch 代码块直接进入 finally 代码块。所以无论代码中是否发生异常，fianlly 中的代码都会执行。若 catch 代码块中包含 return 语句，finally 中的代码还会执行吗？将以上代码中的 catch 子句修改如下：

```java 
catch (IOException e) {
    System.out.println("readFile method catch block.");
    return;
}
```

```
readFile method catch block.
readFile method finally block.
```


​		调用 readFile 方法，观察当 catch 子句中调用 return 语句时，finally 子句是否执行。可见，即使 catch 中包含了 return 语句，finally 子句依然会执行。若 finally 中也包含 return 语句，finally 中的 return 会覆盖前面的 return.

##### try-with-resource

​		上面例子中，finally 中的 close 方法也可能抛出 IOException, 从而覆盖了原始异常。JAVA 7 提供了更优雅的方式来实现资源的自动释放，自动释放的资源需要是实现了 AutoCloseable 接口的类。

```Java
private  static void tryWithResourceTest(){
    try (Scanner scanner = new Scanner(new FileInputStream("c:/abc"),"UTF-8")){
        // code
    } catch (IOException e){
        // handle exception
    }
}
```


​		try 代码块退出时，会自动调用 scanner.close 方法，和把 scanner.close 方法放在 finally 代码块中不同的是，若 scanner.close 抛出异常，则会被抑制，抛出的仍然为原始异常。被抑制的异常会由 addSusppressed 方法添加到原来的异常，如果想要获取被抑制的异常列表，可以调用 getSuppressed 方法来获取。





