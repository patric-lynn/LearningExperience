### Spring基础知识

#### 1.Spring概述

##### 1.什么是spring?

​		Spring是一个**轻量级Java开发框架**，最早有Rod Johnson创建，目的是为了解决**企业级应用开发**的业务逻辑层和其他各层的耦合问题。它是一个分层的JavaSE/JavaEE full-stack（一站式）轻量级开源框架，为开发Java应用程序提供全面的基础架构支持。Spring负责基础架构，因此Java开发者可以专注于应用程序的开发。

​		Spring最根本的使命是解决企业级应用开发的复杂性，即简化Java开发。

​		Spring可以做很多事情，它为企业级开发提供给了丰富的功能，但是这些功能的底层都依赖于它的两个核心特性，也就是**依赖注入**（dependency injection，DI）和**面向切面编程**（aspect-oriented programming，AOP）。

​		为了降低Java开发的复杂性，Spring采取了以下4种关键策略

- 基于**POJO**的轻量级和最小侵入性编程；
- 通过**依赖注入**和面向接口实现**松耦合**；
- 基于**切面和惯例**进行**声明式编程**；
- 通过**切面和模板**减少**样板式代码**。

##### 2.Spring有哪些应用场景

​		应用场景：JavaEE企业应用开发，包括**SSH、SSM**等

​		Spring价值：

- Spring是**非侵入式**的框架，目标是使应用程序代码对框架依赖最小化；
- Spring提供**一致的编程模型**，使应用直接使用POJO开发，与运行环境隔离开来；
- Spring推动应用设计风格向**面向对象**和**面向接口**开发转变，提高了代码的**重用性和可测试性**；

##### 3.Spring由哪些模块组成？

​			Spring 总共大约有 20 个模块， 由 1300 多个不同的文件构成。 而这些组件被分别整合在**核心容器（Core Container）** 、 **AOP（Aspect Oriented Programming）**和**设备支持（Instrmentation）** 、**数据访问与集成（Data Access/Integeration）** 、 **Web**、 **消息**（Messaging） 、 **Test**等 6 个模块中。 以下是 Spring 5 的模块结构图：

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200308110437683.png" alt="image-20200308110437683" style="zoom:30%;" />

​		**spring core**：提供了框架的基本组成部分，包括**控制反转（Inversion of Control，IOC）**和**依赖注入（Dependency Injection，DI）**功能。
​		**spring beans**：提供了BeanFactory，是**工厂模式的一个经典实现**，Spring将管理对象称为**Bean**。
​		**spring context**：构建于 core 封装包基础上的 **context 封装包**，提供了一种**框架式的对象访问方法**。
​		**spring jdbc**：提供了一个**JDBC的抽象层**，消除了烦琐的JDBC编码和数据库厂商特有的错误代码解析， 用于简化JDBC。
​		**spring aop**：提供了**面向切面**的编程实现，让你可以**自定义拦截器、切点**等。
​		**spring Web**：提供了针对 Web 开发的集成特性，例如文件上传，利用 servlet listeners 进行 **ioc 容器初始化**和针对 Web 的 **ApplicationContext**。
​		**spring test**：主要为测试提供支持的，支持使用JUnit或TestNG对Spring组件进行单元测试和集成测试。

##### 4.Spring 框架中都用到了哪些设计模式？

**工厂模式**：BeanFactory就是简单工厂模式的体现，用来创建对象的实例；
**单例模式**：Bean默认为**单例模式**。
**代理模式**：Spring的AOP功能用到了**JDK的动态代理**和**CGLIB字节码生成技术**；
**模板方法**：用来解**决代码重复的问题**。比如. RestTemplate, JmsTemplate, JpaTemplate。
**观察者模式**：定义对象键一种**一对多的依赖关系**，当一个对象的状态发生改变时，所有依赖于它的对象都会得到通知被制动更新，如Spring中listener的实现–ApplicationListener。

##### 5.详细讲解一下核心容器（spring context应用上下文) 模块

​		这是基本的Spring模块，提供spring 框架的**基础功能**，BeanFactory 是 任何以spring为基础的**应用的核心**。Spring 框架建立在此模块之上，它使Spring成为一个容器。

​		Bean 工厂是**工厂模式**的一个实现，提供了**控制反转功能**，用来把应用的配置和依赖从真正的应用代码中分离。最常用的就是org.springframework.beans.factory.xml.XmlBeanFactory ，它根据XML文件中的定义加载beans。该容器从XML 文件读取配置**元数据**并用它去创建一个完全配置的系统或应用。

##### 6.使用 Spring 有哪些方式？

​		使用 Spring 有以下方式：

- 作为一个成熟的 Spring Web 应用程序。
- 作为第三方 Web 框架，使用 Spring Frameworks 中间层。
- 作为企业级 Java Bean，它可以包装现有的 POJO（Plain Old Java Objects）。
- 用于远程使用。
  
    

#### 2.Spring控制反转(IOC)

##### 1.什么是Spring IOC 容器？

​		控制反转即IoC (Inversion of Control)，它把传统上由**程序代码直接操控的对象的调用权交给容器**，通过容器来实现对象组件的**装配和管理**。所谓的“控制反转”概念就是对组件对象控制权的转移，**从程序代码本身转移到了外部容器**。

​		Spring IOC 负责创建对象，管理对象（通过依赖注入（DI），装配对象，配置对象，并且管理这些对象的整个生命周期。

##### 2.控制反转(IoC)有什么作用

- 管理对象的**创建**和**依赖关系的维护**。对象的创建并不是一件简单的事，在对象关系比较复杂时，如果依赖关系需要程序猿来维护的话，那是相当头疼的
- 解耦，由容器去维护具体的对象
- 托管了类的产生过程，比如我们需要在类的产生过程中做一些处理，最直接的例子就是代理，如果有容器程序可以把这部分处理交给容器，应用程序则无需去关心类是如何完成代理的

##### 3.BeanFactory 和 ApplicationContext有什么区别？

​		BeanFactory和ApplicationContext是Spring的**两大核心接口**，都可以当做Spring的容器。其中ApplicationContext是BeanFactory的**子接口**。

###### 1.两者的依赖关系

​		BeanFactory：是Spring里面最**底层的接口**，包含了各种Bean的定义，读取bean配置文档，管理bean的加载、实例化，控制bean的**生命周期**，维护bean之间的**依赖关系**。

​		ApplicationContext接口作为BeanFactory的派生，除了提供BeanFactory所具有的功能外，还提供了更完整的框架功能：

- 继承MessageSource，因此支持国际化。
- 统一的资源文件访问方式。
- 提供在监听器中**注册bean的事件**。
- 同时加载多个配置文件。
- 载入多个（有继承关系）上下文 ，使得每一个上下文都专注于一个特定的层次，比如应用的web层。

###### 2.加载方式

​		**BeanFactroy采用的是延迟加载形式来注入Bean的**，即只有在使用到某个Bean时(调用getBean())，才对该Bean进行加载实例化。这样，我们就不能发现一些存在的Spring的配置问题。如果Bean的某一个属性没有注入，BeanFacotry加载后，直至第一次使用调用getBean方法才会抛出异常。

​		**ApplicationContext，它是在容器启动时，一次性创建了所有的Bean**。这样，在容器启动时，我们就可以发现Spring中存在的配置错误，这样有利于检查所依赖属性是否注入。 ApplicationContext启动后预载入所有的单实例Bean，通过预载入**单实例bean** ,确保当你需要的时候，你就不用等待，因为它们已经创建好了。相对于基本的BeanFactory，ApplicationContext **唯一的不足是占用内存空间**。当应用程序配置Bean较多时，程序启动较慢。

###### 创建方式

​		**BeanFactory通常以编程的方式被创建**，**ApplicationContext还能以声明的方式创建**，如使用ContextLoader。

###### 4.注册方式

​		BeanFactory和ApplicationContext都支持BeanPostProcessor、BeanFactoryPostProcessor的使用，但两者之间的区别是：BeanFactory需要**手动注册**，而ApplicationContext则是**自动注册**。



##### 4.什么是Spring的依赖注入？

​		控制反转IoC是一个很大的概念，可以用不同的方式来实现。其主要实现方式有两种：**依赖注入**和**依赖查找**

​		**依赖注入**：相对于IoC而言，依赖注入(DI)更加准确地描述了IoC的设计理念。所谓依赖注入（Dependency Injection），即**组件之间的依赖关系由容器在应用系统运行期来决定**，也就是由容器**动态地**将某种依赖关系的目标对象**实例**注入到应用系统中的**各个关联的组件之中**。组件不做定位查询，只提供普通的Java方法让容器去决定依赖关系。



##### 5.有哪些不同类型的依赖注入实现方式？

​		依赖注入是时下**最流行的IoC实现方式**，依赖注入分为**接口注入**（Interface Injection），**Setter方法注入**（Setter Injection）和**构造器注入**（Constructor Injection）三种方式。其中接口注入由于在灵活性和易用性比较差，现在从Spring4开始已被废弃。

​		**构造器依赖注入**：构造器依赖注入通过容器**触发一个类的构造器来实现**的，该类有一系列参数，每个参数代表一个对其他类的依赖。

​		**Setter方法注入**：Setter方法注入是容器通过**调用无参构造器或无参static工厂** 方法实例化bean之后，调用该bean的setter方法，即实现了基于setter的依赖注入。
![image-20200308130216043](/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200308130216043.png)





#### 3.Spring Beans

##### 1.什么是Spring beans？

​		Spring beans 是那些形成Spring应用的**主干的java对象**。它们被Spring IOC容器初始化，装配，和管理。这些beans通过容器中配置的元数据创建。比如，以XML文件形式定义。



##### 2.如何给Spring 容器提供配置元数据？Spring有几种配置方式

​		这里有三种重要的方法给Spring 容器提供配置元数据。

- XML配置文件。
- 基于**注解**的配置。
- 基于java的配置。



##### 3.解释Spring支持的几种bean的作用域

Spring框架支持以下五种bean的作用域：

- singleton : bean在每个Spring ioc 容器中**只有一个实例**。
- prototype：一个bean的定义**可以有多个实例**。
- request：每次http请求都会**创建一个bean**，该作用域仅在**基于web的Spring ApplicationContext情形下有效**。
- session：在一个HTTP Session中，一个bean定义**对应一个实例**。该作用域仅在基于web的Spring ApplicationContext情形下有效。
- global-session：在一个**全局的HTTP Session中**，一个bean定义**对应一个实例**。该作用域仅在基于web的Spring ApplicationContext情形下有效。



##### 4.Spring框架中的单例bean是线程安全的吗？

​		不是，Spring框架中的单例bean**不是线程安全**的。spring 中的 bean 默认是单例模式，spring 框架并没有对单例 bean 进行**多线程的封装处理**。

​		实际上大部分时候 spring bean 是无状态的（比如 dao 类），所有某种程度上来说 bean 也是安全的，但如果 bean 有状态的话（比如 view model 对象），那就要**开发者自己去保证线程安全了**，最简单的就是改变 bean 的作用域，**把“singleton”变更为“prototype”**，这样请求 bean 相当于 new Bean()了，所以就可以保证线程安全了。

- 有状态就是**有数据存储功能**。
- 无状态就是**不会保存数据**。



##### 5.Spring如何处理线程并发问题？

​		在一般情况下，只有**无状态的Bean才可以在多线程环境下共享**，在Spring中，绝大部分Bean都可以声明为singleton作用域，因为Spring对一些Bean中**非线程安全状态采用ThreadLocal进行处理**，解决线程安全问题。

​		**ThreadLocal**和**线程同步机制**都是为了**解决多线程中相同变量的访问冲突问题**。同步机制采用了“时间换空间”的方式，仅提供一份变量，不同的线程在访问前需要获取锁，没获得锁的线程则需要排队。而ThreadLocal采用了**“空间换时间”的方式**。

​		ThreadLocal会为每一个线程提供**一个独立的变量副本**，从而隔离了多个线程对数据的访问冲突。因为每一个线程都拥有自己的变量副本，从而也就**没有必要对该变量进行同步了**。ThreadLocal提供了线程安全的共享对象，在编写多线程代码时，可以把不安全的变量封装进ThreadLocal。



##### 6.什么是bean装配？

​		装配，或bean 装配是指在Spring 容器中把bean组装到一起，前提是容器需要知道bean的依赖关系，如何通过依赖注入来把它们装配到一起。



##### 7.什么是bean的自动装配？

​		在Spring框架中，在配置文件中设定bean的依赖关系是一个很好的机制，Spring 容器能够自动装配相互合作的bean，这意味着容器不需要和配置，能通过Bean工厂自动处理bean之间的协作。这意味着 Spring可以通过向Bean Factory中注入的方式自动搞定bean之间的依赖关系。自动装配可以设置在每个bean上，也可以设定在特定的bean上。



##### 8.解释不同方式的自动装配，spring 自动装配 bean 有哪些方式？

​		在spring中，对象无需自己查找或创建与其关联的其他对象，由容器负责把需要相互协作的对象引用赋予各个对象，**使用autowire来配置自动装载模式**。在Spring框架xml配置中共有5种自动装配：

- no：默认的方式是不进行自动装配的，通过手工设置ref属性来进行装配bean。
- byName：通过bean的名称进行自动装配，如果一个bean的 property 与另一bean 的name 相同，就进行自动装配。
- byType：通过参数的数据类型进行自动装配。
- constructor：利用构造函数进行装配，并且构造函数的参数通过byType进行装配。
- autodetect：自动探测，如果有构造方法，通过 construct的方式自动装配，否则使用 byType的方式自动装配。



##### 9.使用@Autowired注解自动装配的过程是怎样的？

​		使用@Autowired注解来自动装配指定的bean。在使用@Autowired注解之前需要在Spring配置文件进行配置，<context:annotation-config />。

​		在启动spring IoC时，容器自动装载了一个AutowiredAnnotationBeanPostProcessor后置处理器，当容器扫描到@Autowied、@Resource或@Inject时，就会在IoC容器自动查找需要的bean，并装配给该对象的属性。在使用@Autowired时，首先在容器中查询对应类型的bean：

- 如果查询结果刚好为一个，就将该bean装配给@Autowired指定的数据；
- 如果查询的结果不止一个，那么@Autowired会根据名称来查找；
- 如果上述查找的结果为空，那么会抛出异常。解决方法时，使用required=false。



##### 10.自动装配有哪些局限性？

​		自动装配的局限性是：

**重写**：你仍需用 和 配置来定义依赖，意味着总要重写自动装配。

**基本数据类型**：你不能自动装配简单的属性，如基本数据类型，String字符串，和类。

**模糊特性**：自动装配不如显式装配精确，如果有可能，建议使用显式装配。



#### 4.Spring注解

##### 1.@Component, @Controller, @Repository, @Service 有何区别？

@Component：这将 java 类**标记为 bean**。它是任何 Spring 管理组件的通用构造型。spring 的组件扫描机制现在可以将其拾取并将其拉入应用程序环境中。

@Controller：这将一个类标记为 **Spring Web MVC 控制器**。标有它的 Bean 会**自动导入到 IoC 容器**中。

@Service：此注解是**组件注解的特化**。它不会对 @Component **注解提供任何其他行为**。您可以在服务层类中使用 @Service 而不是 @Component，因为它以更好的方式指定了意图。

@Repository：这个注解是具有**类似用途和功能的 @Component 注解的特化**。它为 DAO 提供了额外的好处。它将 DAO 导入 IoC 容器，并使未经检查的异常有资格转换为 Spring DataAccessException。

##### 2.@Required 注解有什么作用

​		这个注解表明bean的属性必须在配置的时候设置，通过一个bean定义的显式的属性值或通过自动装配，若@Required注解的bean属性未被设置，容器将抛出BeanInitializationException。示例：

```Java
public class Employee {
    private String name;
    @Required
    public void setName(String name){
        this.name=name;
    }
    public string getName(){
        return name;
    }
}
```

##### 3.@Autowired 注解有什么作用

​		@Autowired默认是**按照类型装配注入**的，默认情况下**它要求依赖对象必须存在**（可以设置它required属性为false）。@Autowired 注解提供了更细粒度的控制，包括在何处以及如何完成自动装配。它的用法和@Required一样，修饰setter方法、构造器、属性或者具有任意名称和/或多个参数的PN方法。

```Java
public class Employee {
    private String name;
    @Autowired
    public void setName(String name) {
        this.name=name;
    }
    public string getName(){
        return name;
    }
}
```

##### 4.@Autowired和@Resource之间的区别

@Autowired可用于：构造函数、成员变量、Setter方法

@Autowired和@Resource之间的区别

@Autowired默认是按照**类型装配注入**的，默认情况下它**要求依赖对象必须存在**（可以设置它required属性为false）。

@Resource默认是按照**名称来装配注入**的，只有当找不到与名称匹配的bean才会按照类型来装配注入。

##### 5.@RequestMapping 注解有什么用？

​		@RequestMapping 注解用于**将特定 HTTP 请求方法映射到将处理相应请求的控制器中的特定类/方法**。此注释可应用于两个级别：

- 类级别：映射请求的 URL
- 方法级别：映射 URL 以及 HTTP 请求方法



#### 5.Spring数据访问

##### 1.解释对象/关系映射集成模块

​		Spring 通过提供**ORM模块**，支持我们在直接JDBC之上使用一个对象/关系映射映射(ORM)工具，Spring 支持集成主流的ORM框架，如**Hiberate**，JDO和 **iBATIS，JPA**，TopLink，JDO，OJB 。Spring的事务管理同样支持以上所有ORM框架及JDBC。

##### 2.在Spring框架中如何更有效地使用JDBC？

​		使用Spring JDBC 框架，资源管理和错误处理的代价都会被减轻。所以开发者只需写**statements 和 queries**从数据存取数据，JDBC也可以在Spring框架提供的模板类的帮助下更有效地被使用，这个模板叫JdbcTemplate

##### 3.解释JDBC抽象和DAO模块

​		通过使用JDBC抽象和DAO模块，保证数据库代码的简洁，并能避免数据库资源错误关闭导致的问题，它在各种不同的数据库的错误信息之上，提供了一个**统一的异常访问层**。它还利用Spring的AOP 模块给Spring应用中的对象提供事务管理服务。

##### 4.spring DAO 有什么用？

​		Spring DAO（数据访问对象） 使得 JDBC，Hibernate 或 JDO 这样的数据访问技术更容易以一种**统一的方式工作**。这使得用户容易在持久性技术之间切换。它还允许您在编写代码时，无需考虑捕获每种技术不同的异常。

##### 5.使用Spring通过什么方式访问Hibernate？

​		在Spring中有两种方式访问Hibernate：

- 使用 Hibernate 模板和回调进行控制反转
- 扩展 HibernateDAOSupport 并应用 AOP 拦截器节点

##### 6.Spring支持的事务管理类型， spring 事务实现方式有哪些？

​		Spring支持两种类型的事务管理：

-   **编程式事务管理**：这意味你通过编程的方式管理事务，给你带来极大的灵活性，但是难维护。

-   **声明式事务管理**：这意味着你可以将业务代码和事务管理分离，你只需用注解和XML配置来管理事务。


##### 7.Spring事务的实现方式和实现原理

​		Spring事务的**本质其实就是数据库对事务的支持**，没有数据库的事务支持，spring是无法提供事务功能的。真正的数据库层的事务提交和回滚是通过binlog或者redo log实现的。

##### 8.说一下 spring 的事务隔离？

​		spring 有五大隔离级别，默认值为 ISOLATION_DEFAULT（使用数据库的设置），其他四个隔离级别和数据库的隔离级别一致：

- **ISOLATION_DEFAULT**：用底层数据库的设置隔离级别，数据库设置的是什么我就用什么；

- **ISOLATION_READ_UNCOMMITTED**：未提交读，最低隔离级别、事务未提交前，就可被其他事务读取（会出现幻读、脏读、不可重复读）；

- **ISOLATION_READ_COMMITTED**：提交读，一个事务提交后才能被其他事务读取到（会造成幻读、不可重复读），SQL server 的默认级别；

- **ISOLATION_REPEATABLE_READ**：可重复读，保证多次读取同一个数据时，其值都和事务开始时候的内容是一致，禁止读取到别的事务未提交的数据（会造成幻读），MySQL 的默认级别；

- **ISOLATION_SERIALIZABLE**：序列化，代价最高最可靠的隔离级别，该隔离级别能防止脏读、不可重复读、幻读。

**脏读** ：表示一个事务能够读取另一个事务中**还未提交的数据**。比如，某个事务尝试插入记录 A，此时该事务还未提交，然后另一个事务尝试读取到了记录 A。

**不可重复读** ：是指在一个事务内，**多次读同一数据**。

**幻读** ：指同一个事务内**多次查询返回的结果集不一样**。比如同一个事务 A 第一次查询时候有 n 条记录，但是第二次同等条件下查询却有 n+1 条记录，这就好像产生了幻觉。发生幻读的原因也是**另外一个事务新增或者删除或者修改了第一个事务结果集里面的数据**，同一个记录的数据内容被修改了，所有数据行的记录就变多或者变少了。

##### 9.Spring框架的事务管理有哪些优点？

-   为不同的事务API 如 JTA，JDBC，Hibernate，JPA 和JDO，提供一个不变的编程模式。
-   为编程式事务管理提供了一套简单的API而不是一些复杂的事务API。
-   支持声明式事务管理。
-   和Spring各种数据访问抽象层很好得集成。



#### 6.Spring面向切面编程(AOP)

##### 1.什么是AOP

​		OOP(Object-Oriented Programming)面向对象编程，允许开发者定义纵向的关系，但并适用于定义横向的关系，导致了大量代码的重复，而不利于各个模块的重用。

​		AOP(Aspect-Oriented Programming)，一般称为**面向切面编程**，作为面向对象的一种补充，用于将那些与业务无关，但却对多个对象产生影响的公共行为和逻辑，抽取并封装为一个可重用的模块，这个模块被命名为“切面”（Aspect），减少系统中的重复代码，**降低了模块间的耦合度，同时提高了系统的可维护性**。可用于**权限认证、日志、事务处理**等。

##### 2.Spring AOP里面的几个名词

（1）**切面**（Aspect）：切面是通知和切点的结合。通知和切点共同定义了切面的全部内容。 在Spring AOP中，切面可以使用通用类（基于模式的风格） 或者在普通类中以 @AspectJ 注解来实现。

（2）**连接点**（Join point）：指方法，在Spring AOP中，一个连接点总是代表一个方法的执行。 应用可能有数以千计的时机应用通知。这些时机被称为连接点。连接点是在应用执行过程中能够插入切面的一个点。这个点可以是调用方法时、抛出异常时、甚至修改一个字段时。切面代码可以利用这些点插入到应用的正常流程之中，并添加新的行为。

（3）**通知**（Advice）：在AOP术语中，切面的工作被称为通知。

（4）**切入点**（Pointcut）：切点的定义会匹配通知所要织入的一个或多个连接点。我们通常使用明确的类和方法名称，或是利用正则表达式定义所匹配的类和方法名称来指定这些切点。

（5）**引入**（Introduction）：引入允许我们向现有类添加新方法或属性。

（6）**目标对象**（Target Object）： 被一个或者多个切面（aspect）所通知（advise）的对象。它通常是一个代理对象。也有人把它叫做 被通知（adviced） 对象。 既然Spring AOP是通过运行时代理实现的，这个对象永远是一个 被代理（proxied） 对象。

（7）**织入**（Weaving）：织入是把切面应用到目标对象并创建新的代理对象的过程。在目标对象的生命周期里有多少个点可以进行织入：

-   **编译期**：切面在目标类编译时被织入。AspectJ的织入编译器是以这种方式织入切面的。
-   **类加载期**：切面在目标类加载到JVM时被织入。需要特殊的类加载器，它可以在目标类被引入应用之前增强该目标类的字节码。AspectJ5的加载时织入就支持以这种方式织入切面。
-   **运行期**：切面在应用运行的某个时刻被织入。一般情况下，在织入切面时，AOP容器会为目标对象动态地创建一个代理对象。SpringAOP就是以这种方式织入切面。


##### 3.什么是切面 Aspect？

​		aspect 由 pointcount 和 advice 组成，切面是通知和切点的结合。 它既包含了横切逻辑的定义, 也包括了连接点的定义. Spring AOP 就是负责实施切面的框架, 它将切面所定义的横切逻辑编织到切面所指定的连接点中.
AOP 的工作重心在于如何将增强编织目标对象的连接点上, 这里包含两个工作:

-   如何通过 pointcut 和 advice 定位到特定的 joinpoint 上
-   如何在 advice 中编写切面代码.











### SpringBoot知识集合

#### 1.定义与优缺点

##### 1.什么是 Spring Boot？

​		Spring Boot 是 Spring 开源组织下的子项目，是 Spring 组件一站式解决方案，主要是简化了使用 Spring 的难度，简省了繁重的配置，提供了各种启动器，开发者能快速上手。

##### 2.Spring Boot 有哪些优点？

​		Spring Boot 主要有如下优点：

- 容易上手，提升开发效率，为 Spring 开发提供一个更快、更广泛的入门体验。
- 开箱即用，远离繁琐的配置。
- 提供了一系列大型项目通用的非业务性功能，例如：内嵌服务器、安全管理、运行数据监控、运行状况检查和外部化配置等。
- 没有代码生成，也不需要XML配置。
- 避免大量的 Maven 导入和各种版本冲突。

##### 3.Spring Boot 的核心注解是哪个？它主要由哪几个注解组成的？

​		启动类上面的注解是**@SpringBootApplication**，它也是 Spring Boot 的**核心注解**，主要组合包含了以下 3 个注解：

-   @SpringBootConfiguration：组合了 @Configuration 注解，实现配置文件的功能。

-   @EnableAutoConfiguration：打开自动配置的功能，也可以关闭某个自动配置的选项，如关闭数据源自动配置功能： @SpringBootApplication(exclude = { DataSourceAutoConfiguration.class })。

-   @ComponentScan：Spring组件扫描。




#### 2.SpringBoot配置

##### 1.什么是 JavaConfig？

​		Spring JavaConfig 是 Spring 社区的产品，它提供了配置 Spring IoC 容器的纯Java 方法。因此它有助于避免使用 XML 配置。使用 JavaConfig 的优点在于：

（1）**面向对象的配置**。由于配置被定义为 JavaConfig 中的类，因此用户可以充分利用 Java 中的面向对象功能。一个配置类可以继承另一个，重写它的@Bean 方法等。

（2）**减少或消除 XML 配置**。基于依赖注入原则的外化配置的好处已被证明。但是，许多开发人员不希望在 XML 和 Java 之间来回切换。JavaConfig 为开发人员提供了一种纯 Java 方法来配置与 XML 配置概念相似的 Spring 容器。从技术角度来讲，只使用 JavaConfig 配置类来配置容器是可行的，但实际上很多人认为将JavaConfig 与 XML 混合匹配是理想的。

（3）**类型安全和重构友好**。JavaConfig 提供了一种类型安全的方法来配置 Spring容器。由于 Java 5.0 对泛型的支持，现在可以按类型而不是按名称检索 bean，不需要任何强制转换或基于字符串的查找。



##### 2.Spring Boot 自动配置原理是什么？

​		注解 @EnableAutoConfiguration, @Configuration, @ConditionalOnClass 就是自动配置的核心，@EnableAutoConfiguration 给容器导入META-INF/spring.factories 里定义的自动配置类。筛选有效的自动配置类。每一个自动配置类结合对应的 xxxProperties.java 读取配置文件进行自动配置功能



##### 3.spring boot 核心配置文件是什么？

​		单纯做 Spring Boot 开发，可能不太容易遇到 bootstrap.properties 配置文件，但是在结合 Spring Cloud 时，这个配置就会经常遇到了，特别是在需要加载一些远程配置文件的时侯。

​		spring boot 核心的两个配置文件：

-   bootstrap (. yml 或者 . properties)：boostrap 由父 ApplicationContext 加载的，比 applicaton 优先加载，配置在应用程序上下文的引导阶段生效。一般来说我们在 Spring Cloud Config 或者 Nacos 中会用到它。且 boostrap 里面的属性不能被覆盖；
-   application (. yml 或者 . properties)： **由ApplicatonContext 加载**，用于 spring boot 项目的自动化配置。



##### 4.在自定义端口上运行 Spring Boot 应用程序？

​		为了在自定义端口上运行 Spring Boot 应用程序，您可以在application.properties 中指定端口。server.port = 8090



#### 3.安全与监视器

##### 1.如何实现 Spring Boot 应用程序的安全性？

​		为了实现 Spring Boot 的安全性，我们使用 spring-boot-starter-security 依赖项，并且必须添加安全配置。它只需要很少的代码。配置类将必须扩展WebSecurityConfigurerAdapter 并覆盖其方法。



##### 2.什么是 CSRF 攻击？

​		CSRF 代表**跨站请求伪造**。这是一种攻击，迫使最终用户在当前通过身份验证的Web 应用程序上执行不需要的操作。CSRF 攻击专门针对状态改变请求，而不是数据窃取，因为攻击者无法查看对伪造请求的响应。



##### 3.Spring Boot 中的监视器是什么？

​		Spring boot actuator 是 spring 启动框架中的重要功能之一。Spring boot 监视器可帮助您访问生产环境中正在运行的应用程序的当前状态。有几个指标必须在生产环境中进行检查和监控。即使一些外部应用程序可能正在使用这些服务来向相关人员触发警报消息。监视器模块公开了一组可直接作为 HTTP URL 访问的REST 端点来检查状态。



##### 4.我们如何监视所有 Spring Boot 微服务？

​		Spring Boot 提供监视器端点以监控各个微服务的度量。这些端点对于获取有关应用程序的信息（如它们是否已启动）以及它们的组件（如数据库等）是否正常运行很有帮助。但是，使用监视器的一个主要缺点或困难是，我们必须单独打开应用程序的知识点以了解其状态或健康状况。想象一下涉及 50 个应用程序的微服务，管理员将不得不击中所有 50 个应用程序的执行终端。为了帮助我们处理这种情况，我们将使用位于的开源项目。 它建立在 Spring Boot Actuator 之上，它提供了一个 Web UI，使我们能够可视化多个应用程序的度量。



#### 4.第三方项目整合

##### 1.什么是 WebSockets？

WebSocket 是一种计算机通信协议，通过单个 TCP 连接提供全双工通信信道。

1、WebSocket 是双向的 -使用 WebSocket 客户端或服务器可以发起消息发送。

2、WebSocket 是**全双工的** -客户端和服务器通信是**相互独立**的。

3、单个 TCP 连接 -初始连接使用 HTTP，然后将此连接升级到基于套接字的连接。然后这个单一连接用于所有未来的通信

4、Light -与 http 相比，**WebSocket 消息数据交换要轻得多**。



##### 2.什么是 Spring Data ?

​		Spring Data 是 Spring 的一个子项目。用于简化数据库访问，支持NoSQL 和 关系数据存储。其主要目标是使数据库的访问变得方便快捷。Spring Data 具有如下特点：

###### 1.SpringData 项目支持 NoSQL 存储：

-   MongoDB （文档数据库）
-   Neo4j（图形数据库）
-   Redis（键/值存储）
-   Hbase（列族数据库）

###### 2.SpringData 项目所支持的关系数据存储技术：

-   JDBC
-   JPA

###### 3.Spring Data Jpa 致力于减少数据访问层 (DAO) 的开发量.

​		开发者唯一要做的，就是声明持久层的接口，其他都交给 Spring Data JPA 来帮你完成！Spring Data JPA 通过**规范方法的名字**，根据**符合规范的名字来确定方法需要实现什么样的逻辑**。



##### 3.什么是 Spring Batch？

​		Spring Boot Batch 提供**可重用的函数**，这些函数在处理大量记录时非常重要，包括日志/跟踪，事务管理，作业处理统计信息，作业重新启动，跳过和资源管理。它还提供了更先进的技术服务和功能，通过优化和分区技术，可以实现极高批量和高性能批处理作业。简单以及复杂的大批量批处理作业可以高度可扩展的方式利用框架处理重要大量的信息。



##### 4.什么是 Apache Kafka？

​		Apache Kafka 是一个**分布式发布 - 订阅消息系统**。它是一个**可扩展**的，**容错的**发布 - 订阅消息系统，它使我们能够构建分布式应用程序。这是一个 Apache 顶级项目。Kafka 适合**离线和在线消息消费**。



##### 5.什么是 Swagger？

​		Swagger 广泛用于**可视化 API**，使用 Swagger UI 为前端开发人员提供在线沙箱。Swagger 是用于**生成 RESTful Web 服务**的可视化表示的工具，规范和完整框架实现。它使文档能够以与服务器相同的速度更新。当通过 Swagger 正确定义时，消费者可以使用最少量的实现逻辑来理解远程服务并与其进行交互。因此，Swagger消除了调用服务时的猜测。



##### 6.Swagger如何维护接口文档 ?

​		前后端分离开发日益流行，大部分情况下，我们都是通过 Spring Boot 做前后端分离开发，前后端分离一定会有接口文档，不然会前后端会深深陷入到扯皮中。一个比较笨的方法就是使用 word 或者 md 来维护接口文档，但是效率太低，接口一变，所有人手上的文档都得变。在 Spring Boot 中，这个问题常见的解决方案是 **Swagger** ，使用 Swagger 我们可以**快速生成一个接口文档网站**，接口一旦发生变化，文档就会自动更新，所有开发工程师访问这一个在线网站**就可以获取到最新的接口文档**，非常方便。



#### 5.相关重要问题

##### 1.Spring Boot项目如何热部署？

​		这可以使用 DEV 工具来实现。通过这种依赖关系，您可以节省任何更改，嵌入式tomcat 将重新启动。Spring Boot 有一个开发工具（DevTools）模块，它有助于提高开发人员的生产力。Java 开发人员面临的一个主要挑战是将文件更改自动部署到服务器并自动重启服务器。开发人员可以重新加载 Spring Boot 上的更改，而无需重新启动服务器。这将消除每次手动部署更改的需要。Spring Boot 在发布它的第一个版本时没有这个功能。这是开发人员最需要的功能。DevTools 模块完全满足开发人员的需求。该模块将在生产环境中被禁用。它还提供 H2 数据库控制台以更好地测试应用程序。

```Java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency>
```



##### **2.Spring Boot 中的 starter 到底是什么 ?**

​		首先，这个 Starter 并非什么新的技术点，基本上还是基于 Spring 已有功能来实现的。首先它提供了一个自动化配置类，一般命名为 XXXAutoConfiguration ，在这个配置类中通过条件注解来决定一个配置是否生效（条件注解就是 Spring 中原本就有的），然后它还会提供一系列的默认配置，也允许开发者根据实际情况自定义相关配置，然后通过类型安全的属性注入将这些配置属性注入进来，新注入的属性会代替掉默认属性。正因为如此，很多第三方框架，我们只需要引入依赖就可以直接使用了。当然，开发者也可以自定义 Starter



##### 3.spring-boot-starter-parent 有什么用 ?

​		我们都知道，新创建一个 Spring Boot 项目，默认都是有 parent 的，这个 parent 就是 spring-boot-starter-parent ，**spring-boot-starter-parent** 主要有如下作用：

- 定义了 Java 编译版本为 1.8 。
- 使用 UTF-8 格式编码。
- 继承自 spring-boot-dependencies，这个里边定义了依赖的版本，也正是因为继承了这个依赖，所以我们在写依赖时才不需要写版本号。
- 执行打包操作的配置。
- 自动化的资源过滤。
- 自动化的插件配置。
- 针对 application.properties 和 application.yml 的资源过滤，包括通过 profile 定义的不同环境的配置文件，例如 application-dev.properties 和 application-dev.yml。



##### 4.Spring Boot 打成的 jar 和普通的 jar 有什么区别 ?

​		Spring Boot 项目最终打包成的 jar 是**可执行 jar** ，这种 jar 可以直接通过 java -jar xxx.jar 命令来运行，这种 jar **不可以作为普通的 jar 被其他项目依赖**，**即使依赖了也无法使用其中的类**。

​		Spring Boot 的 jar 无法被其他项目依赖，**主要还是他和普通 jar 的结构不同**。普通的 jar 包，解压后直接就是包名，包里就是我们的代码，而 Spring Boot 打包成的可执行 jar 解压后，在 \BOOT-INF\classes 目录下才是代码，因此无法被直接引用。**如果非要引用，可以在 pom.xml 文件中增加配置**，将 Spring Boot 项目**打包成两个 jar ，一个可执行，一个可引用。**



##### 5.运行 Spring Boot 有哪几种方式？

1）打包用命令或者放到容器中运行

2）用 Maven/ Gradle 插件运行

3）直接执行 main 方法运行



##### 6.开启 Spring Boot 特性有哪几种方式？

1）继承spring-boot-starter-parent项目

2）导入spring-boot-dependencies项目依赖



##### 7.如何使用 Spring Boot 实现异常处理？

​		Spring 提供了一种使用 ControllerAdvice 处理异常的非常有用的方法。 我们通过实现一个 ControlerAdvice 类，来处理控制器类抛出的所有异常。



##### **8.如何使用 Spring Boot 实现分页和排序？**

​		使用 Spring Boot 实现分页非常简单。使用 Spring Data-JPA 可以实现将可分页的传递给存储库方法。



##### **9.微服务中如何实现 session 共享 ?**

​		在微服务中，一个完整的项目被拆分成多个不相同的独立的服务，各个服务**独立部署在不同的服务器上**，各自的 session 被从物理空间上隔离开了，但是经常，我们需要在不同微服务之间共享 session ，常见的方案就是 Spring Session + Redis 来实现 session 共享。将所有微服务的 session 统一保存在 Redis 上，当各个微服务对 session 有相关的读写操作时，都去操作 Redis 上的 session 。这样就**实现了 session 共享**，Spring Session 基于 Spring 中的代理过滤器实现，使得 session 的同步操作对开发人员而言是透明的，非常简便。



##### 10.Spring Boot 中如何实现定时任务 ?

​		定时任务也是一个常见的需求，Spring Boot 中对于定时任务的支持主要还是来自 Spring 框架。在 Spring Boot 中使用定时任务主要有两种不同的方式，一个就是**使用 Spring 中的 @Scheduled 注解**，另一个则是使用第三方框架 Quartz。

- 使用 Spring 中的 @Scheduled 的方式主要通过 @Scheduled 注解来实现。

- 使用 Quartz ，则按照 Quartz 的方式，定义 Job 和 Trigger 即可。
  





### Java设计模式

#### 1.设计模式的分类

总体来说设计模式分为三大类：

##### **1.创建型模式**

​	共五种：**单例模式、工厂方法模式、抽象工厂模式、建造者模式、原型模式**。

##### **2.结构型模式**

​	共七种：**适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式**。

##### **3.行为型模式**

​	共十一种：**策略模式、模板方法模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式**。

其实还有两类：**并发型模式和线程池模式**。用一个图片来整体描述一下：

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200406131324724.png" alt="image-20200406131324724" style="zoom:50%;" />

 

#### 2.设计模式的六大原则

##### 总原则：开闭原则（Open Close Principle）

​		开闭原则就是说**对扩展开放，对修改关闭**。在程序需要进行拓展的时候，不能去修改原有的代码，而是要扩展原有代码，实现一个热插拔的效果。所以一句话概括就是：为了使程序的扩展性好，易于维护和升级。想要达到这样的效果，我们需要使用接口和抽象类等，后面的具体设计中我们会提到这点。

##### 1、单一职责原则

​		不要存在多于一个导致类变更的原因，也就是说每个类应该实现单一的职责，如若不然，就应该把类拆分。 

##### 2、里氏替换原则（Liskov Substitution Principle）

​		里氏代换原则(Liskov Substitution Principle LSP)面向对象设计的基本原则之一。 里氏代换原则中说，任何基类可以出现的地方，子类一定可以出现。 LSP是继承复用的基石，只有当衍生类可以替换掉基类，软件单位的功能不受到影响时，基类才能真正被复用，而衍生类也能够在基类的基础上增加新的行为。里氏代换原则是对“开-闭”原则的补充。实现“开-闭”原则的关键步骤就是抽象化。而基类与子类的继承关系就是抽象化的具体实现，所以里氏代换原则是对实现抽象化的具体步骤的规范。—— From Baidu 百科

​		里氏替换原则中，子类对父类的方法**尽量不要重写和重载**。因为父类代表了定义好的结构，通过这个规范的接口与外界交互，子类不应该随便破坏它。

##### 3、依赖倒转原则（Dependence Inversion Principle）

​		这个是开闭原则的基础，具体内容：**面向接口编程**，依赖于**抽象**而不依赖于**具体**。写代码时用到具体类时，不与具体类交互，而与具体类的上层接口交互。

##### 4、接口隔离原则（Interface Segregation Principle）

​		这个原则的意思是：每个接口中**不存在子类用不到却必须实现的方法**，如果不然，就要将接口拆分。使用多个隔离的接口，比使用单个接口（多个接口方法集合到一个的接口）要好。 

##### 5、迪米特法则（最少知道原则）（Demeter Principle）

​		就是说：一个类对自己依赖的类知道的越少越好。也就是说无论被依赖的类多么复杂，都应该将逻辑封装在方法的内部，通过public方法提供给外部。这样当被依赖的类变化时，才能最小的影响该类。

​		最少知道原则的另一个表达方式是：只与**直接的朋友通信**。类之间只要有耦合关系，就叫朋友关系。耦合分为依赖、关联、聚合、组合等。我们称出现为成员变量、方法参数、方法返回值中的类为直接朋友。局部变量、临时变量则不是直接的朋友。我们要求陌生的类不要作为局部变量出现在类中。

##### 6、合成复用原则（Composite Reuse Principle）

​		原则是尽量首先使用合成/聚合的方式，而不是使用继承。



#### 3.具体设计模式

​		从这一节开始，我们详细介绍Java中23种设计模式的概念，应用场景等情况，并结合他们的特点及设计模式的原则进行分析。 

##### 四类创建型模式

##### 1.单例模式

​		**单例对象（Singleton）是一种常用的设计模式。**

###### **1.单例模式概念**

​		在Java应用中，单例对象能保证在一个JVM中，**该对象只有一个实例存在**。这样的模式有几个好处：

-   某些类创建比较频繁，对于一些**大型的对象**，这是一笔**很大的系统开销**。
-   **省去了new操作符**，降低了系统内存的使用频率，**减轻GC压力**。
-   有些类如交易所的**核心交易引擎**，控制着交易流程，如果该类可以创建多个的话，系统完全乱了。（就像一个军队出现了多个司令员同时指挥，肯定会乱成一团），所以只有使用单例模式，才能保证核心交易服务器独立控制整个流程。

​       **单例模式特点:**

-   单例类只能有**一个实例**。

-   单例类必须**自己创建自己的唯一实例**。

-   单例类必须**给所有其他对象提供这一实例**。

   单例模式确保**某个类只有一个实例**，而且自行实例化并**向整个系统提供这个实例**。
   
   

###### **2.第一种-懒汉式单例**

```Java
//懒汉式单例类.在第一次调用的时候实例化自己   
public class Singleton {
    // 构造方法私有化
    private Singleton() {
    }
    private static Singleton single = null;
    // 静态工厂方法
    public static Singleton getInstance() {
        if (single == null) {
            single = new Singleton();
        }
        return single;
    }
}
```

​		Singleton通过将**构造方法限定为private**避免了类在**外部被实例化**，在同一个虚拟机范围内，Singleton的**唯一实例只能通过getInstance()方法访问**。事实上，通过Java反射机制**是能够实例化构造方法为private的类的**，那基本上会使所有的Java单例实现失效。此问题在此处不做讨论，**姑且掩耳盗铃地认为反射机制不存在**。
​		但是以上懒汉式单例的实现**没有考虑线程安全问题，它是线程不安全的**，并发环境下很可能**出现多个Singleton实例**，要实现线程安全，有以下三种方式，都是对getInstance这个方法改造，保证了懒汉式单例的线程安全:

​		**2.1 方法整体加同步锁——在getInstance方法上加同步**

```java
public static synchronized  Singleton getInstance() {
        if (single == null) {
            single = new Singleton();
        }
        return single;
}
```

​		**2.2 避免已存在实例也被同步——双重检查锁定**
​		可以使用“双重检查加锁”的方式来实现，就可以**既实现线程安全**，**又能够使性能不受很大的影响**。那么什么是“双重检查加锁”机制呢？
​		所谓“**双重检查加锁**”机制，指的是：并不是每次进入getInstance方法都需要同步，而是先不同步，进入方法后，**先检查实例是否存在**，如果不存在**才进行下面的同步块**，这是**第一重检查** ；进入同步块过后，**再次检查实例是否存在**，如果不存在，就在**同步的情况下**创建一个实例，这是**第二重检查**。这样 一来，就**只需要同步一次**了，从而减少了**多次在同步情况下进行判断所浪费的时间**。
​		“双重检查加锁”机制的实现**会使用关键字volatile**，它的意思是：**被volatile修饰的变量的值，将不会被本地线程缓存，所有对该变量的读写都是直接操作共享内存，从而确保多个线程能正确的处理该变量**。注意：在java1.4及以前版本中，很多JVM对于volatile关键字的实现的问题，会**导致“双重检查加锁”的失败**，因此“双重检查加锁”机制**只能用在java5及以上的版本**。

```Java
public class Singleton {
  //注意此处的volatile关键字，指明了instance为同步对象；
    private volatile static Singleton instance = null;
    private Singleton(){}
    public static Singleton getInstance(){
        //先检查实例是否存在，如果不存在才进入下面的同步块
        if(instance == null){
            //利用同步块而不是同步方法，线程安全的创建实例
            synchronized (Singleton.class) {
                //再次检查实例是否存在，如果不存在才真正的创建实例
                if(instance == null){
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

​		这种实现方式既可以实现线程安全地创建实例，而又不会对性能造成太大的影响。它只是**第一次创建实例的时候同步，以后就不需要同步了**，从而加快了运行速度。
​		提示：由于volatile关键字**可能会屏蔽掉虚拟机中一些必要的代码优化**，所以运行效率并不是很高。因此一般建议，没有特别的需要，不要使用。也就是说，虽然可以使用“双重检查加锁”机制**来实现线程安全的单例**，但并不建议大量采用，可以根据情况来选用。

​		**2.3 通过内部类来实现（只执行一个域的访问）——静态(类级)内部类**

```Java
public class Singleton {

    private Singleton(){}
    /**
     *    类级的内部类，也就是静态的成员式内部类，该内部类的实例与外部类的实例
     *    没有绑定关系，而且只有被调用到时才会装载，从而实现了延迟加载。
     */
    private static class SingletonHolder{
        /**
         * 静态初始化器，由JVM来保证线程安全
         */
        private static Singleton instance = new Singleton();
    }

    public static Singleton getInstance(){
        return SingletonHolder.instance;
    }
}
```

​		这种方式比上面1、2都好一些，既实现了线程安全，又**避免了同步带来的性能影响**。当getInstance方法第一次被调用的时候，它**第一次读取 SingletonHolder.instance**，导致SingletonHolder类得到**初始化**；而这个类在装载并被初始化的时候，会**初始化它的静态域**，从而创建Singleton的实例，由于是静态的域，因此**只会在虚拟机装载类的时候初始化一次**，并**由虚拟机来保证它的线程安全性**。

​		这个模式的优势在于，**getInstance方法并没有被同步**，并且只是**执行一个域的访问**，因此延迟初始化并**没有增加任何访问成本**。



###### **3.饿汉式单例**

```Java
//饿汉式单例类.在类初始化时，已经自行实例化   
public class EagerSingleton {
        private static EagerSingleton instance = new EagerSingleton();
        /**
         * 构造方法私有化
         */
        private EagerSingleton(){}
        /**
         * 静态工厂方法
         */
        public static EagerSingleton getInstance(){
            return instance;
        }
}
```

​		饿汉式在类创建的同时就已经**创建好一个静态的对象供系统使用**，以后不再改变，所以天生是线程安全的。缺点则是会占用一定空间。



###### 4.饿汉式和懒汉式单例的区别

​		从名字上来说，饿汉就是类一旦加载，就把单例初始化完成，保证getInstance的时候，单例是已经存在的了；而懒汉比较懒，只有当调用getInstance的时候，才回去初始化这个单例。另外从以下两点再区分以下这两种方式：

​		**4.1线程安全**：
​		饿汉式**天生就是线程安全的**，可以直接用于多线程而不会出现问题，
​		懒汉式本身是**非线程安全**的，为了实现线程安全有三种写法，这三种实现在资源加载和性能方面有些区别。

​		**4.2资源加载和性能**：
​		饿汉式在类创建的同时就实例化一个静态对象出来，不管之后会不会使用这个单例，都会**占据一定的内存**，但是相应的，在第一次调用时速度也会更快，因为其资源已经初始化完成，而懒汉式顾名思义，会**延迟加载**，在第一次使用该单例的时候才会实例化对象出来，第一次调用时要做初始化，如果要做的工作比较多，性能上会有些延迟，之后就和饿汉式一样了。
​		**至于懒汉式的三种实现方式又有些区别：**
 		第1种，在方法调用上加了同步，虽然线程安全了，但是每次都要同步，**会影响性能**，毕竟99%的情况下是不需要同步的；
​		第2种，在getInstance中**做了两次null检查**，确保了只有第一次调用单例的时候才会做同步，这样也是线程安全的，同时**避免了每次都同步的性能损耗**；
​		第3种，保证初始化instance时只有一个线程，所以也是线程安全的，**同时没有性能损耗**，一般倾向于使用这一种。（最建议选择的）

​		**4.3什么是线程安全**？
​		如果你的代码所在的进程中**有多个线程在同时运行**，而这些线程可能会同时运行这段代码。如果每次运行结果和单线程运行的结果是一样的，而且其他的变量的值也和预期的是一样的，就是线程安全的。
​		或者说：一个类或者程序所提供的接口对于线程来说是**原子操作**，或者多个线程之间的切换不会导致该接口的执行**结果存在二义性**,也就是说我们**不用考虑同步的问题**，那就是线程安全的。



###### 5.利用枚举来实现单例（推荐）

​	用枚举来实现单例非常简单，只需要编写一个**包含单个元素的枚举类型**即可。

```Java
public enum Singleton {
    /**
     * 定义一个枚举的元素，它就代表了Singleton的一个实例。
     */

    uniqueInstance;
    /**
     * 单例可以有自己的操作
     */
    public void singletonOperation(){
        //功能处理
    }
}
```

　相关测试代码：

```Java
public enum SingletonEnum {
    INSTANCE01, INSTANCE02;// 定义枚举的两个类型
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name){
        this.name = name;
    }
}
public class Test {
    public static void main(String[] args) {
            SingletonEnum instance01=SingletonEnum.INSTANCE01;
            instance01.setName("tanggao");
            System.out.println(instance01.getName());

            SingletonEnum instance02=SingletonEnum.INSTANCE01;
            System.out.println(instance02.getName());

            SingletonEnum instance03=SingletonEnum.INSTANCE02;
            instance03.setName("zsy");
            System.out.println(instance03.getName());

            SingletonEnum instance04=SingletonEnum.INSTANCE02;
            instance04.setName("zsy1");
            System.out.println(instance04.getName());
            System.out.println(instance03.hashCode()+"\t"+instance04.hashCode());
            System.out.println(instance03==instance04);

    }
}
//运行结果:
tanggao
tanggao
zsy
zsy1
3346521 3346521
true
```

​		使用枚举来实现单实例控制会更加简洁，而且**无偿地提供了序列化机制**，并由JVM从根本上提供保障，**绝对防止多次实例化**，是**更简洁、高效、安全的实现单例的方式**。





##### 2.工厂模式

​		在面向对象编程中, 最通常的方法是**一个new操作符产生一个对象实例**,new操作符就是用来**构造对象实例的**。但是在一些情况下, new操作符直接生成对象会带来一些问题。举例来说, 许多类型对象的创造需要一系列的步骤: 

-   你可能需要**计算或取得对象的初始设置**; 
-   选择**生成哪个子对象实例**; 
-   或在生成你需要的对象之前**必须先生成一些辅助功能的对象**。

​       在这些情况,新对象的建立就是一个“过程”，不仅是一个操作，像一部大机器中的一个齿轮传动。模式的问题：你如何能**轻松方便地构造对象实例**，而**不必关心构造对象实例的细节和复杂过程**呢？解决方案：建立一个工厂来创建对象。

###### 1.定义与分类

​		假如还没有工业革命，如果一个客户要一款车,一般的做法是客户去打造一款车，然后拿来用，这显示很低效。工厂模式主要是**为创建对象提供过渡接口**，以便将创建对象的**具体过程屏蔽隔离起来**，达到提高**灵活性**的目的。工厂模式在《Java与模式》中分为三类：

-   **简单工厂模式**（Simple Factory）：用户不用去创建宝马车。因为有一个工厂来帮他想要的车，比如想要宝马车，工厂就创建这个系列的车。即**工厂可以创建同一系列产品**。
-   **工厂方法模式**（Factory Method）：为了满足客户，车系列越来越多，**一个工厂无法创建所有的车系列**。于是由**单独分出来多个具体的工厂**。每个具体工厂创建一种系列。即**具体工厂类只能创建一个具体产品**。
-   **抽象工厂模式**（Abstract Factory）：随着客户要求越来越高，对车进行分类，分为**商务车和运动车两个族**。

​        这三种模式从上到下**逐步抽象**，并且**更具一般性**。也可将工厂模式分为两类：**工厂方法模式（Factory Method）与抽象工厂模式（Abstract Factory）**。将简单工厂模式（Simple Factory）看为工厂方法模式的一种特例，两者归为一类。这两种分类方式皆可。



###### **2.简单工厂模式**

​		首先，简单工厂模式又称**静态工厂方法模式**，定义**一个用于创建对象的接口**。先来看看它的组成：

1.   **工厂类角色**：这是本模式的核心，含有一定的**商业逻辑和判断逻辑**。在java中它往往由**一个具体类**实现。
2.   **抽象产品角色**：它一般是具体产品**继承的父类或者实现的接口**。在java中**由接口或者抽象类来实现**。
3.   **具体产品角色**：工厂类所创建的**对象就是此角色的实例**。在java中由一个具体类实现。

​        简单工厂一般分为：**普通简单工厂、多方法简单工厂、静态方法简单工厂**三种模式：

-   **普通简单工厂**。就是建立一个工厂类，对实现了**同一接口**的**一些类进行实例**的创建。

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200411010340530.png" alt="image-20200411010340530" style="zoom:40%;" />

```Java
//举例如下：（我们举一个发送邮件和短信的例子）
//首先，创建二者的共同接口：
public interface Sender {
	public void Send();
}
//其次，创建实现类：
public class MailSender implements Sender {
	@Override
	public void Send() {
		System.out.println("this is mailsender!");
	}
}
public class SmsSender implements Sender {
 
	@Override
	public void Send() {
		System.out.println("this is sms sender!");
	}
}
//最后，建工厂类：
public class SendFactory {
 
	public Sender produce(String type) {
		if ("mail".equals(type)) {
			return new MailSender();
		} else if ("sms".equals(type)) {
			return new SmsSender();
		} else {
			System.out.println("请输入正确的类型!");
			return null;
		}
	}
}
//来测试下：
public class FactoryTest {
 
	public static void main(String[] args) {
		SendFactory factory = new SendFactory();
		Sender sender = factory.produce("sms");
		sender.Send();
	}
}
//输出：this is sms sender!
```

-   **多个方法简单工厂**。是对普通工厂方法模式的改进，在普通工厂方法模式中，如果传递的字符串出错，则不能正确创建对象，而多个工厂方法模式是**提供多个工厂方法**，**分别创建对象**。

    <img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200411011051494.png" alt="image-20200411011051494" style="zoom:40%;" />

```Java
//将上面的代码做下修改，改动下SendFactory类就行，如下：
public class SendFactory {
	
	public Sender produceMail(){
		return new MailSender();
	}
	
	public Sender produceSms(){
		return new SmsSender();
	}
}
//测试
public class FactoryTest {
 
	public static void main(String[] args) {
		SendFactory factory = new SendFactory();
		Sender sender = factory.produceMail();
		sender.Send();
	}
}
```

-   **静态方法简单工厂**。将上面的多个工厂方法模式里的**方法置为静态的**，不需要创建实例，直接调用即可。

```java
public class SendFactory {
	
	public static Sender produceMail(){
		return new MailSender();
	}
	
	public static Sender produceSms(){
		return new SmsSender();
	}
}
//测试
public class FactoryTest {
 
	public static void main(String[] args) {	
		Sender sender = SendFactory.produceMail();
		sender.Send();
	}
}
```

​        总体来说，简单工厂模式适合：凡是出现了**大量的产品需要创建**，并且具有**共同的接口**时，可以通过工厂方法模式进行创建。在以上的三种模式中，**第一种如果传入的字符串有误，不能正确创建对象**，第三种相对于第二种，**不需要实例化工厂类**，所以，大多数情况下，我们会选用第三种——**静态工厂方法模式**。



###### 3.工厂方法模式

​		简单工厂模式有一个问题就是，类的创建依赖工厂类，也就是说，如果想要拓展程序，必须对**工厂类**进行修改，这**违背了闭包原则**，所以，从设计角度考虑，有一定的问题，如何解决？就用到**工厂方法模式**，创建一个**工厂接口**和创建**多个工厂实现类**，这样一旦需要增加新的功能，直接**增加新的工厂类**就可以了，不需要修改之前的代码。

​		工厂方法模式去掉了简单工厂模式中工厂方法的**静态属性**，使得**它可以被子类继承**。这样在简单工厂模式里集中在工厂方法上的压力可以由工厂方法模式里**不同的工厂子类来分担**。工厂方法模式的结构，来看下它的组成：

1.  **抽象工厂角色**： 这是工厂方法模式的**核心**，它与应用程序无关。是具体工厂角色**必须实现的接口或者必须继承的父类**。在java中它由**抽象类或者接口**来实现。
2.  **具体工厂角色**：它**含有和具体业务逻辑有关的代码**。由应用程序调用以创建对应的具体产品的对象。
3.  **抽象产品角色**：它是具体产品**继承的父类或者是实现的接口**。在java中一般有抽象类或者接口来实现。
4.  **具体产品角色**：具体工厂角色**所创建的对象就是此角色的实例**。在java中由具体的类来实现。		

```Java
/**
 * 抽象产品角色
 * @author Administrator
 *
 */
abstract class Car {
    abstract void drive();
}
/**
 * 具体产品类
 * @author Administrator
 *
 */
public class bmwCar extends Car {

    @Override
    public void drive() {
        System.out.println("驾驶宝马车......");
    }
}
/**
 * 具体产品类
 * @author Administrator
 *
 */
public class benzCar extends Car {
    @Override
    public void drive() {
        System.out.println("驾驶奔驰车......");
    }
}


/**
 * 抽象工厂角色 用來生产车
 * @author Administrator
 *
 */
public interface abstractFactory {
    public Car driveFactory();

}
/**
 * 具体工厂角色
 * 创建奔驰车对象
 */
public class benzCarFactory implements abstractFactory {
    @Override
    public  Car driveFactory() {
        return new benzCar();
    }
}
/**
 * 具体工厂角色
 * 创建宝马车对象
 */
public class bmwCarFactory implements abstractFactory {
    @Override
    public Car driveFactory() {
        return new bmwCar();
    }
}


//客户
public class Client {
    private static Car benzcar,bmwcar;
    private static abstractFactory benzcarfactory,bmwcarfactory;
    public static void main(String[] args) throws Exception {
        //告诉(工厂) 要奔驰车
        benzcarfactory = new benzCarFactory();
        benzcar = benzcarfactory.driveFactory();
        //可以开车了
        benzcar.drive();
        System.out.println("-------------------");
        bmwcarfactory = new bmwCarFactory();
        bmwcar = bmwcarfactory.driveFactory();
        bmwcar.drive();
    }
}
```

​		这个模式的好处就是，如果你现在想增加一个功能：发即时信息，则**只需做一个实现类**，实现Sender接口，同时**做一个工厂类**，实现Provider接口，就OK了，无需去改动现成的代码。这样做，拓展性较好！

​		工厂方法模式**使用继承自抽象工厂角色**的多个子类来**代替简单工厂模式中的“上帝类”**。正如上面所说，这样便分担了对象承受的压力；而且这样使得结构变得灵活起来——当有新的产品产生时，只要按照抽象产品角色来生成具体的实现类，那么就可以被客户使用，而不必去修改任何已有的代码。可以看出工厂角色的结构也是**符合开闭原则**的！
​		然而也可以看出工厂方法的加入，**使得对象的数量成倍增长**。当产品种类非常多时，会出现**大量的与之对应的工厂对象**，这不是我们所希望的。如果不能避免这种情况，可以考虑使用**简单工厂模式与工厂方法模式相结合**的方式来减少工厂类：**即对于产品树上类似的种类（一般是树的叶子中互为兄弟的）使用简单工厂模式来实现**。



###### 4.抽象工厂模式

​		抽象工厂需要**创建一些列产品族**，着重点在于"创建哪些"产品上，也就是说，如果你开发，你的主要任务是划分**不同差异的产品线**，并且尽量保持**每条产品线接口一致**，从而可以**从同一个抽象工厂继承**。工厂方法模式和抽象工厂模式不好分清楚，他们的区别如下：

```Java
/* 工厂方法模式：*/
一个抽象产品类，可以派生出多个具体产品类。   
一个抽象工厂类，可以派生出多个具体工厂类。   
每个具体工厂类只能创建一个具体产品类的实例。

/* 抽象工厂模式：*/
多个抽象产品类，每个抽象产品类可以派生出多个具体产品类。   
一个抽象工厂类，可以派生出多个具体工厂类。   
每个具体工厂类可以创建多个具体产品类的实例，也就是创建的是一个产品线下的多个产品。   

/* 区别：*/
工厂方法模式只有一个抽象产品类，而抽象工厂模式有多个。   
工厂方法模式的具体工厂类只能创建一个具体产品类的实例，而抽象工厂模式可以创建多个。
工厂方法创建 "一种" 产品，他的着重点在于"怎么创建"，也就是说如果你开发，你的大量代码很可能围绕着这种产品的构造，初始化这些细节上面。也因为如此，类似的产品之间有很多可以复用的特征，所以会和模版方法相随。
```

​		**产品族**：不同产品等级结构中，功能相关联的产品组成的家族。还是让我们用一个例子来形象地说明一下吧。BenzSportsCar和BmwSportsCar 就是**一个产品族**。他们都可以放到跑车家族中，因此功能有所关联。同理BmwBussinessCar和BenzBussinessCar也是一个产品族。
​		回到抽象工厂模式的话题上。可以说，抽象工厂模式和工厂方法模式的区别就在于**需要创建对象的复杂程度**上。而且抽象工厂模式是三个里面**最为抽象、最具一般性**的。抽象工厂模式的用意为：给客户端提供一个接口，可以创建多个产品族中的产品对象，而且使用抽象工厂模式还要满足以下条件：

1.   系统中有**多个产品族**，而系统一次只可能消费其中一族产品。
2.   同属于同一个产品族的产品一起使用。

​        以下是抽象工厂模式的实现实例：

```Java
// 抽象产品类  定义不同的产品之间的标准，商务车
public interface ICarA {
    public void drive();
}
// 抽象产品类  定义不同的产品之间的标准 跑车族
public interface ICarB {
    public void drive();
}

//具体产品类
public class productAHongqi implements ICarA {
    @Override
    public void drive() {
        System.out.println("开商务族--红旗车");
    }
}
//具体产品类
public class productABmw implements ICarA {
    @Override
    public void drive() {
         System.out.println("开商务族--宝马车 ..."); 
    }
}

//具体产品类
public class producSporttBAudi implements ICarB {
    @Override
    public void drive() {
        System.out.println("开跑车族--奥迪车...");
    }
}
//具体产品类
public class productSportBBenz implements ICarB {
    @Override
    public void drive() {
         System.out.println("开跑车族--奔驰车 ..."); 
    }
}


/**
 * 抽象工厂类 创建跑车族跑车
 * @author Administrator
 *
 */
public abstract class abstractoryFactory1 {
    abstract ICarB getProductBBenz();
    abstract ICarB getProductBAudi();
}
/**
 * 抽象工厂类  创建商务族跑车
 * @author Administrator
 *
 */
public abstract class abstractoryFactory2 {
    abstract ICarA getProductABmw();
    abstract ICarA getProductBHongqi();
}


/**
 * 具体工厂类 跑车族
 * @author Administrator
 *
 */
public class Factory1 extends abstractoryFactory1 {

    @Override
    ICarB getProductBBenz() {
        return new productSportBBenz();
    }

    @Override
    ICarB getProductBAudi() {
        return new producSporttBAudi();
    }
}
/**
 * 具体工厂类
 * 商务族
 * @author Administrator
 *
 */
public class Factory2 extends abstractoryFactory2 {
    @Override
    ICarA getProductABmw() {
        return new productABmw();
    }

    @Override
    ICarA getProductBHongqi() {
        return new productAHongqi();
    }
}


public class Client {
    public static void main(String[] args) {
        //工厂一制造的产品族车
        abstractoryFactory1 factory1 = new Factory1(); 
        ICarB productsportAbenz = factory1.getProductBBenz(); 
        ICarB productsportBaudi = factory1.getProductBAudi(); 

        productsportAbenz.drive(); 
        productsportBaudi.drive(); 
        //工厂二制造的产品族车
        abstractoryFactory2 factory2 = new Factory2(); 
        ICarA productAbmw = factory2.getProductABmw(); 
        ICarA productBhongqi = factory2.getProductBHongqi(); 
        productAbmw.drive(); 
        productBhongqi.drive(); 
    }
}
```



##### 3.建造者模式

###### 1.建造者模式概念　

​		建造模式是**对象的创建模式**。建造模式可以将一个产品的**内部表象**（internal representation）与产品的**生产过程**分割开来，从而可以使**一个建造过程生成具有不同的内部表象的产品对象**。

-   **产品的内部表象**

    ​		一个产品常有不同的组成成分作为产品的零件，这些零件有可能是对象，也有可能不是对象，它们通常又叫做产品的内部表象（internal representation）。不同的产品可以有不同的内部表象，也就是不同的零件。使用建造模式可以使客户端不需要知道所生成的产品有哪些零件，每个产品的对应零件彼此有何不同，是怎么建造出来的，以及怎么组成产品。

-   **对象性质的建造**

    　　​		有些情况下，一个对象的一些性质必须按照**某个顺序赋值**才有意义。在某个性质没有赋值之前，另一个性质则无法赋值。这些情况使得性质本身的建造涉及到复杂的商业逻辑。这时候，此对象相当于一个有待建造的产品，而对象的这些性质**相当于产品的零件**，建造产品的过程是建造零件的过程。由于建造零件的过程很复杂，因此，这些零件的建造过程往往被“外部化”到另一个称做建造者的对象里，建造者对象返还给客户端的是一个**全部零件都建造完毕的产品对象**。

    ​		顾名思义，builder的意思是建造者或者建筑工人。例如：楼房是千差万别的，楼房的外形，层数，内部房间的数量，房间的装饰都不一样。但是对于建造者来说，抽象出来的建筑流程是确定的。因为建筑一座楼房，都可以归纳为几个步骤(也就是生产过程：对象性质的建造)：1打桩(内部具体细节构建:内部表象, 下2-4同为内部具体细节构建)、2建地基、3搭框架、4内部建设。同理，建造者设计模式也是基于这样的概念而生的，这个设计模式用来解决什么样的情况呢：即**流程不变，但每个流程实现的具体细节是会变化的**。这样的情况，可以考虑使用建造者。就像盖房子，4 个流程都必须有，但每个流程各自的实现细节，各个房子各有不同。建造者模式的好处就是保证了流程不会变化，即流程不会增加也不会遗漏，也不会产生流程次序 的错误。这是非常重要的，看新闻，一些楼歪歪的事件，很多都是建设楼盘的时候，流程出现了问题导致的。（看来这些人并不知道建造者模式啊）。而建造者模 式，保证了流程的确定性，而流程内部的实现细节，是可继承扩展的。从根源上解决了流程不规范的问题。
    
    

###### 2.建造者模式结构

![建造者模式](/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/20160124163530064.png)

​		在这个示意性的系统里，最终产品Product只有两个零件，即part1和part2。相应的建造方法也有两个：**buildPart1()和buildPart2()**、同时可以看出本模式涉及到四个角色，它们分别是：

　　**抽象建造者（Builder）角色**：给出一个抽象接口，以规范产品对象的各个组成成分的建造。一般而言，此接口独立于应用程序的商业逻辑。模式中直接创建产品对象的是具体建造者 (ConcreteBuilder)角色。具体建造者类必须实现这个接口所要求的两种方法：一种是**建造方法(buildPart1和 buildPart2)**，另一种是**返还结构方法(retrieveResult)**。一般来说，产品所包含的零件数目与建造方法的数目相符。换言之，有多少 零件，就有多少相应的建造方法。

　　**导演者（Director）角色**：担任这个角色的类**调用具体建造者角色以创建产品对象**。应当指出的是，导演者角色并没有产品类的具体知识，真正拥有产品类的具体知识的是具体建造者角色。

​		**产品（Product）角色**：产品便是建造中的**复杂对象**。一般来说，一个系统中会有多于一个的产品类，而且这些产品类并不一定有共同的接口，而完全可以是不相关联的。

　　一般来说，每有一个产品类，就有一个相应的具体建造者类。这些产品应当有一样数目的零件，而每有一个零件就相应地在所有的建造者角色里有一个建造方法。

​		**源代码：**

```Java
/**
 * 产品角色
 * 　产品类Product
 * @author Administrator
 *
 */
public class Product {
    //定义一些产品的部件
    private String part1;
    private String part2;

    public String getPart1() {
        return part1;
    }
    public void setPart1(String part1) {
        this.part1 = part1;
    }
    public String getPart2() {
        return part2;
    }
    public void setPart2(String part2) {
        this.part2 = part2;
    }
}

/**
 * 抽象建造者角色
 *      抽象建造者类Builder
 * @author Administrator
 *
 */
public interface Builder {
    //产品零件一建造方法
    public void buildPart1();
    //产品零件二建造方法
    public void buildPart2();
    //返回产品的方法
    public Product retrieveResult();

}

/**
 * 具体建造者角色
 *      具体建造者类ConcreteBuilder
 * @author Administrator
 *
 */
public class ConcreteBuilder implements Builder {
    private Product product = new Product();
    @Override
    public void buildPart1() {
        //构建产品的第一个零件
        product.setPart1("编号：007");
    }

    @Override
    public void buildPart2() {
        //构建产品的第二个零件
        product.setPart2("名称：暗黑者");
    }

    @Override
    public Product retrieveResult() {
        //返回产品
        return product;
    }
}

/**
 * 导演者角色
 *      导演者类Director
 * @author Administrator
 *
 */
public class Director {
    /**
     * 持有当前需要使用的建造器对象
     */
    private Builder builder;
    /**
     * 构造方法，传入建造器对象
     * @param builder 建造器对象
     */
    public Director(Builder builder){
        this.builder = builder;
    }
    /**
     * 产品构造方法，负责调用各个零件建造方法,比如产品的生产顺序等
     */
    public void construct(){
        builder.buildPart1();
        builder.buildPart2();
    }
}

//客户端
public class Client {

    public static void main(String[] args) {
        Builder builder = new ConcreteBuilder();
        Director director = new Director(builder);
        director.construct();
        Product product = builder.retrieveResult();
        System.out.println(product.getPart1());
        System.out.println(product.getPart2());
    }
}
```

​		**时序图**

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/20160124163803926.png" alt="这里写图片描述" style="zoom:70%;" />

​		客户端负责**创建导演者和具体建造者对象**。然后，客户端把**具体建造者对象交给导演者**，导演者操作具体建造者，开始创建产品。当产品完成后，建造者把产品返还给客户端。
​		把创建具体建造者对象的任务交给客户端而不是导演者对象，是为了将导演者对象与具体建造者对象的耦合变成动态的，从而使导演者对象可以操纵数个具体建造者对象中的任何一个。

​		**两个很重要的部分**

-   一个部分是**Builder接口**，这里是定义了如何构建各个部件，也就是知道每个部件功能如何实现
-   另外一个部分是**Director**，Director是知道如何组合来构建产品，也就是说Director负责整体的构建算法，而且通常是分步骤地来执行,也就是说如何组装这些部件。

​        不管如何变化，建造模式都存在这么两个部分，一个部分是**部件构造**。另一个部分是**整体构建的算法**。
再直白点说，建造模式的重心在于分离构建算法和具体的构造实现，从而使得构建算法可以重用。具体的构造实现可以很方便地扩展和切换，从而可以灵活地组合来构造出不同的产品对象。



###### 3.建造模式使用场景

​		假设有一个电子杂志系统，定期地向用户的电子邮件信箱发送电子杂志。用户可以通过网页订阅电子杂志，也可以通过网页结束订阅。当客户开始订阅 时，系统发送一个电子邮件表示欢迎，当客户结束订阅时，系统发送一个电子邮件表示欢送。本例子就是这个系统负责**发送“欢迎”和“欢送”邮件的模块**。在本例中，产品类就是发给某个客户的“欢迎”和“欢送”邮件，如下图所示。

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/20160124163938147.png" alt="系统类图" style="zoom:70%;" />

​		虽然在这个例子里面各个产品类均有一个共同的接口，但这仅仅是本例特有的，并不代表建造模式的特点。建造模式可以**应用到具有完全不同接口的产品类**上。大多数情况下是不知道最终构建出来的产品是什么样的，所以在标准的建造模式里面，一般是不需要对产品定义抽象接口的，因为最终构造的产品千差万别，给这些产品定义公共接口几乎是没有意义的。下图所示就是这个系统的类图。

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/20160124164002522.png" alt="系统类图" style="zoom:90%;" />

​		这个系统含有**客户端（Client）、导演者（Director）、抽象建造者（Builder）、具体建造者（WelcomeBuilder和GoodbyeBuilder）、产品（WelcomeMessage和GoodbyeMessage）**等角色。

​		**源代码**

```Java
//抽象类AutoMessage源代码，send()操作仅仅是示意性的，并没有给出任何发送电子邮件的代码。
public abstract class AutoMessage {
    //收件人地址
 private String to;
    //发件人地址
 private String from;
    //标题
 private String subject;
    //内容
 private String body;
    //发送日期
 private Date sendDate;
    public void send(){
        System.out.println("收件人地址：" + to);
        System.out.println("发件人地址：" + from);
        System.out.println("标题：" + subject);
        System.out.println("内容：" + body);
        System.out.println("发送日期：" + sendDate);
    }
    public String getTo() {
        return to;
    }
    public void setTo(String to) {
        this.to = to;
    }
    public String getFrom() {
        return from;
    }
    public void setFrom(String from) {
        this.from = from;
    }
    public String getSubject() {
        return subject;
    }
    public void setSubject(String subject) {
        this.subject = subject;
    }
    public String getBody() {
        return body;
    }
    public void setBody(String body) {
        this.body = body;
    }
    public Date getSendDate() {
        return sendDate;
    }
    public void setSendDate(Date sendDate) {
        this.sendDate = sendDate;
    }
}

//具体产品类WelcomeMessage
public class WelcomeMessage extends AutoMessage {
    /**
     * 构造子
     */
    public WelcomeMessage(){
        System.out.println("发送欢迎信息");
    }
}

//具体产品类GoodbyeMessage
public class GoodbyeMessage extends AutoMessage{
    /**
     * 构造子
     */
    public GoodbyeMessage(){
        System.out.println("发送欢送信息");
    }
}

//抽象建造者类
public abstract class Builder {
 protected AutoMessage msg;
    //标题零件的建造方法
 public abstract void buildSubject();
    //内容零件的建造方法
 public abstract void buildBody();
    //收件人零件的建造方法
 public void buildTo(String to){
        msg.setTo(to);
    }
    //发件人零件的建造方法
 public void buildFrom(String from){
        msg.setFrom(from);
    }
    //发送时间零件的建造方法
 public void buildSendDate(){
        msg.setSendDate(new Date());
    }
    /**
     * 邮件产品完成后，用此方法发送邮件
     * 此方法相当于产品返还方法
     */
    public void sendMessage(){
        msg.send();
    }
}

//具体建造者WelcomeBuilder
public class WelcomeBuilder extends Builder {
    public WelcomeBuilder(){
        msg = new WelcomeMessage();
    }
    @Override
    public void buildBody() {
        // TODO Auto-generated method stub
 　　　　msg.setBody("欢迎内容");
    }

    @Override
    public void buildSubject() {
        // TODO Auto-generated method stub
 　　　　msg.setSubject("欢迎标题");
    }
}

//具体建造者GoodbyeBuilder
public class GoodbyeBuilder extends Builder {

    public GoodbyeBuilder(){
        msg = new GoodbyeMessage();
    }
    @Override
    public void buildBody() {
        // TODO Auto-generated method stub
 　　　　msg.setBody("欢送内容");
    }

    @Override
    public void buildSubject() {
        // TODO Auto-generated method stub
 　　　　msg.setSubject("欢送标题");
    }
}

//导演者Director，这个类提供一个construct()方法，此方法调用建造者的建造方法，包括buildTo()、 buildFrom()、buildSubject()、buildBody()、buildSendDate()等，从而一部分一部分地建造出产品对 象，既AutoMessage对象。
public class Director {
    Builder builder;
    /**
     * 构造子
     */
    public Director(Builder builder){
        this.builder = builder;
    }
    /**
     * 产品构造方法，负责调用各零件的建造方法
     */
    public void construct(String toAddress , String fromAddress){
        this.builder.buildTo(toAddress);
        this.builder.buildFrom(fromAddress);
        this.builder.buildSubject();
        this.builder.buildBody();
        this.builder.buildSendDate();
        this.builder.sendMessage();
    }
}

//客户端Client
public class Client {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
 　　　　Builder builder = new WelcomeBuilder();
        Director director = new Director(builder);
        director.construct("toAddress@45126184@qq.com", "fromAddress@110.com");
    }
}
```



###### 4.**使用建造模式构建复杂对象**

​		考虑这样一个实际应用，要创建一个保险合同的对象，里面很多属性的值都有约束，要求创建出来的对象是满足这些约束规则的。约束规则比如：保险合同通常情况下可以和个人签订，也可以和某个公司签订，但是一份保险合同不能同时与个人和公司签订。这个对象里有很多类似这样的约束，采用建造模式来构建复杂的对象，通常会对建造模式进行一定的简化，因为目标明确，就是创建某个复杂对象，因此做适当简化会使程序更简洁。大致简化如下：
​		● 由于是用Builder模式**来创建某个对象**，因此就**没有必要再定义一个Builder接口**，直接提供一个具体的建造者类就可以了。
​		● 对于创建一个复杂的对象，可能会有很**多种不同的选择和步骤**，干脆**去掉“导演者”**，把导演者的功能和Client的功能合并起来，也就是说,Client这个时候就相当于导演者，它来指导构建器类去构建需要的复杂对象。

​		**保险合同类**

```java
/**
 * 保险合同对象
 */
public class InsuranceContract {
    // 保险合同编号
    private String contractId;
    /**
     * 被保险人员的名称，同一份保险合同，要么跟人员签订，要么跟公司签订 也就是说，“被保险人员”和“被保险公司”这两个属性，不可能同时有值
     */
    private String personName;
    // 被保险公司的名称
    private String companyName;
    // 保险开始生效日期
    private long beginDate;
    // 保险失效日期，一定会大于保险开始生效日期
    private long endDate;
    // 其他数据
    private String otherData;

    // 私有构造方法
    private InsuranceContract(ConcreteBuilder builder) {
        this.contractId = builder.contractId;
        this.personName = builder.personName;
        this.companyName = builder.companyName;
        this.beginDate = builder.beginDate;
        this.endDate = builder.endDate;
        this.otherData = builder.otherData;
    }

    /**
     * 保险合同的一些操作
     */
    public void someOperation() {
        System.out.println("当前正在操作的保险合同编号为【" + this.contractId + "】");
        if(personName!=null){
            System.out.println("被保险人名称:"+personName);
        }
        if(companyName!=null){
            System.out.println("被保险公司名称:"+companyName);
        }
    }
  
    //具体构建者角色
    public static class ConcreteBuilder {
        private String contractId;
        private String personName;
        private String companyName;
        private long beginDate;
        private long endDate;
        private String otherData;

        /**
         * 构造方法，传入必须要有的参数
         * 
         * @param contractId
         *            保险合同编号
         * @param beginDate
         *            保险合同开始生效日期
         * @param endDate
         *            保险合同失效日期
         */
        public ConcreteBuilder(String contractId, long beginDate, long endDate) {
            this.contractId = contractId;
            this.beginDate = beginDate;
            this.endDate = endDate;
        }

        // 建造方法 建造被保险人员的名称
        public ConcreteBuilder setPersonName(String personName) {
            this.personName = personName;
            return this;
        }

        // 建造方法  建造被保险公司的名称
        public ConcreteBuilder setCompanyName(String companyName) {
            this.companyName = companyName;
            return this;
        }

        // 建造方法  建造其他数据
        public ConcreteBuilder setOtherData(String otherData) {
            this.otherData = otherData;
            return this;
        }

        /**
         * 构建真正的对象并返回
         * 
         * @return 构建的保险合同对象
         */
        public InsuranceContract build() {
            if (contractId == null || contractId.trim().length() == 0) {
                throw new IllegalArgumentException("合同编号不能为空");
            }
            boolean signPerson = (personName != null && personName.trim().length() > 0);
            boolean signCompany = (companyName != null && companyName.trim().length() > 0);
            if (signPerson && signCompany) {
                throw new IllegalArgumentException("一份保险合同不能同时与个人和公司签订");
            }
            if (signPerson == false && signCompany == false) {
                throw new IllegalArgumentException("一份保险合同不能没有签订对象");
            }
            if (beginDate <= 0) {
                throw new IllegalArgumentException("一份保险合同必须有开始生效的日期");
            }
            if (endDate <= 0) {
                throw new IllegalArgumentException("一份保险合同必须有失效的日期");
            }
            if (endDate < beginDate) {
                throw new IllegalArgumentException("一份保险合同的失效日期必须大于生效日期");
            }
            return new InsuranceContract(this);
        }
    }
}
```

​		**客户端**

```java 
public class Client {

    public static void main(String[] args) {
        // 创建构建器对象
        InsuranceContract.ConcreteBuilder builder = new InsuranceContract.ConcreteBuilder("9527", 123L, 456L);
        // 设置需要的数据，然后返回保险合同对象
        InsuranceContract contract = builder.setPersonName("小明").setOtherData("test").build();
        // 操作保险合同对象的方法
        contract.someOperation();
    }
}
```

　　在本例中将具体建造者合并到了产品对象中，并将产品对象的**构造函数私有化**，防止客户端不使用构建器来构建产品对象，而是直接去使用new来构建产品对象所导致的问题。另外，这个构建器的功能就是为了创建被构建的对象，完全可以不用单独一个类。



##### 4.原型模式

​		原型模式虽然是创建型的模式，但是**与工程模式没有关系**，从名字即可看出，该模式的思想就是**将一个对象作为原型**，对其进行复制、克隆，**产生一个和原对象类似的新对象**。

###### **1.原型模式定义**

​		原型模式属于**对象的创建模式**。通过给出一个原型对象来指明所有创建的对象的类型，然后用复制这个原型对象的办法创建出更多同类型的对象。简言之：就是复制粘贴。这就是选型模式的用意。在Java中，复制对象是通过clone()实现的，原型类代码如下：

```Java
public class Prototype implements Cloneable {  
    public Object clone() throws CloneNotSupportedException {  
        Prototype proto = (Prototype) super.clone();  
        return proto;  
    }  
}  
```

###### 2.原型模式结构

​		原型模式主要用于**对象的复制**，它的核心是就是类图中的**原型类Prototype**。

​		Prototype类需要具备**以下两个条件**：

-   **实现Cloneable接口**。在java语言有一个Cloneable接口，它的作用只有一个，就是在**运行时通知虚拟机可以安全地在实现了此接口的类上使用clone方法**。在java虚拟机中，只有实现了这个接口的类才可以被拷贝，否则在运行时会抛出 CloneNotSupportedException异常。
-   **重写Object类中的clone方法**。Java中，所有类的父类都是 Object类，Object类中有一个clone方法，**作用是返回对象的一个拷贝，但是其作用域protected类型的，一般的类无法调用**，因此，Prototype类需要将clone方法的作用域修改为public类型。

###### 3.浅复制与深复制

​		首先需要了解对象深、浅复制的概念：

-   **浅复制**：将一个对象复制后，基本数据类型的**变量都会重新创建**，而引用类型，指向的还是**原对象所指向的**。

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200412180848269.png" alt="image-20200412180848269" style="zoom:40%;" />

-   **深复制**：将一个对象复制后，不论是基本数据类型还有引用类型，都是**重新创建的**。简单来说，就是**深复制进行了完全彻底的复制，而浅复制不彻底。**

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200412180825788.png" alt="image-20200412180825788" style="zoom:40%;" />

###### **4.浅度克隆**

​		只负责克隆**按值传递的数据**（比如基本数据类型、String类型），而**不复制它所引用的对象**。换言之，所有的对其他对象的引用都仍然指向原来的对象。

```Java
public class Person1  implements Cloneable{

    //基本数据类型
    private int age;
    //String引用类型
    private String name;

    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }

    public Person1 clone() {
        Person1 person =null;
        try {
             person = (Person1) super.clone();
            return person;
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
            return null;
        }
    }

}

public class Client {
    public static void main(String[] args) {
        Person1 p1=new Person1();
        p1.setName("汤高");
        p1.setAge(20);

        Person p2=p1;//地址相同  只是把引用给了p2 指向同一个地址
        System.out.println(p1==p2);//true
      
        Person1 p2=p1.clone();
        //拷贝  地址不同了 指向不同的地址
        System.out.println("前后地址相同吗:  "+(p2==p1));
        System.out.println("输出p1:" +p1.getName()+"\t"+p1.getAge());
        System.out.println("输出p2:" +p2.getName()+"\t"+p2.getAge());

        //修改拷贝后的对象的属性值
        p2.setName("周思远");
        p2.setAge(19);
        System.out.println("输出p1:" +p1.getName()+"\t"+p1.getAge());
        System.out.println("输出p2:" +p2.getName()+"\t"+p2.getAge());

    }
}

//运行结果:
true
前后地址相同吗: false
输出p1:汤高 20
输出p2:汤高 20
输出p1:汤高 20
输出p2:周思远 19
```

​		通过上述测试可知对于基本类型和String类型的数据前后都是**指向不同的地址空间**,改变一个不会影响其他的对象。但是如果包含引用类型比如对象、数组、集合等,就**只会克隆引用**，结果指向**同一个引用地址**：

```Java
public class Person2  implements Cloneable{

    //基本数据类型
    private int age;
    //String引用类型
    private String name;
    //引用类型
    private List<String> friends=new ArrayList<String>();
    //对象
    private School school;

    public School getSchool() {
        return school;
    }
    public void setSchool(School school) {
        this.school = school;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }

    public List<String> getFriends() {
        return friends;
    }
    public void setFriends(List<String> friends) {
        this.friends = friends;
    }


    public Person2 clone() {
        Person2 person =null;
        try {
             person = (Person2) super.clone();
            return person;
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
            return null;
        }
    }

}

public class Client2 {
    public static void main(String[] args) {
        Person2 p1=new Person2();

        List<String> friends=new ArrayList<String>();
        friends.add("汤小高");
        friends.add("周小思");
        p1.setFriends(friends);

        Person2 p2=p1.clone();
        System.out.println(p1.getFriends());
        System.out.println(p2.getFriends());

        friends.add("TSY");
        p1.setFriends(friends);
        System.out.println(p1.getFriends());
        System.out.println(p2.getFriends());

    }
}

//结果：
[汤小高, 周小思]
[汤小高, 周小思]
[汤小高, 周小思, TSY]
[汤小高, 周小思, TSY]

public class Client3 {
    public static void main(String[] args) {
        Person2 p1=new Person2();
				//修改学校名
        School school=new School();
        school.setName("清华");
        p1.setSchool(school);
				//克隆一个对象
        Person2 p2=p1.clone();

        System.out.println(p1.getSchool()==p2.getSchool());
        System.out.println(p1.getSchool());
        System.out.println(p2.getSchool());
        school.setName("北大");
        p1.setSchool(school);

        System.out.println(p1.getSchool());//北大
        System.out.println(p2.getSchool());//北大
    }
}

//结果：
true
学校名: 清华
学校名: 清华
学校名: 北大
学校名: 北大
```

###### **5.深度克隆**

​		除了浅度克隆要克隆的值外，还**负责克隆引用类型的数据**。那些引用其他对象的变量将指向被复制过的新对象，而不再是原有的那些被引用的对象。换言之，深度克隆把要复制的对象所引用的**对象都复制了一遍**，而这种对被引用到的对象的复制叫做间接复制。
​		深度克隆要深入到多少层，是一个不易确定的问题。在决定以深度克隆的方式复制一个对象的时候，必须决定对间接复制的对象时采取浅度克隆还是继续采用深度克隆。因此，在采取深度克隆时，**需要决定多深才算深**。此外，在深度克隆的过程中，很可能会**出现循环引用的问题，必须小心处理**。

```Java
//要实现深度克隆 必须修改clone()方法
public class Person2  implements Cloneable{

    //基本数据类型
    private int age;
    //String引用类型
    private String name;
    //引用类型
    private List<String> friends=new ArrayList<String>();
    //对象
    private School school;

    public School getSchool() {
        return school;
    }
    public void setSchool(School school) {
        this.school = school;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }

    public List<String> getFriends() {
        return friends;
    }
    public void setFriends(List<String> friends) {
        this.friends = friends;
    }
   //修改后的clone方法
    public Person2 clone() {
        try {
            Person2 person = (Person2) super.clone();
            if(this.getFriends()!=null){
                List<String> friends=new ArrayList<String>();
                for(String friend:this.getFriends()){
                    friends.add(friend);
                }
                person.setFriends(friends);
            }

            if(this.getSchool()!=null){
                 School school=new School();
                 school.setName(this.getSchool().getName());
                 person.setSchool(school);

            }
            return person;
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
            return null;
        }
    }

}

//测试类同上
  
//Client2结果：
[汤小高, 周小思]
[汤小高, 周小思]
[汤小高, 周小思, TSY]
  
//Client3结果：
false
学校名: 清华
学校名: 清华
学校名: 北大
学校名: 清华
```



##### 七类结构模式 

##### 5.适配器模式

 		适配器模式将某个类的**接口转换成客户端期望的另一个接口表示**，目的是消除由于**接口不匹配所造成的类的兼容性问题**。主要分为三类：**类的适配器模式**、**对象的适配器模式**、**接口的适配器模式**。

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200413005609200.png" alt="image-20200413005609200" style="zoom:30%;" />

###### 1.适配器模式的概念

　	**1.适配器模式概念与作用**

​		适配器模式把一个类的接口变换成客户端所期待的另一种接口，从而使原本因接口不匹配而无法在一起工作的两个类能够在一起工作。即Adapter模式使得原本由于接口不兼容而不能一起工作的那些类可以在一起工作。例如220V充电接口转110V接口。

​		**2.适配器模式中的角色**

-   **目标接口**（Target）：客户所期待的接口。目标可以是具体的或抽象的类，也可以是接口。
-   **需要适配的类**（Adaptee）：需要适配的类或适配者类。
-   **适配器**（Adapter）：通过**包装一个需要适配的类或对象或方法**，把原接口转换成目标接口。

​        **3.适配器模式的使用场景**

-   **类的适配器模式**：当希望将**一个类**转换成满足**另一个新接口**的类时，可以使用类的适配器模式，创建一个新类，继承原有的类，实现新的接口即可。
-   **对象的适配器模式**：当希望将**一个对象**转换成满足**另一个新接口的对象**时，可以创建一个Wrapper类，持有原类的一个实例，在Wrapper类的方法中，调用实例的方法就行。
-   **接口的适配器模式**：当**不希望实现一个接口中所有的方法时**，可以创建**一个抽象类Wrapper**，实现所有方法，我们写别的类的时候，继承抽象类即可。

​        **4.适配器模式的优点**

-   **更好的复用性**：系统需要使用现有的类，而此类的接口不符合系统的需要。那么通过适配器模式就可以让这些功能得到更好的复用。
-   **更好的扩展性**：在实现适配器功能的时候，可以**调用自己开发的功能**，从而自然地扩展系统的功能。

​       **5.适配器模式的缺点**

　　过多的使用适配器，会让系统非常零乱，不易整体进行把握。比如，明明看到调用的是A接口，其实内部被适配成了B接口的实现，一个系统如果太多出现这种情况，无异于一场灾难。因此如果不是很有必要，可以不使用适配器，而是**直接对系统进行重构**。





###### 2.类的适配器模式

​		核心思想就是：有一个Source类，拥有一个方法，待适配，目标接口是Targetable，通过Adapter类，将Source的功能扩展到Targetable里.

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200413002146674.png" alt="image-20200413002146674" style="zoom:40%;" />

```Java
//目标接口，或称为标准接口 
public interface Target {
    //普通功能
    public void request();
}

//已存在的、具有特殊功能、但不符合我们既有的标准接口的类  
public class Adaptee {
    public void specificRequest() {
        System.out.println("被适配类...我是两孔插座  具有特殊功能");
    }
}

//适配器类，继承了被适配类，同时实现标准接口
public class Adapter extends Adaptee implements Target  {
    @Override
    public void request() {
        System.out.println("我是适配器类 我能适配任何两孔插座 让它正常工作");
        this.specificRequest();
    }
 }


public class Client {
    public static void main(String[] args) {
         Target adapter = new Adapter(); 
         //通过适配器调用特殊功能
         adapter.request();  
    }
}
```





###### 3.对象的适配器模式

​		基本思路和类的适配器模式相同，只是将Adapter类作修改，这次不继承Source类，而是持有Source类的实例，以达到解决兼容性的问题。（采用对象组合方式实现）

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/20160128161731671.jpeg" alt="这里写图片描述" style="zoom:80%;" />

```Java
//目标接口，或称为标准接口 
public interface Target {
    //普通功能
    public void request();
}

//已存在的、具有特殊功能、但不符合我们既有的标准接口的类  
public class Adaptee {
    public void specificRequest() {
        System.out.println("被适配类...我是两孔插座  具有特殊功能");
    }
} 

//适配器类，直接关联被适配类，同时实现标准接口  
class Adapter implements Target {
    // 直接关联被适配类
    private Adaptee adaptee;

    // 可以通过构造函数传入具体需要适配的被适配类对象
    public Adapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }

    public void request() {
        // 这里是使用委托的方式完成特殊功能
        System.out.println("我是适配器类 我能适配任何两孔插座 让它正常工作");
        this.adaptee.specificRequest();
    }
}

public class Client {
    public static void main(String[] args) {
        //使用特殊功能类，即适配类，  
        // 需要先创建一个被适配类的对象作为参数  
        Target adapter = new Adapter(new Adaptee());  
        adapter.request();  
    }
}
```

​		从类图中我们知道需要修改的只不过就是 **Adapter 类的内部结构**，即 Adapter 自身必须**先拥有一个被适配类的对象**，再把具体的特殊功能**委托给这个对象来实现**。使用对象适配器模式，可以使得 Adapter 类（适配类）根据传入的 Adaptee 对象达到适配**多个不同被适配类的功能**，当然，此时我们可以为多个被适配类提取出一个接口或抽象类。这样看起来的话，似乎对象适配器模式更加灵活一点。

​		**类适配器和对象适配器的权衡**

　　● 类适配器使用**对象继承的方式**，是静态的定义方式；而对象适配器使用**对象组合的方式**，是动态组合的方式。对于类适配器，仅仅引入了一个对象，并**不需要额外的引用来间接得到Adaptee**。对于对象适配器，需要**额外的引用来间接得到Adaptee**。

　　● 对于对象适配器，一个适配器可以**把多种不同的源适配到同一个目标**。换言之，同一个适配器可以把**源类和它的子类**都适配到目标接口。因为对象适配器采用的是对象组合的关系，只要对象类型正确，是不是子类都无所谓。

​		建议尽量使用**对象适配器**的实现方式，多用**合成/聚合**、少用**继承**。当然，具体问题具体分析，根据需要来选用实现方式，最适合的才是最好的。



###### 4.接口的适配器模式

​		第三种适配器模式是**接口的适配器模式**，接口的适配器是这样的：有时我们写的一个**接口中有多个抽象方法**，当我们写该接口的实现类时，必须实现该接口的所有方法，这明显有时比较浪费，因为并不是所有的方法都是我们需要的，有时只需要某一些，此处为了解决这个问题，我们引入了接口的适配器模式，借助于**一个抽象类**，该抽象类实现了该接口，实现了所有的方法，而我们不和原始的接口打交道，只和该抽象类取得联系，所以我们写一个类，**继承该抽象类，重写我们需要的方法**就行。看一下类图：

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200413003209779.png" alt="image-20200413003209779" style="zoom:35%;" />

​		这个很好理解，在实际开发中，我们也常会遇到这种接口中定义了太多的方法，以致于有时我们在一些实现类中并不是都需要。

```Java
public interface Sourceable {  
      
    public void method1();  
    public void method2();  
}  
public abstract class Wrapper2 implements Sourceable{  
      
    public void method1(){}  
    public void method2(){}  
}  
public class SourceSub1 extends Wrapper2 {  
    public void method1(){  
        System.out.println("the sourceable interface's first Sub1!");  
    }  
}  
public class SourceSub2 extends Wrapper2 {  
    public void method2(){  
        System.out.println("the sourceable interface's second Sub2!");  
    }  
}  
public class WrapperTest {  
  
    public static void main(String[] args) {  
        Sourceable source1 = new SourceSub1();  
        Sourceable source2 = new SourceSub2();  
          
        source1.method1();  
        source1.method2();  
        source2.method1();  
        source2.method2();  
    }  
}  

//测试输出：
the sourceable interface's first Sub1!
the sourceable interface's second Sub2!
```



###### 5.**缺省适配模式**

　　缺省适配(Default Adapter)模式为一个接口**提供缺省实现**，这样子类型可以从这个缺省实现进行扩展，而不必从原有接口进行扩展。作为适配器模式的一个特例，缺省是适配模式在JAVA语言中有着特殊的应用。就是一个抽象类对功能接口的所有功能做空实现，然后子类继承这个抽象类，这样就可以对部分功能进行实现或拓展了。

​		**功能接口**

```java
/**
 * 功能接口 谈恋爱后具备如下功能
 *  牵手
 *  接吻
 *  看电影
 *  情侣晚餐
 *  回家见父母
 */
public interface Love {
    //牵手  该功能是恋爱后经常会有的波
    public void hand();

    //接吻
    public void kiss();

    //看电影
    public void movie();

    //情侣晚餐
    public void have();

    //回家见父母
    public void backHome();

    //女朋友是谁
    public String getGirlFriend();

}
```

​		**缺省适配类**

```java
/**
 * 抽象类  
 * 交往类  实现恋爱接口
 *  哈哈 交往多了自然就谈爱了吗 嘻嘻嘻!
 *  但是交往毕竟不是谈恋爱  虽然它具有了谈恋爱的雏形 但是毕竟不是
 *  所以对谈恋爱的所有功能做了空实现 只有架子
 *  相当于没有 但是未来可能会有 
 * @author Administrator
 *
 */
public abstract class Communication implements Love {
    //全是空实现  
    @Override
    public void hand() {}

    @Override
    public void kiss() {}

    @Override
    public void movie() {}

    @Override
    public void have() {}

    @Override
    public void backHome() {}

    @Override
    public String getGirlFriend() {
        return null;
    }

}
```

​		**具体实现**

```Java
/**
 * 具体的类 boy 继承了交往Communication抽象类 
 * 本类已经从交往慢慢恋爱了  所以它已经具备了谈恋爱接口
 * 的所有功能  但是有些功能他每天不可能天天都调用
 * 比如 看电影 情侣晚餐 回家见父母等等 
 * (因为分隔两地,不能天天见面 但是他们一见面就和过年一样高兴
 * 他们就可以调用恋爱中的一些功能
 * 比如他们 可以看电影 共进晚餐
 * )
 * 
 * @author Administrator
 *
 */

public class boy extends Communication {
        //看电影
        public void movie(){
            System.out.println("我们一起看电影 哈哈");
        }

        //情侣晚餐
        public void have(){
            System.out.println("我们一起吃晚餐 哈哈");
        }

        //女朋友是谁
        public String getGirlFriend(){
            return "周思远";
        }
}
```

​		**客户端：**

```Java
public class Client {
    public static void main(String[] args) {

        Love tanggao=new boy();
        tanggao.movie();
        tanggao.have();

        System.out.println("女朋友是 :"+tanggao.getGirlFriend());
    }
}

//结果:
我们一起看电影 哈哈
我们一起吃晚餐 哈哈
女朋友是 :周思远
```

​		在很多情况下，必须让**一个具体类实现某一个接口**，但是这个类又用不到接口所规定的所有的方法。通常的处理方法是，这个具体类要实现所有的方法，那些有用的方法要有实现，那些**没有用的方法也要有空的、平庸的实现**。这些空的方法是一种浪费，有时也是一种混乱。除非看过这些空方法的代码，程序员可能会以为这些方法不是空的。即便他知道其中有一些方法是空的，也不一定知道哪些方法是空的，哪些方法不是空的，除非看过这些方法的源代码或是文档。

　	缺省适配模式可以很好的处理这一情况。可以设计**一个抽象的适配器类实现接口**，此抽象类要给接口所要求的每一种方法都提供一个空的方法。就像具体帮助类boy一样,他只实现某一部分功能,因为他只用到了某一部分功能而已，如果以后要用，再实现。但是今天他不用，就没必要该功能了！

​		适配器模式的**用意是要改变源的接口**，以便于目标接口相容。缺省适配的用意稍有不同，它是为了方便建立一个不平庸的适配器类而提供的一种平庸实现。在任何时候，如果不准备实现一个接口的所有方法时，就可以使用“缺省适配模式”制造一个抽象类，给出所有方法的平庸的具体实现。这样，从这个抽象类再继承下去的子类就不必实现所有的方法了



##### 6.装饰模式

​		顾名思义，装饰模式就是给**一个对象增加一些新的功能**，而且是动态的，要求装饰对象和被装饰对象实现同一个接口，装饰对象持有被装饰对象的实例，关系图如下：

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/e1b8b6a3-0150-31ae-8f77-7c3d888b6f80.jpg" alt="img" style="zoom:75%;" />

​		Source类是被装饰类，Decorator类是一个装饰类，可以为Source类动态的添加一些功能.

###### 1.装饰模式的概念

-   **装饰模式(Decorator)的定义**：又名包装(Wrapper)模式，装饰模式以对客户端透明的方式扩展对象的功能，是继承关系的一个替代方案。
-   装饰模式以对客户端透明的方式**动态的给一个对象附加上更多的责任**。换言之客户端并不会觉的对象在装饰前和装饰后有什么区别。
-   装饰模式可以在不创造更多的子类的模式下，**将对象的功能加以扩展**。

**装饰模式的特点：**

-   装饰对象和真实对象具有**相同的接口**，这样客户端对象就可以以真实对象的**相同的方式**和装饰对象交互。
-   装饰对象包含**一个真实对象的引用(reference)**.
-   装饰对象接受所有来自客户端的请求，它把这些请求**转发给真实的对象**。
-   装饰对象可以在**转发这些请求以前或者以后增加一些附加的功能**。这样就能确保在运行时，不用修改给定对象结构就可以在外部增加附加的功能。在面向对象的程序设计中，通常是**使用继承的关系来扩展给定类的功能**。

**装饰模式与类继承的区别**

-   装饰模式是**一种动态行为**，对已经存在类进行随意组合，而类的继承是一种**静态的行为**，一个类定义成什么样的，该类的对象便具有什么样的功能，无法动态的改变。
-   装饰模式**扩展的是对象的功能**，不需要增加类的数量，而类继承**扩展的是类的功能**，在继承的关系中，如果我们想增加一个对象的功能，我们只能通过继承关系，在子类中增加方法。
-   装饰模式是在不改变原类文件和使用继承的情况下，动态的扩展一个对象的功能，它是通过创建一个包装对象，也就是装饰来包裹真是的对象。

**装饰模式的使用场景**

-   需要**扩展一个类的功能**。
-   **动态的为一个对象增加功能，而且还能动态撤销**。（继承不能做到这一点，继承的功能是静态的，不能动态增删。）缺点：产生过多**相似的对象，不易排错**！



###### **2.装饰模式的结构**

​		装饰模式的**类图**如下：

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/20160129181831723.png" alt="装饰模式类图" style="zoom:70%;" />

​		**装饰模式中的角色有：**

​	● **抽象构件(Component)角色**：给出一个抽象接口，以规范准备接收附加责任的对象。

 	● **具体构件(ConcreteComponent)角色**：定义一个**将要接收附加责任的类**。

​	● **装饰(Decorator)角色**：持有一个**构件(Component)对象的实例**，并定义一个与抽象构件接口一致的接口。

​	● **具体装饰(ConcreteDecorator)角色**：负责**给构件对象“贴上”附加的责任**。

​		**源代码**

```java
//抽象构件角色
public interface Component {

    public void sampleOperation();

}

//具体构件角色
public class ConcreteComponent implements Component {

    @Override
    public void sampleOperation() {
        // 写相关的业务代码    }
}

//装饰角色
public class Decorator implements Component{
    private Component component;

    public Decorator(Component component){
        this.component = component;
    }

    @Override
    public void sampleOperation() {
        // 委派给构件        
    component.sampleOperation();
    }
}

//具体装饰角色
public class ConcreteDecoratorA extends Decorator {

    public ConcreteDecoratorA(Component component) {
        super(component);
    }

    @Override
    public void sampleOperation() {
　　　　　super.sampleOperation();
        // 写相关的业务代码    }
}
public class ConcreteDecoratorB extends Decorator {

    public ConcreteDecoratorB(Component component) {
        super(component);
    }

    @Override
    public void sampleOperation() {
　　　　  super.sampleOperation();
        // 写相关的业务代码    }
}
```

###### **3.具体案列**

​		本例中抽象构建角色由**Programmer程序员接口扮演**，具体构件角色由**类汤高扮演**， 他实现了程序员具有的功能，装饰构件由 **类Derector扮演** 它必须也实现抽象构件接口，具体装饰构件角色由 **类Hacker(黑客)** 和**类 SoftwareAchitect(架构师)扮演**，具体程序员汤高有编程能力,可以给他赋予更多能力 每赋予一种能力,他就多一个技能 这是通过装饰构件实现的

```java
/**
 * 抽象构件角色
 *  程序员接口   程序员具有编程的能力
 * @author Administrator
 *
 */
public interface Programmer {
    //编程
    public void programme();
}
/**
 * 具体构件角色  
 *  汤高是一个具体的程序员  
 * 那么他就具有编程能力
 * @author Administrator
 *
 */
public class 汤高 implements Programmer {

    @Override
    public void programme() {
        System.out.println("我是一个程序员, 我能编程");
    }

}
/**
 * 装饰角色
 *  
 * @author Administrator
 *
 */
public class Derector implements Programmer{
    private Programmer programmer;

    public Derector(Programmer programmer) {
        this.programmer = programmer;
    }

    @Override
    public void programme() {
        programmer.programme();
        //附加的责任或者功能
    }
}
/**
 * 　具体装饰角色 1 
 *  黑客类   他具有附加的功能  他能入侵别人的电脑
 * @author Administrator
 *
 */
public class Hacker extends Derector {

    public Hacker(Programmer programmer) {
        super(programmer);
    }

    @Override
    public void programme() {
        super.programme();
        //附加的责任或者功能
        System.out.println("我具有黑客的技能   我能入侵别人的电脑");
    }

}
/**
 * 　具体装饰角色2  
 *  软件架构师类   他具有附加的功能  能设计总个网站或系统的骨
 * @author Administrator
 *
 */
public class SoftwareArchitect extends Derector {

    public SoftwareArchitect(Programmer programmer) {
        super(programmer);
    }

    @Override
    public void programme() {
        super.programme();
        //附加的责任或者功能
        System.out.println("我具有架构师的技能  我能设计总个网站或系统的骨架");
    }
}
//客户端
public class Client {

    public static void main(String[] args) {
        //创建构件对象   汤高 ->他是一个具体的程序员 
        //但是现在他只有编程能力  那怎么行
        //必须赋予他更大的能力  不然怎么赚大钱!
        //所以 上帝给他装饰了一番  瞬间提神了他的B格
        Programmer programmer=new 汤高();

        //装饰类登场  屌丝  我来装饰你  让你具有更大的能力 
        Derector hacker=new Hacker(programmer);
        //这下汤高这个屌丝程序员就具有黑客的技能包了
        //这下就没有谁敢叫他屌丝了吧  哈哈  不然分分钟让你电脑崩溃
        System.out.println("第一次装饰");
        hacker.programme();

        //程序员还不满足  他还要更多的技能  因为他要逆袭
        //所以上帝再给他装饰了一下 
        //在他具有黑客技能的基础上另外赋予了他架构师的功能
        System.out.println("--------------第二次装饰");
        Derector  achitect=new SoftwareArchitect(hacker);

        achitect.programme();
        //也可以一步装饰两个技能 因为他们有共同的父类抽象构件接口 Programmer
        System.out.println("------------一步装饰两个技能");
        Derector achitect1=new SoftwareArchitect(   new  Hacker(new 汤高() ) );
        achitect1.programme();
    }

}

//结果：
第一次装饰
我是一个程序员, 我能编程
我具有黑客的技能 我能入侵别人的电脑
————–第二次装饰
我是一个程序员, 我能编程
我具有黑客的技能 我能入侵别人的电脑
我具有架构师的技能 我能设计总个网站或系统的骨架
————一步装饰两个技能
我是一个程序员, 我能编程
我具有黑客的技能 我能入侵别人的电脑
我具有架构师的技能 我能设计总个网站或系统的骨架
```



###### 4.装饰模式、适配器模式、代理模式区别

-   **适配器模式**：一个适配允许通常因为接口不兼容而不能在一起工作的类工作在一起，做法是将类自己的接口包裹在一个已存在的类中。适配器的**特点在于兼容**，从代码上的特点来说，适配类与原有的类**具有相同的接口**，并且**持有新的目标对象**。

-   **装饰器模式**：原有的不能满足现有的需求，对**原有的进行增强**。装饰器模式特点在于增强，他的特点是**被装饰类和所有的装饰类必须实现同一个接口**，而且必须持有被装饰的对象，可以无限装饰。
-   **代理模式**，同一个类而去调用另一个类的方法，**不对这个方法进行直接操作**。代理模式的特点在于**隔离**，隔离调用类和被调用类的关系，**通过一个代理类去调用**。



##### 7.代理模式 

​		其实每个模式名称就表明了该模式的作用，代理模式就是**多一个代理类**出来，**替原对象进行一些操作**，比如我们在租房子的时候会去找中介，为什么呢？因为你对该地区房屋的信息掌握的不够全面，希望找一个更熟悉的人去帮你做，此处的代理就是这个意思。再如我们有的时候打官司，需要请律师，因为律师在法律方面有专长，可以替我们进行操作，表达我们的想法。先来看看关系图：

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200413155027434.png" alt="image-20200413155027434" style="zoom:40%;" />

###### 1.代理模式的特点

​		代理模式是**对象的结构模式**。代理模式**给某一个对象提供一个代理对象**，并由代理对象控制对原对象的引用

**代理模式的应用场景**

如果已有的方法在使用的时候需要对原有的方法进行改进，此时有两种办法：

1、修改原有的方法来适应。这样违反了“对扩展开放，对修改关闭”的原则。

2、就是采用一个代理类调用原有的方法，且对产生的结果进行控制。这种方法就是代理模式。

使用代理模式，可以将功能划分的更加清晰，有助于后期维护！



###### 2.静态代理

在代理模式中的角色：

-   **抽象主题角色**：声明了**目标对象和代理对象的共同接口**，这样一来在任何可以使用目标对象的地方都可以使用代理对象。
-   **真实主题角色**：定义了**代理对象所代表的目标对象**。
-   **代理主题角色**：代理对象内部**含有目标对象的引用**，从而可以在任何时候操作目标对象；代理对象提供一个与目标对象相同的接口，以便**可以在任何时候替代目标对象**。代理对象通常在客户端调用传递给目标对象之前或之后，执行某个操作，而不是单纯地将调用传递给目标对象。它可以增加一些真实主题里面没有的功能。

​        生活中的例子：过年加班比较忙，没空去买火车票，这时可以打个电话到附近的票务中心，叫他们帮你买张回家的火车票，当然这会附加额外的劳务费。但要清楚票务中心自己并不卖票，只有火车站才真正卖票，票务中心卖给你的票其实是通过火车站实现的。这个例子，你就是“客户”，票务中心就是“代理角色”，火车站是“真实角色”，卖票称为“**抽象角色**”！

​		**静态代理**

-   **优点**：对真实对象进行封装，**不会修改目标类的代码**。
-   **缺点**：1.多个不同类型目标对象需要代理时，就**需要建立多个代理类**，造成类的膨胀
    	              2.**代码的冗余**
                	       3.**编译期加入，不够灵活**

```Java
//抽象角色：声明真实对象和代理对象的共同接口；
public interface  TicketManager { 
    /**
     * 售票
     */
    public  void  soldTicket();
    /**
     * 改签
     */
    public void changeTicket();
    /**
     * 退票
     */
    public void returnTicket();
}   
//真实主题角色
public class TicketManagerImpl implements TicketManager {

    @Override
    public void soldTicket() {
        //checkIdentity();
        System.out.println("售票");
    }

    @Override
    public void changeTicket(){
        //checkIdentity();
        System.out.println("改签");
    }

    @Override
    public void returnTicket() {
        //checkIdentity();
        System.out.println("退票");
    }

    /**
     * 身份验证
     */
    public void checkIdentity(){
        System.out.println("身份验证");
    }
}
//代理主题角色（添加了身份验证功能）
public class StaticProxyTicketManager implements TicketManager {
    TicketManager ticketManager;//目标对象的引用

    public StaticProxyTicketManager(TicketManager ticketManager) {
        this.ticketManager = ticketManager;
    }

    @Override
    public void soldTicket() {
        checkIdentity();
        ticketManager.soldTicket();
    }

    @Override
    public void changeTicket() {
        checkIdentity();
        ticketManager.changeTicket();
    }

    @Override
    public void returnTicket() {
        checkIdentity();
        ticketManager.changeTicket();
    }
    /**
     * 身份验证
     */
    public void checkIdentity(){
        System.out.println("身份验证--------------");
    }

}
//第二个代理主题角色(添加了日志功能)
//代理类  实现同一个接口
public class LogProxy implements TicketManager {
    TicketManager ticketManager;//目标类的引用
  
    public LogProxy(TicketManager ticketManager){
        this.ticketManager=ticketManager;
    }
    @Override
    public void soldTicket() {
        ticketManager.soldTicket();
        log();//后置增强
    }

    @Override
    public void changeTicket() {
        ticketManager.changeTicket();
        log();
    }

    @Override
    public void returnTicket() {
        ticketManager.returnTicket();
        log();

    }
    //增强
    private void log() {
        System.out.println("日志...");

    }

}
//客户端
public class Test {
    public static void main(String[] args) {
        //装饰模式   new TicketManagerImpl()  真实的目标对象
        //TicketManager tm=new StaticProxyTicketManager(new TicketManagerImpl());
        TicketManager tm=new LogProxy(new StaticProxyTicketManager(new TicketManagerImpl()));

        tm.soldTicket();
        tm.changeTicket();
        tm.returnTicket();
    }
}
//结果：
身份验证————–
售票
日志…
身份验证————–
改签
日志…
身份验证————–
改签
日志…
```


​		从上面例子可以看出 **客户端通过代理来购票** 而代理实际上不能卖票给客户，他实际上是通过**目标对象卖票给客户的**，也就是说他是通过真实主题的目标对象实现给客户端卖票的功能，他只是一个中介，但我们可以在它里面增加一些功能，比如**身份验证**或者**宣传打广告**等其他的功能。静态代理类：在程序运行前，代理类的.class文件就已经存在了，已确定被代理的对象



###### 3.动态代理

​		**动态代理（Dynamic Proxy）**：相比静态代理，动态代理**具有更强的灵活性**，因为它不用在我们设计实现的时候就指定某一个代理类来代理哪一个被代理对象，我们可以**把这种指定延迟到程序运行时由JVM来实现**。所谓代理，就是需要代理类和被代理类有**相同的对外接口或者说成服务**，所以代理类一般都必须实现了所有被代理类已实现的接口，因为接口就是**制定了一系列对外服务的标准**。

**1.JDK实现动态代理**

​		正因为动态代理有这样灵活的特性，所以我们在设计动态代理类（DynamicProxy）时**不用显式地让它实现与真实主题类（RealSubject）相同的接口（interface）**，而是把这种实现推迟到运行时。

​		为了能让DynamicProxy类能够在运行时才去实现**RealSubject类**已实现的一系列接口并执行接口中相关的方法操作，需要让 DynamicProxy类**实现JDK自带的java.lang.reflect.InvocationHandler接口**，该接口中的**invoke() 方法能够让DynamicProxy实例在运行时调用被代理类的“对外服务”**，即调用被代理类需要对外实现的所有接口中的方法，也就是完成对真实方法的调 用，Java帮助文档中**称这些真实方法为处理程序**。

​		按照上面所述，我们肯定**必须先把被代理类RealSubject已实现的所有interface都加载到JVM中**，不然JVM怎么能够找到这些方法呢？明白了这个道理，那么我们就可以**创建一个被代理类的实例**，获得该实例的类加载器ClassLoader。所谓的类加载器ClassLoader，就是具有某个类的类定义，即类的内部相关结构（包括继承树、方法区等等）。更重要的是，动态代理模式**可以使得我们在不改变原来已有的代码结构的情况下，对原来的“真实方法”进行扩展、增强其功能**，并且可以达到**控制被代理对象的行为的目的**。请详看下面代码中的DynamicProxy类，其中必须实现的invoke()方法**在调用被代理类的真实方法的前后都可进行一定的特殊操作**。这是动态代理最明显的优点。类图如下：

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/20160328211321804.png" alt="这里写图片描述" style="zoom:70%;" />

```Java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class DynamicProxyTicketManager implements InvocationHandler {
    private Object targetObject;
    /**
     * 目标的初始化方法，根据目标生成代理类
     * 
     * @param targetObject
     * @return
     */
    public Object newProxyInstance(Object targetObject) {
        this.targetObject = targetObject;
        // 第一个参数，目标对象 的装载器
        // 第二个参数，目标接口已实现的所有接口，而这些是动态代理类要实现的接口列表
        // 第三个参数， 调用实现了InvocationHandler的对象生成动态代理实例，当你一调用代理，代理就会调用InvocationHandler的invoke方法
        return Proxy.newProxyInstance(targetObject.getClass().getClassLoader(), targetObject.getClass().getInterfaces(),
                this);
    }

    /**
     * 反射，这样你可以在不知道具体的类的情况下，根据配置的参数去调用一个类的方法。在灵活编程的时候非常有用。
     */
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

        // 检查
        checkIdentity();
        Object ret = null;
        try {
            // 调用目标方法
            ret = method.invoke(targetObject, args);
            // 执行成功，打印成功信息
            log();
        } catch (Exception e) {
            e.printStackTrace();
            // 失败时，打印失败信息
            System.out.println("error-->>" + method.getName());
            throw e;
        }
        return ret;
    }

    /**
     * 身份验证
     */
    public void checkIdentity(){
        System.out.println("身份验证--------------");
    }
    public void log(){
        System.out.println("日志..." );
    }

}
```

客户端

```Java
public class Test {
    public static void main(String[] args) {
        DynamicProxyTicketManager dynamicProxyTicketManager=new DynamicProxyTicketManager();
        TicketManager tm=(TicketManager) dynamicProxyTicketManager.newProxyInstance(new TicketManagerImpl());

        tm.soldTicket();
        tm.changeTicket();
        tm.returnTicket();
    }
}
```

**优点：**

-   一个动态代理类更加简单了，可以**解决创建多个静态代理的麻烦**，避免不断的重复多余的代码
-   调用目标代码时，会在方法“运行时”动态的加入，决定是什么类型，才调谁，灵活

**缺点**：

-   系统灵活了，但是相比而言，**效率降低了**，比静态代理慢一点
-   动态代理比静态代理在代码的**可读性上差了一点**，不太容易理解
-   JDK动态代理只能**对实现了接口的类进行代理**



**2.Cglib实现动态代理**

​		AOP的源码中用到了**两种动态代理来实现拦截切入功能**：**jdk动态代理和cglib动态代理**。

​		两种方法同时存在，各有优劣。jdk动态代理是**由java内部的反射机制来实现**的 ，cglib动态代理**底层则是借助asm来实现的**。总的来说，反射机制在生成类的过程中比较高效，而asm在生成类之后的**相关执行过程中比较高效**（可以通过将asm生成的类进行缓存，这样解决asm生成类过程低效问题）。还有一点必须注意：jdk动态代理的应用前提，必须是**目标类基于统一的接口**。如果没有 上述前提，jdk动态代理不能应用。由此可以看出，jdk动态代理有一定的局限性，cglib这种**第三方类库实现的动态代理应用更加广泛**， 且在效率上更有优势。

​		JDK的动态代理机制只能代理实现了接口的类，否则不能实现JDK的动态代理，cglib是针对类来实现代理的，他的原理是**对指定的目标类生成一个子类**，并覆盖其中方法实现增强，但因为**采用的是继承**，所以不能对final修饰的类进行代理。

​		**介绍：**

```Java
CGLIB的核心类：
net.sf.cglib.proxy.Enhancer – 主要的增强类
net.sf.cglib.proxy.MethodInterceptor – 主要的方法拦截类，它是Callback接口的子接口，需要用户实现
net.sf.cglib.proxy.MethodProxy – JDK的java.lang.reflect.Method类的代理类，可以方便的实现对源对象方法的调用,如使用：
Object o = methodProxy.invokeSuper(proxy, args);//虽然第一个参数是被代理对象，也不会出现死循环的问题。
net.sf.cglib.proxy.MethodInterceptor接口是最通用的回调（callback）类型，它经常被基于代理的AOP用来实现拦截（intercept）方法的调用。这个接口只定义了一个方法
public Object intercept(Object object, java.lang.reflect.Method method,
Object[] args, MethodProxy proxy) throws Throwable;
第一个参数是代理对像，第二和第三个参数分别是拦截的方法和方法的参数。原来的方法可能通过使用java.lang.reflect.Method 对象的一般反射调用，或者使用 net.sf.cglib.proxy.MethodProxy对象调用。net.sf.cglib.proxy.MethodProxy通常被首选使 用，因为它更快
```

​		**源代码**：

```Java
public class CglibDynamicProxyTicketManager implements MethodInterceptor  {
    private Object targetObject;//目标对象
    /** 
     * 创建代理对象 
     *  
     * @param targetObject 
     * @return 
     */  
    public Object getInstance(Object targetObject) {  
        this.targetObject = targetObject;  
        Enhancer enhancer = new Enhancer();  // 用这个类来创建代理对象(被代理类的子类)： 并设置父类；设置回调；
        enhancer.setSuperclass(this.targetObject.getClass()); // 设置被代理类作为其父类的代理目标
        // 回调方法  
        enhancer.setCallback(this);  // 设置回调--当这个代理对象的方法被调用时 回调方法intercept()会被执行
        // 创建代理对象  
        return enhancer.create();  
    }  

    @Override
    //回调方法
    // methodProxy 代理的类的方法
    /**
     * methodProxy 会调用父类(目标对象)的被代理的方法,比如soldTicket方法等
     */
    public Object intercept(Object obj, Method method, Object[] args,  
            MethodProxy methodProxy) throws Throwable {
        Object result = null;
        checkIdentity();//前置增强
        result=methodProxy.invokeSuper(obj, args); //调用新生成的cglib的代理对象 所属的父类的被代理的方法
         log();//后置增强
        return result;
    }

    /**
     * 身份验证
     */
    public void checkIdentity(){
        System.out.println("身份验证--------------");
    }
    public void log(){
        System.out.println("日志..." );
    }


}
```

客户端

```Java
public class Test {
    public static void main(String[] args) {
        CglibDynamicProxyTicketManager cglibdynamicProxyTicketManager=new CglibDynamicProxyTicketManager();
        //生成代理对象
        TicketManager tm=(TicketManager) cglibdynamicProxyTicketManager.getInstance(new TicketManagerImpl());

        tm.soldTicket();//当调用代理对象的被代理对象的方法时  会自动回调 代理类中的Intercept()方法
        tm.changeTicket();
        tm.returnTicket();
    }
}
//结果同上
```



##### 8.外观模式（Facade）

​		外观模式（Facade）是为了解决**类与类之家的依赖关系的**，像spring一样，可以将类和类之间的关系配置到配置文件中，而外观模式就是将他们的**关系放在一个Facade类中**，降低了类类之间的耦合度，该模式中没有涉及到接口，看下类图：（我们以一个计算机的启动过程为例）

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/eebe2103-6ced-35f2-8664-3a2e8a557f81.jpg" alt="img" style="zoom:75%;" />

```java
public class Computer {  
    private CPU cpu;  
    private Memory memory;  
    private Disk disk;  
      
    public Computer(){  
        cpu = new CPU();  
        memory = new Memory();  
        disk = new Disk();  
    }  
      
    public void startup(){  
        System.out.println("start the computer!");  
        cpu.startup();  
        memory.startup();  
        disk.startup();  
        System.out.println("start computer finished!");  
    }  
      
    public void shutdown(){  
        System.out.println("begin to close the computer!");  
        cpu.shutdown();  
        memory.shutdown();  
        disk.shutdown();  
        System.out.println("computer closed!");  
    }  
}  
```

​		如果我们没有Computer类，那么，CPU、Memory、Disk他们之间将会相互持有实例，产生关系，这样会造成严重的依赖，修改一个类，可能会带来其他类的修改，这不是我们想要看到的，有了Computer类，他们之间的关系被放在了Computer类里，这样就起到了解耦的作用.

###### 1.外观模式的概念

​		Facade（外观）模式为子系统中的各类（或结构与方法）提供一个简明一致的界面，隐藏子系统的复杂性，使子系统更加容易使用。

​		**结构**

-   **门面(Facade)角色** ：客户端可以调用这个角色的方法。此角色知晓相关的（一个或者多个）子系统的功能和责任。在正常情况下，本角色会将所有从客户端发来的请求委派到相应的子系统去。
-   **子系统(SubSystem)角色** ：可以同时有一个或者多个子系统。每个子系统都不是一个单独的类，而**是一个类的集合**（如上面的子系统就是由SystemA、SystemB、 SystemC三个类组合而成）。每个子系统都可以被客户端直接调用，或者被门面角色调用。子系统并不知道门面的存在，对于子系统而言，门面仅仅是另外一 个客户端而已。

​        **特点**

-   外观模式为复杂子系统提供了**一个简单接口**，并不为子系统添加新的功能和行为。
-   外观模式实现了子系统与客户之间的**松耦合关系**。
-   外观模式没有封装子系统的类，只是**提供了简单的接口**。 如果应用需要，它并不限制客户使用子系统类。因此可以再系统易用性与通用性之间选择。
-   外观模式注重的是**简化接口**，它更多的时候是从架构的层次去看整个系统，而并非单个类的层次。

###### 2.外观模式案例

```Java
//子系统(SubSystem)角色
public class SystemA {
    public void doA(){
        System.out.println("子系统A的功能");
    }
}
//子系统(SubSystem)角色
public class SystemB {
    public void doB(){
        System.out.println("子系统B的功能");
    }
}
//子系统(SubSystem)角色
public class SystemC {
    public void doC(){
        System.out.println("子系统C的功能");
    }
}
/**
 * 门面(Facade)角色
 * @author Administrator
 *
 */
public class Facade {
    private SystemA systemA;
    private SystemB systemB;
    private SystemC systemC;

    Facade(){
        systemA=new SystemA();
        systemB=new SystemB();
        systemC=new SystemC();
    }
    // 方法 满足客户端需要的功能
    public void doAB(){
        systemA.doA();
        systemB.doB();
    }
    // 方法 满足客户端需要的功能
    public void doABC(){
        systemA.doA();
        systemB.doB();
        systemC.doC();
    }

}
public class Client {

    public static void main(String[] args) {
        Facade f=new Facade();
        System.out.println("客户1需要的两个子系统功能");
        f.doAB();
        System.out.println("----------------------");
        System.out.println("客户2需要的三个子系统功能");
        f.doABC();
    }

}
//结果：
客户1需要的两个子系统功能
子系统A的功能
子系统B的功能

客户2需要的三个子系统功能
子系统A的功能
子系统B的功能
子系统C的功能
```



##### 9.桥接模式（Bridge）

​		桥接模式就是把事物和其具体实现分开，使他们可以各自独立的变化。桥接的用意是：**将抽象化与实现化解耦，使得二者可以独立变化**，像我们常用的JDBC桥DriverManager一样，JDBC进行连接数据库的时候，在各个数据库之间进行切换，**基本不需要动太多的代码，甚至丝毫不用动**，原因就是**JDBC提供统一接口**，每个数据库提供各自的实现，用一个叫做数据库驱动的程序来桥接就行了。我们来看看关系图：

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/35f0b172-b976-3757-bb51-c65d5c9ce68e.jpg" alt="img" style="zoom:67%;" />

###### **1.桥接模式的定义**

​		将**抽象部分与实现(行为)部分分离**，使它们都可以独立的变化。桥接模式的做法是把变化部分(实现)抽象出来，使变化部分与主类(抽象)分离开来，从而将**多个维度的变化彻底分离**。最后，提供一个管理类(如下面的引擎类)来组合不同维度上的变化，通过这种组合来满足业务的需要。

​		**应用场景**

-   如果你**不希望在抽象和实现部分采用固定的绑定关系**，可以采用桥接模式，来把抽象和实现部分分开，然后在程序运行期间来动态的设置抽象部分需要用到的具体的实现，还可以动态切换具体的实现。
-   如果出现抽象部分和实现部分**都应该可以扩展的情况**，可以采用桥接模式，让抽象部分和实现部分可以独立的变化，从而可以灵活的进行单独扩展，而不是搅在一起，扩展一边会影响到另一边。
-   如果希望实现**部分的修改**，不会对客户产生影响，可以采用桥接模式，客户是面向抽象的接口在运行，实现部分的修改，可以独立于抽象部分，也就不会对客户产生影响了，也可以说对客户是透明的。
-   如果采用继承的实现方案，会**导致产生很多子类**，对于这种情况，可以考虑采用桥接模式，分析功能变化的原因，看看是否能分离成不同的纬度，然后通过桥接模式来分离它们，从而减少子类的数目。



###### 2.桥接模式的案例

​		本案例是实现汽车安装引擎的功能。汽车有两种 奔驰和宝马 他们安装的引擎不同实现有两种方法

​		**1.传统方法**

```Java
public interface  Car {

    public void installEngine200();

    public void installEngine300();
}

public class Bmw implements Car {

    @Override
    public void installEngine200() {
        System.out.println("Bmw车组装Engine200");
    }

    @Override
    public void installEngine300() {
        System.out.println("Bmw车组装Engine300");      
    }
}

public class Benz implements Car {

    @Override
    public void installEngine200() {
        System.out.println("Benz车组装Engine200");
    }

    @Override
    public void installEngine300() {
        System.out.println("Benz车组装Engine300");      
    }
}


public class Client {

    public static void main(String[] args) {
        //奔驰车安装200引擎
        Car benz=new Benz();
        benz.installEngine200();
        //宝马车安装300引擎
        Car bwm=new Bwm();
        bwm.installEngine300();
    }
}
//运行结果：
Benz车组装Engine200
Bwm车组装Engine300
```

​		缺点: 只要在Car接口中增加一个引擎类型的方法 那么它的具体实现类中也得增加一个**空实现**(如果该车不需要这个引擎) 比如我再增加一个型号400的引擎 ,相应的奔驰和宝马中都得增加该方法 但是奔驰车不需要这种引擎 这样是不是造成了代码的冗余,所以这种方法不好拓展，这就要用到第二种方法 ：**桥接模式**

​		**2.使用桥接模式**

```java
//Implementor : Engine 定义实现接口(也就是引擎接口)。
//与实现(行为)部分
public interface Engine {
    public void addEngine();
}
//ConcreteImplementor : Engine200 ;Engine300 实现 引擎接口中方法。
//具体实现接口
public class Engine200 implements Engine {

    @Override
    public void addEngine() {
        System.out.println("组装Engine200");
    }

}
public class Engine300 implements Engine {

    @Override
    public void addEngine() {
        System.out.println("组装Engine300");
    }
}
//Abstraction : Car 定义抽象接口。
//抽象部分
public abstract class Car {
    private Engine engine;// 持有一个实现部分对象，形成聚合关系


    public Car(Engine engine) {
        this.engine = engine;
    }

    public Engine getEngine() {
        return engine;
    }

    public void setEngine(Engine engine) {
        this.engine = engine;
    }

    public abstract void install();
}

//RefinedAbstraction :Benz ;Bwm 扩展 Abstraction 类。
public class Benz extends Car {
    public Benz(Engine engine) {
        super(engine);
    }

    @Override
    public void install() {
        System.out.println("Benz车安装");
        this.getEngine().addEngine();
    }
}
public class Bwm extends Car {

    public Bwm(Engine engine) {
        super(engine);
    }
    @Override
    public void install() {
        System.out.println("Bwm车安装");
        this.getEngine().addEngine();
    }
}
//测试代码
public class Client {

    public static void main(String[] args) {
        //创建实现(行为)  ->Engine引擎
        //第一种 引擎  
        Engine engine200=new Engine200();
        //第二种 引擎
        Engine engine300=new Engine300();
        //创建抽象    ->车
        Car benz=new Benz(engine200);
        benz.install();

        Car bwm=new Bwm(engine300);
        bwm.install();
    }
}
//运行结果:
Benz车安装
组装Engine200
Bwm车安装
组装Engine300
```



##### 10.组合模式（Composite）

​		组合模式有时又叫**部分-整体**模式在处理类似树形结构的问题时比较方便，看看关系图：

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200413181627864.png" alt="image-20200413181627864" style="zoom:50%;" />

​		**使用场景**：将多个**对象组合在一起进行操作**，常用于**表示树形结构中**，例如二叉树，数等。

```java
blic class TreeNode {  
      
    private String name;  
    private TreeNode parent;  
    private Vector<TreeNode> children = new Vector<TreeNode>();  
      
    public TreeNode(String name){  
        this.name = name;  
    }  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public TreeNode getParent() {  
        return parent;  
    }  
  
    public void setParent(TreeNode parent) {  
        this.parent = parent;  
    }  
      
    //添加孩子节点  
    public void add(TreeNode node){  
        children.add(node);  
    }  
      
    //删除孩子节点  
    public void remove(TreeNode node){  
        children.remove(node);  
    }  
      
    //取得孩子节点  
    public Enumeration<TreeNode> getChildren(){  
        return children.elements();  
    }  
}  
public class Tree {  
  
    TreeNode root = null;  
  
    public Tree(String name) {  
        root = new TreeNode(name);  
    }  
  
    public static void main(String[] args) {  
        Tree tree = new Tree("A");  
        TreeNode nodeB = new TreeNode("B");  
        TreeNode nodeC = new TreeNode("C");  
          
        nodeB.add(nodeC);  
        tree.root.add(nodeB);  
        System.out.println("build the tree finished!");  
    }  
}  
```



##### **11.享元模式（Flyweight）**

​		享元模式的主要目的是**实现对象的共享**，即共享池，当系统中对象多的时候可以**减少内存的开销**，**通常与工厂模式一起使用**。

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/f7aae0dd-b250-3829-bb07-49d87069bfbb.jpg" alt="img" style="zoom:70%;" />

​		FlyWeightFactory负责**创建和管理享元单元**，当一个客户端请求时，工厂需要检查当前对象池中是否有符合条件的对象，如果有，**就返回已经存在的对象**，如果没有，**则创建一个新对象**，FlyWeight是超类。一提到共享池，我们很容易联想到Java里面的JDBC连接池，想想每个连接的特点，我们不难总结出：适用于作共享的一些个对象，他们有一些共有的属性，就拿数据库连接池来说，url、driverClassName、username、password及dbname，这些**属性对于每个连接来说都是一样的**，所以就适合用**享元模式来处理**，建一个工厂类，将上述类似属性作为内部数据，其它的作为外部数据，在方法调用时，当做参数传进来，这样就节省了空间，减少了实例的数量。

​		通过**连接池的管理**，实现了数据库连接的共享，不需要每一次都重新创建连接，节省了数据库重新创建的开销，提升了系统的性能！

```Java
public class ConnectionPool {  
      
    private Vector<Connection> pool;  
      
    /*公有属性*/  
    private String url = "jdbc:mysql://localhost:3306/test";  
    private String username = "root";  
    private String password = "root";  
    private String driverClassName = "com.mysql.jdbc.Driver";  
  
    private int poolSize = 100;  
    private static ConnectionPool instance = null;  
    Connection conn = null;  
  
    /*构造方法，做一些初始化工作*/  
    private ConnectionPool() {  
        pool = new Vector<Connection>(poolSize);  
  
        for (int i = 0; i < poolSize; i++) {  
            try {  
                Class.forName(driverClassName);  
                conn = DriverManager.getConnection(url, username, password);  
                pool.add(conn);  
            } catch (ClassNotFoundException e) {  
                e.printStackTrace();  
            } catch (SQLException e) {  
                e.printStackTrace();  
            }  
        }  
    }  
  
    /* 返回连接到连接池 */  
    public synchronized void release() {  
        pool.add(conn);  
    }  
  
    /* 返回连接池中的一个数据库连接 */  
    public synchronized Connection getConnection() {  
        if (pool.size() > 0) {  
            Connection conn = pool.get(0);  
            pool.remove(conn);  
            return conn;  
        } else {  
            return null;  
        }  
    }  
}  
```



##### 四类关系模式【6.12】

​		先来张图，看看这11中模式的关系：

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200413182208087.png" alt="image-20200413182208087" style="zoom:35%;" />

​		第一类：通过父类与子类的关系进行实现。

​		第二类：两个类之间。

​		第三类：类的状态。

​		第四类：通过中间类

##### 12.父类与子类模式

###### 1.策略模式

​		策略模式定义了一系列算法，并将每个算法封装起来，使他们可以**相互替换**，且算法的变化不会影响到使用算法的客户。需要设计一个接口，为一系列实现类提供**统一的方法**，多个实现类实现该接口，设计一个抽象类（可有可无，属于辅助类），提供辅助函数，关系图如下：

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/2319a2c3-7ebd-3ee3-b389-1548074ea9c6.jpg" alt="img" style="zoom:75%;" />

###### 2.模板方法模式（Template Method）

​		解释一下**模板方法模式**，就是指：一个抽象类中，有一个主方法，再定义1...n个方法，可以是抽象的，也可以是实际的方法，定义一个类，继承该抽象类，重写抽象方法，通过调用抽象类，实现对子类的调用，先看个关系图：

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/c3d57775-ddf9-302b-9dfe-c65967518d3c.jpg" alt="img" style="zoom:75%;" />

​		就是在AbstractCalculator类中定义一个主方法calculate，calculate()调用spilt()等，Plus和Minus分别继承AbstractCalculator类，通过对AbstractCalculator的调用实现对子类的调用



##### 13.两个类之间的模式

###### 1.观察者模式（Observer）

​		包括这个模式在内的接下来的四个模式，都是类和类之间的关系，**不涉及到继承**，学的时候应该记得归纳。观察者模式很好理解，类似于**邮件订阅和RSS订阅**，当我们浏览一些博客或wiki时，经常会看到RSS图标，就这的意思是，当你订阅了该文章，如果后续有更新，会及时通知你。其实，简单来讲就一句话：当一个对象变化时，其它**依赖该对象的对象都会收到通知**，并且随着变化！对象之间是一种**一对多的关系**。先来看看关系图：

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/d588525c-fbad-3040-971c-69b2716c67a4.jpg" alt="img" style="zoom:75%;" />

​		我解释下这些类的作用：MySubject类就是我们的主对象，Observer1和Observer2是依赖于MySubject的对象，当MySubject变化时，Observer1和Observer2必然变化。AbstractSubject类中定义着需要监控的对象列表，可以对其进行修改：**增加或删除被监控对象，且当MySubject变化时，负责通知在列表内存在的对象**。

```Java
//一个Observer接口：
public interface Observer {  
    public void update();  
}  
//两个实现类：
public class Observer1 implements Observer {  
  
    @Override  
    public void update() {  
        System.out.println("observer1 has received!");  
    }  
}  
public class Observer2 implements Observer {  
  
    @Override  
    public void update() {  
        System.out.println("observer2 has received!");  
    }  
  
}  
//Subject接口及实现类：
public interface Subject {  
      
    /*增加观察者*/  
    public void add(Observer observer);  
      
    /*删除观察者*/  
    public void del(Observer observer);  
      
    /*通知所有的观察者*/  
    public void notifyObservers();  
      
    /*自身的操作*/  
    public void operation();  
} 
public abstract class AbstractSubject implements Subject {  
  
    private Vector<Observer> vector = new Vector<Observer>();  
    @Override  
    public void add(Observer observer) {  
        vector.add(observer);  
    }  
  
    @Override  
    public void del(Observer observer) {  
        vector.remove(observer);  
    }  
  
    @Override  
    public void notifyObservers() {  
        Enumeration<Observer> enumo = vector.elements();  
        while(enumo.hasMoreElements()){  
            enumo.nextElement().update();  
        }  
    }  
}  
public class MySubject extends AbstractSubject {  
  
    @Override  
    public void operation() {  
        System.out.println("update self!");  
        notifyObservers();  
    }  
  
}  
//测试类
public class ObserverTest {  
  
    public static void main(String[] args) {  
        Subject sub = new MySubject();  
        sub.add(new Observer1());  
        sub.add(new Observer2());  
          
        sub.operation();  
    }  
  
}  
//输出：

update self!
observer1 has received!
observer2 has received!
```



###### **2.迭代子模式（Iterator）**

​		顾名思义，迭代器模式就是**顺序访问聚集中的对象**，一般来说，集合中非常常见，如果对集合类比较熟悉的话，理解本模式会十分轻松。这句话包含两层意思：一是需要遍历的对象，即聚集对象，二是迭代器对象，用于对聚集对象进行遍历访问。

​		**模拟一个迭代器：**

```Java
//两个接口
public interface Collection {  
      
    public Iterator iterator();  
      
    /*取得集合元素*/  
    public Object get(int i);  
      
    /*取得集合大小*/  
    public int size();  
}  
public interface Iterator {  
    //前移  
    public Object previous();  
      
    //后移  
    public Object next();  
    public boolean hasNext();  
      
    //取得第一个元素  
    public Object first();  
}  

//两个实现：
public class MyCollection implements Collection {  
  
    public String string[] = {"A","B","C","D","E"};  
    @Override  
    public Iterator iterator() {  
        return new MyIterator(this);  
    }  
  
    @Override  
    public Object get(int i) {  
        return string[i];  
    }  
  
    @Override  
    public int size() {  
        return string.length;  
    }  
}  
public class MyIterator implements Iterator {  
  
    private Collection collection;  
    private int pos = -1;  
      
    public MyIterator(Collection collection){  
        this.collection = collection;  
    }  
      
    @Override  
    public Object previous() {  
        if(pos > 0){  
            pos--;  
        }  
        return collection.get(pos);  
    }  
  
    @Override  
    public Object next() {     
        if(pos<collection.size()-1){  
            pos++;  
        }  
        return collection.get(pos);  
    }  
  
    @Override  
    public boolean hasNext() {  
        if(pos<collection.size()-1){  
            return true;  
        }else{  
            return false;  
        }  
    }  
  
    @Override  
    public Object first() {  
        pos = 0;  
        return collection.get(pos);  
    }  
  
}  

//测试类
public class Test {  
  
    public static void main(String[] args) {  
        Collection collection = new MyCollection();  
        Iterator it = collection.iterator();  
          
        while(it.hasNext()){  
            System.out.println(it.next());  
        }  
    }  
}  
//输出：
A B C D E
```

​		此处我们貌似模拟了一个集合类的过程，感觉是不是很爽？其实JDK中各个类也都是这些基本的东西，加一些设计模式，再加一些优化放到一起的，只要我们把这些东西学会了，掌握好了，我们也可以写出自己的集合类，甚至框架！



###### 3.责任链模式（Chain of Responsibility）

​		接下来我们将要谈谈责任链模式，有多个对象，**每个对象持有对下一个对象的引用，这样就会形成一条链**，请求在这条链上传递，直到某一对象决定处理该请求。但是发出者并不清楚到底最终那个对象会处理该请求，所以，责任链模式可以实现，在隐瞒客户端的情况下，对系统进行动态的调整。先看看关系图： 

 <img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/729a82ce-0987-347c-a4f1-bf64dee59ddb.jpg" alt="img" style="zoom:75%;" />

 

​		Abstracthandler类提供了**get和set方法**，方便MyHandle类设置和修改引用对象，MyHandle类是核心，实例化后生成一系列相互持有的对象，构成一条链。



###### 4.命令模式（Command）

​		命令模式很好理解，举个例子，司令员下令让士兵去干件事情，从整个事情的角度来考虑，司令员的作用是，发出口令，口令经过传递，传到了士兵耳朵里，士兵去执行。这个过程好在，三者相互解耦，任何一方都不用去依赖其他人，只需要做好自己的事儿就行，司令员要的是结果，不会去关注到底士兵是怎么实现的。

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/98cda4fc-00b1-300d-a25b-63229f0f1cbd.jpg" alt="img" style="zoom:75%;" />

​		Invoker是**调用者**（司令员），Receiver是**被调用者**（士兵），MyCommand是命令，实现了Command接口，持有接收对象。

​		命令模式的目的就是达到**命令的发出者和执行者之间解耦，实现请求和执行分开**，熟悉Struts的同学应该知道，Struts其实就是一种将请求和呈现分离的技术，其中必然涉及命令模式的思想！

​		其实每个设计模式都是很重要的一种思想，看上去很熟，其实是因为我们在学到的东西中都有涉及，尽管有时我们并不知道，其实在Java本身的设计之中处处都有体现，**像AWT、JDBC、集合类、IO管道或者是Web框架**，里面设计模式无处不在。



##### 14.类的状态模式

###### **1.备忘录模式（Memento）**

​		主要目的是**保存一个对象的某个状态**，以便在适当的时候恢复对象，个人觉得叫**备份模式**更形象些，通俗的讲下：假设有原始类A，A中有各种属性，A可以决定需要备份的属性，备忘录类B是用来存储A的一些内部状态，类C呢，就是一个用来存储备忘录的，且只能存储，不能修改等操作。做个图来分析一下：

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/853d5c5a-9b7b-3341-a72e-abd3cbc3c81f.jpg" alt="img" style="zoom:75%;" />

​		Original类是原始类，里面有需要保存的属性value及创建一个备忘录类，用来保存value值。Memento类是备忘录类，Storage类是存储备忘录的类，持有Memento类的实例，该模式很好理解。

```Java
public class Original {  
      
    private String value;  
      
    public String getValue() {  
        return value;  
    }  
  
    public void setValue(String value) {  
        this.value = value;  
    }  
  
    public Original(String value) {  
        this.value = value;  
    }  
  
    public Memento createMemento(){  
        return new Memento(value);  
    }  
      
    public void restoreMemento(Memento memento){  
        this.value = memento.getValue();  
    }  
}  
public class Memento {  
      
    private String value;  
  
    public Memento(String value) {  
        this.value = value;  
    }  
  
    public String getValue() {  
        return value;  
    }  
  
    public void setValue(String value) {  
        this.value = value;  
    }  
}  
public class Storage {  
      
    private Memento memento;  
      
    public Storage(Memento memento) {  
        this.memento = memento;  
    }  
  
    public Memento getMemento() {  
        return memento;  
    }  
  
    public void setMemento(Memento memento) {  
        this.memento = memento;  
    }  
}  
public class Test {  
  
    public static void main(String[] args) {  
          
        // 创建原始类  
        Original origi = new Original("egg");  
  
        // 创建备忘录  
        Storage storage = new Storage(origi.createMemento());  
  
        // 修改原始类的状态  
        System.out.println("初始化状态为：" + origi.getValue());  
        origi.setValue("niu");  
        System.out.println("修改后的状态为：" + origi.getValue());  
  
        // 回复原始类的状态  
        origi.restoreMemento(storage.getMemento());  
        System.out.println("恢复后的状态为：" + origi.getValue());  
    }  
}  
//输出：
初始化状态为：egg
修改后的状态为：niu
恢复后的状态为：egg
```

​		简单描述下：新建原始类时，value被初始化为egg，后经过修改，将value的值置为niu，最后倒数第二行进行恢复状态，结果成功恢复了。其实我觉得这个模式叫“备份-恢复”模式最形象。

###### 2.状态模式（State）

​		核心思想就是：**当对象的状态改变时，同时改变其行为**，很好理解！就拿QQ来说，有几种状态，在线、隐身、忙碌等，每个状态对应不同的操作，而且你的好友也能看到你的状态，所以，状态模式就两点：1、可以通过改变状态来获得不同的行为。2、你的好友能同时看到你的变化。看图：

![img](/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/006156d2-f41f-3019-a194-b872a59ca426.jpg)

​		State类是个状态类，Context类可以实现切换.



##### 15.通过中间类的模式

###### 1.访问者模式（Visitor）

​		访问者模式把数据结构和作用于结构上的操作解耦合，使得操作集合可相对自由地演化。访问者模式适用于数据结构相对稳定算法又易变化的系统。因为访问者模式使得算法操作增加变得容易。若系统数据结构对象易于变化，经常有新的数据对象增加进来，则不适合使用访问者模式。访问者模式的优点是增加操作很容易，因为增加操作意味着增加新的访问者。访问者模式将有关行为集中到一个访问者对象中，其改变不影响系统数据结构。其缺点就是增加新的数据结构很困难。—— From 百度百科

​		简单来说，访问者模式就是一种**分离对象数据结构与行为的方法**，通过这种分离，可达到为一个被访问者动态添加新的操作而无需做其它的修改的效果。简单关系图：

![img](/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/96bd38f3-2888-3cc5-b90f-0e7542dc5845.jpg)

​		该模式适用场景：如果我们想为一个现有的类增加新功能，不得不考虑几个事情：1、新功能会不会与现有功能出现兼容性问题？2、以后会不会再需要添加？3、如果类不允许修改代码怎么办？面对这些问题，最好的解决方法就是使用访问者模式，访问者模式**适用于数据结构相对稳定的系统**，把数据结构和算法解耦，

###### 2.中介者模式（Mediator）

​		中介者模式也是用来降低类类之间的耦合的，因为如果类类之间有依赖关系的话，不利于功能的拓展和维护，因为只要修改一个对象，其它关联的对象都得进行修改。如果使用中介者模式，只需关心和Mediator类的关系，具体类类之间的关系及调度交给Mediator就行，这有点像spring容器的作用。

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200413185157203.png" alt="image-20200413185157203" style="zoom:50%;" />

###### 3.解释器模式（Interpreter）

​		解释器模式是我们最后一讲，一般主要应用在OOP开发中的编译器的开发中，所以适用面比较窄。

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200413185244680.png" alt="image-20200413185244680" style="zoom:50%;" />

​		解释器模式用来做各种各样的解释器，如正则表达式等的解释器等等！