### Java技术栈之日志

#### 日志的作用

​		在项目开发中，都不可避免的使用到日志。没有日志虽然不会影响项目的正确运行，但是没有日志的项目可以说是不完整的。日志在**调试**，**错误**或者**异常定位**，数据分析中的作用是不言而喻的。

##### 调试

在Java项目调试时，查看栈信息可以方便地知道当前程序的运行状态，输出的日志**便于记录程序在之前的运行结果**。如果你大量使用System.out或者System.err，这是一种最方便最有效的方法，但显得不够专业。

##### 错误定位

不要以为项目能正确跑起来就可以高枕无忧，项目在运行一段时候后，可能由于数据问题，网络问题，内存问题等出现异常。这时日志可以帮助开发或者运维人员**快速定位错误位置**，提出解决方案。

##### 数据分析

大数据的兴起，使得大量的日志分析成为可能，ELK也让日志分析门槛降低了很多。日志中蕴含了**大量的用户数据**，包括**点击行为**，**兴趣偏好**等，用户画像对于公司下一步的战略方向有一定指引作用。



#### 常用日志框架

Log4j，Logback，Log4j2性能比较与日志门面SLF4J简介

##### Log4j

Log4j是Apache的一个Java的日志库，通过使用Log4j，我们可以控制日志信息输送的目的地（控制台、文件、数据库等）；我们也可以控制每一条日志的输出格式；通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程。

##### Logback

Logback，一个“可靠、通用、快速而又灵活的Java日志框架”。logback当前分成三个模块：logback-core，logback- classic和logback-access。logback-core是其它两个模块的基础模块。logback-classic是log4j的一个改良版本。此外logback-classic完整实现SLF4J API使你可以很方便地更换成其它日志系统，如log4j或JDK14 Logging。logback-access模块与Servlet容器（如Tomcat和Jetty）集成，以提供HTTP访问日志功能。请注意，您可以在logback-core之上轻松构建自己的模块。

##### Log4j2

Apache Log4j 2是对Log4j的升级，它比其前身Log4j 1.x提供了重大改进，并提供了Logback中可用的许多改进，同时修复了Logback架构中的一些问题。

现在最优秀的Java日志框架是Log4j2，没有之一。根据官方的测试表明，在多线程环境下，Log4j2的异步日志表现更加优秀。在异步日志中，Log4j2使用独立的线程去执行I/O操作，可以极大地提升应用程序的性能。

##### 性能比较

在官方的测试中，**Log4j1/Logback/Log4j2**三个日志框架的异步日志性能比较如下图所示。

其中，`Loggers all async`是基于[LMAX Disruptor](http://lmax-exchange.github.com/disruptor/)实现的。可见Log4j2的异步日志性能是最棒的。

![image-20200309202609821](/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200309202609821.png)



##### 日志门面SLF4J

上述介绍的是一些日志框架的实现（Log4j、Logback、log4j2），这里我们需要用日志门面来解决系统与日志实现框架的耦合性。SLF4J，即简单日志门面（Simple Logging Facade for Java），它不是一个真正的日志实现，而是一个抽象层（ abstraction layer），它允许你在后台使用任意一个日志实现。

![image-20200309202818708](/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200309202818708.png)

前面介绍的几种日志框架一样，每一种日志框架都有自己单独的API，要使用对应的框架就要使用其对应的API，这就大大的增加应用程序代码对于日志框架的耦合性。使用了SLF4J后，对于应用程序来说，无论底层的日志框架如何变，应用程序不需要修改任意一行代码，就可以直接上线了。



#### Apache Log4j2详解

##### 简介

Apache Log4j 2是对Log4j的升级，它比其前身Log4j 1.x提供了重大改进，并提供了Logback中可用的许多改进，同时修复了Logback架构中的一些问题。是目前最优秀的Java日志框架，没有之一。

##### 特征

###### API分离

Log4j的API与实现分开，使应用程序开发人员可以清楚地了解可以使用哪些类和方法，同时确保向前兼容性。这使Log4j团队能够以安全且兼容的方式进行改进。

###### 性能提升

Log4j 2包含基于LMAX Disruptor库的下一代异步记录器。在多线程场景中，异步记录器的吞吐量比Log4j 1.x和Logback高18倍，延迟低。

###### 自动重新加载配置

与Logback一样，Log4j 2可以在修改时自动重新加载其配置。与Logback不同，它会在重新配置发生时不会丢失日志事件。

###### 高级过滤

与Logback一样，Log4j 2支持基于Log事件中的上下文数据，标记，正则表达式和其他组件进行过滤。此外，过滤器还可以与记录器关联。与Logback不同，您可以在任何这些情况下使用通用的Filter类。

###### 插件架构

Log4j使用插件模式配置组件。因此，您无需编写代码来创建和配置Appender，Layout，Pattern Converter等。在配置了的情况下，Log4j自动识别插件并使用它们。

###### 无垃圾机制

在稳态日志记录期间，Log4j 2 在独立应用程序中是无垃圾的，在Web应用程序中是低垃圾。这减少了垃圾收集器的压力，并且可以提供更好的响应性能。

##### Log4j2调用

###### 引用依赖

在一般项目中使用Log4j2至少需要引用log4j-api-2.x和log4j-core-2.x这两个jar包。

```Java
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.12.0</version>
</dependency>

<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-api</artifactId>
    <version>2.12.0</version>
</dependency>
```

在spring boot项目中使用Log4j2

```Java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <!-- 排除spring boot默认日志logback -->
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>

<!-- 引入log4j2依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
```

小编在使用中遇到一些坑，同时引入spring-boot-starter和spring-boot-starter-web依赖时，需要在spring-boot-starter中排除spring-boot-starter-logging依赖，因为spring-boot-starter-web依赖于spring-boot-starter，根据Maven依赖关系，在spring-boot-starter中排除spring-boot-starter-logging依赖才能排除成功。

###### 添加配置文件

默认情况下，Log4j2在classpath下查找名为log4j2.xml的配置文件。你也可以使用Java启动命令指定配置文件的全路径。-Dlog4j.configurationFile=opt/demo/log4j2.xml，你还可以使用Java代码指定配置文件路径

```Java
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.core.LoggerContext;

import java.io.File;

public static void main(String[] args) {

    LoggerContext loggerContext = (LoggerContext) LogManager.getContext(false);
    File file = new File("opt/demo/log4j2.xml");
    loggerContext.setConfigLocation(file.toURI());

}
```

##### 同步日志与异步日志

###### 同步日志

所谓同步日志，即当输出日志时，必须等待日志输出语句执行完毕后，才能执行后面的业务逻辑语句。

###### 混合同步和异步日志

Log4j-2.9及更高版本在类路径上需要disruptor-3.3.4.jar或更高版本。在Log4j-2.9之前，需要disruptor-3.0.0.jar或更高版本。无需将系统属性“Log4jContextSelector”设置为任何值。

可以在配置中组合同步和异步记录器。这为您提供了更大的灵活性，但代价是性能略有下降（与使所有记录器异步相比）。使用<asyncRoot>或<asyncLogger> 配置元素指定需要异步的记录器。配置只能包含一个根记录器（<root> 或<asyncRoot>元素），但是可以组合异步和非异步记录器。例如，包含<asyncLogger>元素的配置文件也可以包含<root>和同步记录器的元素。

默认情况下，异步记录器不会将位置传递给I / O线程。如果您的某个布局或自定义过滤器需要位置信息，则需要在所有相关记录器的配置中设置“includeLocation = true”，包括根记录器。

##### Log4j2配置文件详解

###### Configuration

根节点，有status和monitorInterval等多个属性

status的值有 “trace”, “debug”, “info”, “warn”, “error” and “fatal”，用于控制log4j2日志框架本身的日志级别，如果将stratus设置为较低的级别就会看到很多关于log4j2本身的日志，如加载log4j2配置文件的路径等信息

monitorInterval，含义是每隔多少秒重新读取配置文件，可以不重启应用的情况下修改配置

###### Properties

属性。使用来定义常量，以便在其他配置项中引用，该配置是可选的，例如定义日志的存放位置

###### Appenders

Appenders是输出源，用于定义日志输出的地方。
log4j2支持的输出源有很多，有控制台ConsoleAppender、文件FileAppender、AsyncAppender、RandomAccessFileAppender、RollingFileAppender、RollingRandomAccessFile 等

###### Filters

Filters决定日志事件能否被输出。过滤条件有三个值：`ACCEPT(接受)`，`DENY(拒绝)`，`NEUTRAL(中立)`。

ThresholdFilter

输出warn级别一下的日志

```
<Filters>
    <!--如果是error级别拒绝，设置 onMismatch="NEUTRAL" 可以让日志经过后续的过滤器-->
    <ThresholdFilter level="error" onMatch="DENY" onMismatch="NEUTRAL"/>
    <!--如果是debug\info\warn级别的日志输出-->
    <ThresholdFilter level="debug" onMatch="ACCEPT" onMismatch="DENY"/>
</Filters>
```


只输出error级别以上的日志

```
<Filters>
    <ThresholdFilter level="error" onMatch="ACCEPT" onMismatch="DENY"/>
</Filters>
```

TimeFilter

时间过滤器可用于将过滤器限制为仅当天的某个部分。

```
<Filters>
    <!-- 只允许在每天的 8点~8点半 之间输出日志 -->
    <TimeFilter start="08:00:00" end="08:30:00" onMatch="ACCEPT" onMismatch="DENY" />
</Filters>
```

###### PatternLayout

用于指定输出日志的格式。

PatternLayout：控制台或文件输出源（Console、File、RollingRandomAccessFile）都必须包含一个PatternLayout节点，用于指定输出文件的格式（如 日志输出的时间 文件 方法 行数 等格式），例如 pattern="%d{yyyy-MM-dd HH:mm:ss,SSS} [%t] %-5level %logger{0} - %msg%n"

```
  %d 输出日志日期格式，如%d{yyyy-MM-dd HH:mm:ss,SSS}输出2012-11-02 14:34:02,123
  %t 输出当前线程名称
  %-5level 输出日志级别，-5表示左对齐并且固定输出5个字符，如果不足在右边补空格
  %logger 输出logger名称
  %msg 日志信息
  %n 换行

  其他常用的占位符有：
  %F 输出所在的类文件名，如Log4j2Test.java
  %L 输出行号
  %M 输出所在方法名
  %l 输出语句所在的行数, 包括类名、方法名、文件名、行数
```

###### Policy & Strategy

`Policy`是用来控制日志文件何时(When)进行滚动的；`Strategy`是用来控制日志文件如何(How)进行滚动的。

如果配置的是`RollingFile`或`RollingRandomAccessFile`，则必须配置一个`Policy`。

###### Loggers

Loggers节点，常见的有两种：Root和Logger。
`Root`节点用来指定项目的根日志，如果没有单独指定`Logger`，那么就会默认使用该Root日志输出

**Root**

每个配置都必须有一个根记录器Root。如果未配置，则将使用**默认根LoggerConfig**，其级别为ERROR且附加了Console appender。根记录器和其他记录器之间的主要区别是：1.根记录器没有name属性。2.根记录器不支持additivity属性，因为它没有父级。

- level：日志输出级别，共有8个级别，按照从低到高为：All < Trace < Debug < Info < Warn < Error < Fatal < OFF
- AppenderRef：Root的子节点，用来指定该日志输出到哪个Appender.

**Logger**

Logger节点用来单独指定日志的形式，比如要为指定包下的class指定不同的日志级别等。

使用Logger元素必须有一个**name属性**，root logger不用name元属性
每个Logger可以使用TRACE，DEBUG，INFO，WARN，ERROR，ALL或OFF之一配置级别。如果未指定级别，则默认为ERROR。可以为additivity属性分配值true或false。如果省略该属性，则将使用默认值true。

Logger还可以**配置一个或多个AppenderRef属**性。引用的每个appender将与指定的Logger关联。如果在Logger上配置了多个appender，则在处理日志记录事件时会调用每个appender。

- name：用来指定该Logger所适用的类或者类所在的包全路径，继承自Root节点。一般是项目包名或者框架的包名，比如：com.jourwon，org.springframework

- level：日志输出级别，共有8个级别，按照从低到高为：All < Trace < Debug < Info < Warn < Error < Fatal < OFF
- AppenderRef：Logger的子节点，用来指定该日志输出到哪个Appender，如果没有指定，就会默认继承自Root。如果指定了，那么会在指定的这个Appender和Root的Appender中都会输出，此时我们可以设置Logger的additivity="false"只在自定义的Appender中进行输出。



##### 日志重复打印问题

如果Root中的日志包含了Logger中的日志信息，并且AppenderRef是一样的配置，则日志会打印两次。

这是log4j2继承机制问题，在Log4j2中，logger是**有继承关系**的，root是根节点，在log4j2中，有个additivity的属性，它是子Logger 是否继承 父Logger 的 **输出源（appender） 的属性**。具体说，默认情况下子Logger会继承父Logger的appender，也就是说子Logger会在父Logger的appender里输出。若是additivity设为false，则子Logger只会在自己的appender里输出，而不会在父Logger的appender里输出。

要打破这种传递性，也非常简单，在logger中添加 additivity = “false”，如下所示：

```Java
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
    <Appenders>
        <Console name="Console">
            <PatternLayout>
                <Pattern>%d{yyyy-MM-dd HH:mm:ss,SSS} [%t] %-5level %logger{0} - %msg%n</Pattern>
            </PatternLayout>
        </Console>
    </Appenders>
    <Loggers>
        <!-- name属性为项目包名或者类名 -->
        <Logger name="com.jourwon" level="debug" additivity="false">
            <AppenderRef ref="Console"/>
        </Logger>
        <Root level="error">
            <AppenderRef ref="Console"/>
        </Root>
    </Loggers>
</Configuration>
```

##### 使用Lombok工具简化创建Logger类

lombok就是一个注解工具jar包，能帮助我们省略一繁杂的代码。
使用Lombok后，@Slf4j注解生成了log日志常量，无需去声明一个log就可以在类中使用log记录日志。

```Java
@Slf4j
public class Log4jTest {

    public static void main(String[] args) {
        log.error("Something else is wrong here");
    }

}
```



#### slf4j与log4j、log4j2对比

##### log4j

如果在我们系统中单独使用log4j的话，我们只需要引入log4j的核心包就可以了，我这里用的是：log4j-1.2.17.jar，然后在系统中使用如下代码输出日志：

```
public class Log4jTest {
	private static final org.apache.log4j.Logger logger = org.apache.log4j.Logger.getLogger(Log4jTest.class);
	
	public static void main(String[] args) {
		logger.info("hello word");
	}
}
```

在系统的src目录下添加依赖的配置文件：

```
log4j.rootLogger=INFO, stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n
```

##### log4j2

​    如果在我们系统中单独使用log4j2的话，我们只需要引入log4j2的核心包就可以了，我这里用的是：log4j-api-2.7.jar和log4j-core-2.7.jar，然后在系统中使用如下代码输出日志：

   在系统的src目录下添加依赖的配置文件：

```
public class Log4j2Test {
	private static org.apache.logging.log4j.Logger logger = org.apache.logging.log4j.LogManager.getLogger(Log4jTest.class);
	public static void main(String[] args) {
		logger.info("hello word");
	}
}
```

   在系统的src目录下添加依赖的配置文件：

```
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="info">
	<Appenders>
		<Console name="consoleAppender" target="SYSTEM_OUT">
			<PatternLayout pattern="%d{DATE} %5p %c{1}:%L - %m%n" />
		</Console>
 
 
	</Appenders>
	<Loggers>
		<Root level="info">
			<AppenderRef ref="consoleAppender" />
		</Root>
	</Loggers>
</Configuration>
```

#####  log4j与log4j2的区别：

1.获取Logger的api不一样，log4j的api为org.apache.log4j.Logger，而log4j2的api为org.apache.logging.log4j.Logger
2.配置方式不一样，**log4j2对properties的配置支持不是很好**，它的格式一般为xml格式或者yaml格式，这种格式的可读性比较好，各种配置一目了然
3.Log4j2.0基于LMAX Disruptor的**异步日志在多线程环境下性能会远远优于**Log4j 1.x和logback（官方数据是10倍以上）



##### slf4j

它仅仅是一个为Java程序提供日志输出的统一接口，并不是一个具体的日志实现方案，就比如JDBC一样，只是一种规则而已，所以单独的slf4j是不能工作的，必须搭配其他具体的日志实现方案，比如log4j或者log4j2，它的优势和原理可以参考：https://blog.csdn.net/winwill2012/article/details/71786004，要在系统中使用slf4j，我们需要引入的核心包为：slf4j-api-1.6.4.jar

##### slf4j+log4j

如果我们在系统中需要使用slf4j和log4j来进行日志输出的话，我们需要引入下面的jar包：
log4j核心jar包：log4j-1.2.17.jar
slf4j核心jar包：slf4j-api-1.6.4.jar
slf4j与log4j的桥接包：slf4j-log4j12-1.6.1.jar，这个包的作用就是使用slf4j的api，但是底层实现是基于log4j
所以我们获取日志可以通过下面的代码进行：

```
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
 
public class Slf4jTest {
	private static final Logger logger = LoggerFactory.getLogger(Slf4jTest2.class);
 
	public static void main(String[] args) {
		logger.info("hello world");
	}
}
```

##### slf4j+log4j2

如果我们在系统中需要使用slf4j和log4j2来进行日志输出的话，我们需要引入下面的jar包：
log4j2核心jar包：log4j-api-2.7.jar和log4j-core-2.7.jar
slf4j核心jar包：slf4j-api-1.6.4.jar
slf4j与log4j2的桥接包：log4j-slf4j-impl-2.7.jar，这个包的作用就是使用slf4j的api，但是底层实现是基于log4j2
所以我们获取日志仍可以通过上面的代码进行：

##### slf4j+log4j不修改代码升级到log4j2

如果我们系统中刚开始用的是slf4j和log4j，然后出于性能考虑，要升级到slf4j和log4j2，并且不需要改动任何代码的话(因为我们系统可能是一个大工程，然后基本上每个类都会有日志输出，改动代码可能牵一发而动全身)，出于这个考虑，我们可以这样来进行修改：
1、删除项目中存在的Log4j1.x所必须的log4j和slf4j-log4j12等依赖，例如从我们上面做的去升级的话，需要删除log4j-1.2.17.jar和slf4j-log4j12-1.6.1.jar
2、添加log4j2和slf4j桥接包：log4j-slf4j-impl-2.7.jar替换log4j和slf4j桥接包：slf4j-log4j12-1.6.1.jar
3、如果我们在系统中使用了log4j的api去获取Logger的话：

```
org.apache.log4j.Logger logger = org.apache.log4j.Logger.getLogger(Log4jTest.class)
```

我们需要添加log4j-1.2-api-2.7.jar去替换log4j-1.2.17.jar
4、将log4j的properties文件修改为log4j2的xml文件，然后，同样在系统中使用slf4j的方式获取日志：

```
org.slf4j.Logger logger = org.slf4j.LoggerFactory.getLogger(Self4jTest.class);
```



