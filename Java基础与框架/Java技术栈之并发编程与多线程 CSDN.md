### Java技术栈之并发编程 CSDN

#### 并发编程理论

Java并发编程是整个Java开发体系中最难以理解，但也是最重要的知识点之一，因此学习起来比较费劲，从而导致很多人望而却步，但是无论是职场面试还是高并发高流量的系统的实现都离不开并发编程，能够真正掌握并发编程的人才在市场上供不应求。

##### 1.并发编程优点

###### 充分利用多核CPU的计算能力

摩尔定律：当价格不变时，集成电路上可容纳的元器件的数目，约每隔18-24个月便会增加一倍，性能也将提升一倍。换言之，每一美元所能买到的电脑性能，将每隔18-24个月翻一倍以上。这一定律揭示了信息技术进步的速度。

一直以来，硬件的发展极其迅速，也有一个很著名的"摩尔定律"，你可能会奇怪明明讨论的是并发编程为什么会扯到了硬件的发展，这其中的关系应该是多核CPU的发展为并发编程提供的硬件基础。摩尔定律并不是一种自然法则或者是物理定律，它只是基于观测数据，对未来的一种预测。按照所预测的速度，我们的计算能力会按照指数级别的速度增长，不久以后会拥有超强的计算能力，正是在畅想未来的时候，2004年，Intel宣布4GHz芯片的计划推迟到2005年，然后在2004年秋季，Intel宣布彻底取消4GHz的计划，也就是说摩尔定律的有效性超过了半个世纪戛然而止。但是，聪明的硬件工程师并没有停止研发的脚步，他们为了进一步提升计算速度，不是再追求单独的计算单元，而是将多个计算单元整合到了一起，也就是形成了多核CPU。短短十几年的时间，家用型CPU，比如Intel i7就可以达到4核心甚至8核心。而专业服务器则通常可以达到几个独立的CPU，每一个CPU甚至拥有多达8个以上的内核。因此，摩尔定律似乎在CPU核心扩展上继续得到体验。因此，多核的CPU的背景下，催生了并发编程的趋势，通过并发编程的形式可以将多核CPU的计算能力发挥到极致，性能得到提升。顶级计算机科学家Donald Ervin Knuth如此评价这种情况：在我看来，这种现象（并发）或多或少是由于硬件设计者无计可施导致的，他们将摩尔定律的责任推给了软件开发者。

###### 方便进行业务拆分，提升系统并发能力和性能

在特殊的业务场景下先天的就适合于并发编程。现在的系统动不动就要求百万级甚至千万级的并发量，而多线程并发编程正是开发高并发系统的基础，利用好多线程机制可以大大提高系统整体的并发能力以及性能。比如在图像处理领域，一张1024X768像素的图片，包含78万6千多个像素。将所有的像素遍历一边都需要很长的时间，面对如此复杂的计算量就需要充分利用多核CPU的计算能力。又比如当我们在网上购物时，为了提升响应速度，减库存、生成订单等等这些操作就可以进行拆分，利用多线程的技术完成。面对复杂业务模型，并行程序会比串行程序更适应业务需求，而并发编程更能吻合这种业务拆分 。

##### **2.并发编程的缺点**

并发编程的目的就是为了能提高程序的执行效率，提高程序运行速度，但是并发编程并不总是能提高程序运行速度的，而且并发编程可能会遇到很多问题，比如：内存泄漏、上下文切换、线程安全、死锁等问题。

###### 频繁的上下文切换

​	**任务从保存到再加载就是一次上下文切换**。

时间片是CPU分配给各个线程的时间，因为时间非常短，所以CPU不断通过切换线程，让我们觉得多个线程是同时执行的，时间片一般是几十毫秒。而每次切换时，需要保存当前的状态，以便能够进行恢复先前的状态，而这个切换时非常损耗性能，过于频繁反而无法发挥出多线程编程的优势。

​	**减少上下文切换的解决方案**

- 无锁并发编程：可以参照concurrentHashMap锁分段的思想，不同的线程处理不同段的数据，这样在多线程竞争的条件下，可以减少上下文切换的时间。
- CAS算法：利用Atomic下使用CAS算法来更新数据，使用了乐观锁，可以有效的减少一部分不必要的锁竞争带来的上下文切换。
- 使用最少线程：避免创建不需要的线程，比如任务很少，但是创建了很多的线程，这样会造成大量的线程都处于等待状态。
- 协程：在单线程里实现多任务的调度，并在单线程里维持多个任务间的切换。

由于上下文切换也是个相对比较耗时的操作，所以在"java并发编程的艺术"一书中有过一个实验，并发累加未必会比串行累加速度要快。 可以使用Lmbench3测量上下文切换的时长 vmstat测量上下文切换次数

###### 线程安全

多线程编程中最难以把握的就是临界区线程安全问题，稍微不注意就会出现死锁的情况，一旦产生死锁就会造成系统功能不可用。

```Java
public class DeadLockDemo {
    private static String resource_a = "A";
    private static String resource_b = "B";

    public static void main(String[] args) {
        deadLock();
    }

    public static void deadLock() {
        Thread threadA = new Thread(new Runnable() {
            @Override
            public void run() {
                synchronized (resource_a) {
                    System.out.println("get resource a");
                    try {
                        Thread.sleep(3000);
                        synchronized (resource_b) {
                            System.out.println("get resource b");
                        }
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        });
        Thread threadB = new Thread(new Runnable() {
            @Override
            public void run() {
                synchronized (resource_b) {
                    System.out.println("get resource b");
                    synchronized (resource_a) {
                        System.out.println("get resource a");
                    }
                }
            }
        });
        threadA.start();
        threadB.start();

    }
}
```

在上面的这个demo中，开启了两个线程threadA， threadB，其中threadA占用了resource_a， 并等待被threadB占用的resource _b。threadB占用了resource _b正在等待被threadA占用的resource _a。因此threadA，threadB出现线程安全的问题，形成死锁。同样可以通过jps，jstack证明这种推论：

```Java
"Thread-1":
  waiting to lock monitor 0x000000000b695360 (object 0x00000007d5ff53a8, a java.lang.String),
  which is held by "Thread-0"
"Thread-0":
  waiting to lock monitor 0x000000000b697c10 (object 0x00000007d5ff53d8, a java.lang.String),
  which is held by "Thread-1"

Java stack information for the threads listed above:
===================================================
"Thread-1":
        at learn.DeadLockDemo$2.run(DeadLockDemo.java:34)
        - waiting to lock <0x00000007d5ff53a8(a java.lang.String)
        - locked <0x00000007d5ff53d8(a java.lang.String)
        at java.lang.Thread.run(Thread.java:722)
"Thread-0":
        at learn.DeadLockDemo$1.run(DeadLockDemo.java:20)
        - waiting to lock <0x00000007d5ff53d8(a java.lang.String)
        - locked <0x00000007d5ff53a8(a java.lang.String)
        at java.lang.Thread.run(Thread.java:722)

Found 1 deadlock.
```

如上所述，完全可以看出当前死锁的情况。

那么，通常可以用如下方式避免死锁的情况：

- 避免一个线程同时获得多个锁；
- 避免一个线程在锁内部占有多个资源，尽量保证每个锁只占用一个资源；
- 尝试使用定时锁，使用lock.tryLock(timeOut)，当超时等待时当前线程不会阻塞；
- 对于数据库锁，加锁和解锁必须在一个数据库连接里，否则会出现解锁失败的情况。

所以，如何正确的使用多线程编程技术有很大的学问，比如如何保证线程安全，如何正确理解由于JVM内存模型在原子性，有序性，可见性带来的问题，比如数据脏读，DCL等问题。而在学习多线程编程技术的过程中也会让你收获颇丰。

##### 3.重要概念

###### 阻塞与非阻塞

阻塞与非阻塞的重点在于进/线程等待消息时候的行为，也就是在等待消息的时候，当前进/线程是挂起状态，还是非挂起状态。

- 阻塞：调用在发出去后，在消息返回之前，当前进/线程会被挂起，直到有消息返回，当前进/线程才会被激活；

- 非阻塞：调用在发出去后，不会阻塞当前进/线程，而会立即返回。

    

###### 同步与异步

同步：当一个同步调用发出去后，调用者要一直等待调用结果的返回后，才能进行后续的操作。

异步：当一个异步调用发出去后，调用者不用管被调用方法是否完成，都会继续执行后面的代码。 异步调用，要想获得结果，一般有两种方式：

- 主动轮询异步调用的结果；
- 被调用方通过callback来通知调用方调用结果；

比如，在超市购物，如果一件物品没了，你得等仓库人员跟你调货，直到仓库人员跟你把货物送过来，你才能继续去收银台付款，这就类似同步调用。而异步调用了，就像网购，你在网上付款下单后，什么事就不用管了，该干嘛就干嘛去了，当货物到达后你收到通知去取就好。



###### 临界区

临界区用来表示一种**公共资源或者说是共享数据**，可以被多个线程使用。但是每个线程使用时，一旦临界区资源被一个线程占有，那么其他线程必须等待。



###### 并发与并行

- 并发： 同一时间段，多个任务交替执行 (单位时间内不一定同时执行)；

- 并行：单位时间内，多个任务同时执行。真正意义上的“同时进行”，真正的并行只能出现在拥有多个CPU的系统中；

- 串行：线程串行的情况下，有n个任务或者你可以理解n个方法，由一个线程顺序执行。由于任务、方法都在一个线程执行所以不存在线程不安全情况，也就不存在临界区的问题。

    

###### 上下文切换

多线程编程中一般线程的个数都大于 CPU 核心的个数，而一个 CPU 核心在任意时刻只能被一个线程使用，为了让这些线程都能得到有效执行，CPU 采取的策略是为每个线程分配时间片并轮转的形式。当一个线程的时间片用完的时候就会重新处于就绪状态让给其他线程使用，这个过程就属于一次上下文切换。

概括来说就是：当前任务在执行完 CPU 时间片切换到另一个任务之前会先保存自己的状态，以便下次再切换回这个任务时，可以再加载这个任务的状态。**任务从保存到再加载的过程就是一次上下文切换。**

上下文切换通常是计算密集型的。也就是说，它需要相当可观的处理器时间，在每秒几十上百次的切换中，每次切换都需要**纳秒量级的时间**。所以，上下文切换对系统来说意味着消耗大量的 CPU 时间，事实上，可能是操作系统中时间消耗最大的操作。Linux 相比与其他操作系统（包括其他类 Unix 系统）有很多的优点，其中有一项就是，其上下文切换和模式切换的时间消耗非常少。



#### 进程和线程

一个 Java 程序的运行是 main 线程和多个其他线程同时运行。

##### 1.进程与线程的区别总结

线程具有许多传统进程所具有的特征，故又称为轻型进程(Light—Weight Process)或进程元；而把传统的进程称为重型进程(Heavy—Weight Process)，它相当于只有一个线程的任务。在引入了线程的操作系统中，通常一个进程都有若干个线程，至少包含一个线程。

- 根本区别：进程是操作系统**资源分配的基本单位**，而线程是处理器任务**调度和执行的基本单位**

- 资源开销：每个进程都有**独立的代码和数据空间**（程序上下文），程序之间的切换会有较大的开销；线程可以看做轻量级的进程，同一类线程共享代码和数据空间，每个线程都有自己**独立的运行栈和程序计数器（PC）**，线程之间切换的开销小。

- 包含关系：如果一个进程内有多个线程，则执行过程不是一条线的，而是多条线（线程）共同完成的；线程是进程的一部分，所以线程也被称为**轻权进程或者轻量级进程**。

- 内存分配：同一进程的线程**共享本进程的地址空间和资源**，而进程之间的地址空间和资源**是相互独立的**

- 影响关系：一个进程崩溃后，在保护模式下**不会对其他进程产生影响**，但是一个线程崩溃整个进程都死掉。所以**多进程要比多线程健壮**。

- 执行过程：每个独立的进程有程序运行的入口、顺序执行序列和程序出口。但是线程**不能独立执行**，必须依存在应用程序中，由应用程序提供多个线程执行控制，两者均可并发执行



##### 2. JVM 角度--进程和线程之间的关系

一个进程中可以有多个线程，多个线程共享进程的堆和方法区 (JDK1.8 之后的元空间)资源，但是每个线程有自己的**程序计数器、虚拟机栈 和 本地方法栈**。

###### 1.程序计数器为什么是私有的?

程序计数器主要有下面两个作用：

字节码解释器通过改变程序计数器来依次读取指令，从而实现代码的流程控制，如：顺序执行、选择、循环、异常处理。
在多线程的情况下，程序计数器用于记录当前线程执行的位置，从而当线程被切换回来的时候能够知道该线程上次运行到哪儿了。
需要注意的是，如果执行的是 native 方法，那么程序计数器记录的是 undefined 地址，只有执行的是 Java 代码时程序计数器记录的才是下一条指令的地址。

所以，程序计数器私有主要是为了线程切换后能恢复到正确的执行位置。

###### 2.虚拟机栈和本地方法栈为什么是私有的?  

- **虚拟机栈**：每个 Java 方法在执行的同时会创建一个栈帧用于**存储局部变量表、操作数栈、常量池引用**等信息。从方法调用直至执行完成的过程，就对应着一个栈帧在 Java 虚拟机栈中入栈和出栈的过程。
- **本地方法栈**：和虚拟机栈所发挥的作用非常相似，区别是： 虚拟机栈为**虚拟机执行 Java 方法** （也就是字节码）服务，而本地方法栈则为**虚拟机使用到的 Native 方法服务**。 在 HotSpot 虚拟机中和 Java 虚拟机栈合二为一。
    所以，为了保证线程中的局部变量不被别的线程访问到，虚拟机栈和本地方法栈是线程私有的。



###### 3.堆和方法区

堆和方法区是所有线程共享的资源，其中堆是进程中最大的一块内存，主要用于存放新创建的对象 (所有对象都在这里分配内存)，方法区主要用于存放已被加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。


###### 4.多进程和多线程区别

多进程：操作系统中同时运行的多个程序

多线程：在同一个进程中同时运行的多个任务

举个例子，多线程下载软件，可以同时运行多个线程，但是通过程序运行的结果发现，每一次结果都不一致。 因为多线程存在一个特性：**随机性**。造成的原因：CPU在瞬间不断切换去处理各个线程而导致的，可以理解成多个线程在抢CPU资源。

​		**多线程提高CPU使用率**

![image-20200310164711675](/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310164711675.png)

多线程并不能提高运行速度，但可以提高运行效率，让CPU的使用率更高。但是如果多线程有安全问题或出现频繁的上下文切换时，运算速度可能反而更低。

###### 5.Java中的多线程

Java程序的进程里有几个线程：主线程，垃圾回收线程(后台线程)等

在 Java 中，当我们启动 main 函数时其实就是启动了一个 JVM 的进程，而 main 函数所在的线程就是这个进程中的一个线程，也称主线程。

Java支持多线程，当Java程序执行main方法的时候，就是在执行一个名字叫做main的线程，可以在main方法执行时，开启多个线程A,B,C，多个线程 main,A,B,C同时执行，相互抢夺CPU，Thread类是java.lang包下的一个常用类,每一个Thread类的对象，就代表一个处于某种状态的线程



#### 线程概念

##### 1.创建线程的四种方式

创建线程的四种方式

1. 继承Thread类（缺点：单继承）

    - 定义一个Thread类的子类，重写run方法，将相关逻辑实现，run()方法就是线程要执行的业务逻辑方法

    - 创建自定义的线程子类对象

    - 调用子类实例的star()方法来启动线程

        ```Java
        public class MyThread extends Thread {
        
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName() + " run()方法正在执行...");
            }
        }
        public class TheadTest {
        
            public static void main(String[] args) {
                MyThread myThread = new MyThread(); 	
                myThread.start();
                System.out.println(Thread.currentThread().getName() + " main()方法执行结束");
            }
        }
        
        运行结果：
        main main()方法执行结束
        Thread-0 run()方法正在执行...
        ```

2. 实现Runnable接口（用的最多）

    - 定义Runnable接口实现类MyRunnable，并重写run()方法
    - 创建MyRunnable实例myRunnable，以myRunnable作为target创建Thead对象，**该Thread对象才是真正的线程对象**
    - 调用线程对象的start()方法

    ```Java
    public class MyRunnable implements Runnable {
    
        @Override
        public void run() {
            System.out.println(Thread.currentThread().getName() + " run()方法执行中...");
        }
    }
    public class RunnableTest {
    
        public static void main(String[] args) {
            MyRunnable myRunnable = new MyRunnable();
            Thread thread = new Thread(myRunnable);
            thread.start();
            System.out.println(Thread.currentThread().getName() + " main()方法执行完成");
        }
    }
    
    执行结果：
    main main()方法执行完成
    Thread-0 run()方法执行中...
    ```

3. 使用Callable和Future创建线程

    - 创建实现Callable接口的类myCallable
    - 以myCallable为参数创建FutureTask对象
    - 将FutureTask作为参数创建Thread对象
    - 调用线程对象的start()方法

    ```Java
    public class MyCallable implements Callable<Integer> {
    
        @Override
        public Integer call() {
            System.out.println(Thread.currentThread().getName() + " call()方法执行中...");
            return 1;
        }
    }
    public class CallableTest {
    
        public static void main(String[] args) {
            FutureTask<Integer> futureTask = new FutureTask<Integer>(new MyCallable());
            Thread thread = new Thread(futureTask);
            thread.start();
    
            try {
                Thread.sleep(1000);
                System.out.println("返回结果 " + futureTask.get());
            } catch (InterruptedException e) {
                e.printStackTrace();
            } catch (ExecutionException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + " main()方法执行完成");
        }
    }
    
    执行结果
    Thread-0 call()方法执行中...
    返回结果 1
    main main()方法执行完成
    ```

4. 使用Executor框架创建线程池

    Executors提供了一系列工厂方法用于创先线程池，返回的线程池都实现了ExecutorService接口。

    主要有newFixedThreadPool，newCachedThreadPool，newSingleThreadExecutor，newScheduledThreadPool，后续详细介绍这四种线程池

    ```Java
    public class MyRunnable implements Runnable {
    
        @Override
        public void run() {
            System.out.println(Thread.currentThread().getName() + " run()方法执行中...");
        }
    }
    public class SingleThreadExecutorTest {
    
        public static void main(String[] args) {
            ExecutorService executorService = Executors.newSingleThreadExecutor();
            MyRunnable runnableTest = new MyRunnable();
            for (int i = 0; i < 5; i++) {
                executorService.execute(runnableTest);
            }
            System.out.println("线程任务开始执行");
            executorService.shutdown();
        }
    }
    
    执行结果：
    线程任务开始执行
    pool-1-thread-1 is running...
    pool-1-thread-1 is running...
    pool-1-thread-1 is running...
    pool-1-thread-1 is running...
    pool-1-thread-1 is running...
    ```

##### 2.线程的状态和生命周期

Java 线程在运行的生命周期中的指定时刻只可能处于下面 6 种不同状态的其中一个状态

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310175648348.png" alt="image-20200310175648348" style="zoom:50%;" />

线程在生命周期中并不是固定处于某一个状态而是随着代码的执行在不同状态之间切换。

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310175725928.png" alt="image-20200310175725928" style="zoom:50%;" />

​		由上图可以看出：线程创建之后它将处于 **NEW（新建）** 状态，调用 `start()` 方法后开始运行，线程这时候处于 **READY（可运行）** 状态。可运行状态的线程获得了 CPU 时间片（timeslice）后就处于 **RUNNING（运行）** 状态。（操作系统隐藏 Java 虚拟机（JVM）中的 RUNNABLE 和 RUNNING 状态，它只能看到 RUNNABLE 状态，所以 Java 系统一般将这两个状态统称为 **RUNNABLE（运行中）** 状态 ）

​		当线程执行 **wait()**方法之后，线程进入 **WAITING（等待）**状态。进入等待状态的线程需要依靠其他线程的通知才能够返回到运行状态，而 TIME_WAITING(超时等待) 状态相当于在等待状态的基础上增加了超时限制，比如通过 sleep（long millis）方法或 wait（long millis）方法可以将 Java 线程置于 TIMED WAITING 状态。当超时时间到达后 Java 线程将会返回到 RUNNABLE 状态。当线程调用同步方法时，在没有获取到锁的情况下，线程将会进入到 BLOCKED（阻塞） 状态。线程在执行 Runnable 的run()方法之后将会进入到 TERMINATED（终止） 状态。

##### 3.线程状态的基本操作

除了新建一个线程外，线程在生命周期内还有需要进行一些基本操作，而这些操作会成为线程间一种通信方式，比如使用中断（interrupted）方式通知实现线程间的交互等等，下面就将具体说说这些操作。

###### interrupted

中断可以理解为线程的一个标志位，它表示了一个运行中的线程是否被其他线程进行了中断操作。中断好比其他线程对该线程打了一个招呼。其他线程可以调用该线程的interrupt()方法对其进行中断操作，同时该线程可以调用
isInterrupted()来感知其他线程对其自身的中断操作，从而做出响应。另外，同样可以调用Thread的静态方法
interrupted()对当前线程进行中断操作，该方法会清除中断标志位。**需要注意的是，当抛出InterruptedException时候，会清除中断标志位，也就是说在调用isInterrupted会返回false**。
<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310175903926.png" alt="image-20200310175903926" style="zoom:50%;" />

```Java
public class InterruptDemo {
    public static void main(String[] args) throws InterruptedException {
        //sleepThread睡眠1000ms
        final Thread sleepThread = new Thread() {
            @Override
            public void run() {
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                super.run();
            }
        };
        //busyThread一直执行死循环
        Thread busyThread = new Thread() {
            @Override
            public void run() {
                while (true) ;
            }
        };
        sleepThread.start();
        busyThread.start();
        sleepThread.interrupt();
        busyThread.interrupt();
        while (sleepThread.isInterrupted()) ;
        System.out.println("sleepThread isInterrupted: " + sleepThread.isInterrupted());
        System.out.println("busyThread isInterrupted: " + busyThread.isInterrupted());
    }
}

输出结果：
sleepThread isInterrupted: false
busyThread isInterrupted: true
java.lang.InterruptedException: sleep interrupted
	at java.lang.Thread.sleep(Native Method)
	at com.jourwon.test.InterruptDemo$1.run(InterruptDemo.java:17)
```

开启了两个线程分别为sleepThread和BusyThread， sleepThread睡眠1s，BusyThread执行死循环。然后分别对着两个线程进行中断操作，可以看出sleepThread抛出InterruptedException后清除标志位，而busyThread就不会清除标志位。

另外，同样可以通过中断的方式实现线程间的简单交互， while (sleepThread.isInterrupted()) 表示在Main线程中会持续监测sleepThread线程，一旦sleepThread的中断标志位清零，即**sleepThread.isInterrupted()**返回为false时才会继续Main线程才会继续往下执行。因此，中断操作可以看做线程间一种简便的交互方式。一般在结束线程时通过中断标志位或者标志位的方式可以有机会去清理资源，相对于武断而直接的结束线程，这种方式要优雅和安全。



###### join

join方法可以看做是线程间**协作**的一种方式，很多时候，一个线程的输入可能**非常依赖于另一个线程的输出**，这就像两个好基友，一个基友先走在前面突然看见另一个基友落在后面了，这个时候他就会在原处等一等这个基友，等基友赶上来后，就两人携手并进。其实线程间的这种**协作方式**也符合现实生活。在软件开发的过程中，从客户那里获取需求后，需要经过需求分析师进行需求分解后，这个时候产品，开发才会继续跟进。如果一个线程实例A执行了**threadB.join()**，其含义是：当前**线程A会等待threadB线程终止后threadA才会继续执行**。关于join方法一共提供如下这些方法:
<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310180135709.png" alt="image-20200310180135709" style="zoom:50%;" />

Thread类除了提供join()方法外，另外还提供了超时等待的方法，如果线程threadB在等待的时间内还没有结束的话，threadA会在超时之后继续执行。join方法源码关键是： 

```
while (isAlive()) {
    wait(0);
 }
```

可以看出来当前等待对象threadA会一直阻塞，直到被等待对象threadB结束后即isAlive()返回false的时候才会结束while循环，当threadB退出时会调用notifyAll()方法通知所有的等待线程。下面用一个具体的例子来说说join方法的使用：

```Java
public class JoinDemo {
    public static void main(String[] args) {
        Thread previousThread = Thread.currentThread();
        for (int i = 1; i <= 10; i++) {
            Thread curThread = new JoinThread(previousThread);
            curThread.start();
            previousThread = curThread;
        }
    }
    static class JoinThread extends Thread {
        private Thread thread;

        public JoinThread(Thread thread) {
            this.thread = thread;
        }

        @Override
        public void run() {
            try {
                thread.join();
                System.out.println(thread.getName() + " terminated.");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
输出结果：
main terminated.
Thread-0 terminated.
Thread-1 terminated.
Thread-2 terminated.
Thread-3 terminated.
Thread-4 terminated.
Thread-5 terminated.
Thread-6 terminated.
Thread-7 terminated.
Thread-8 terminated.
```

在上面的例子中一个创建了10个线程，每个线程都会等待前一个线程结束才会继续运行。可以通俗的理解成接力，前一个线程将接力棒传给下一个线程，然后又传给下一个线程…



###### sleep

public static native void sleep(long millis)方法显然是Thread的静态方法，很显然它是让当前线程按照指定的时间休眠，其休眠时间的精度取决于处理器的计时器和调度器。需要注意的是如果当前线程获得了锁，sleep方法并不会失去锁。sleep方法经常拿来与Object.wait()方法进行比价，这也是面试经常被问的地方。

​	**sleep() VS wait()**
​	两者主要的区别：

- sleep()方法是Thread的静态方法，而wait是Object实例方法
- wait()方法必须要在同步方法或者同步块中调用，也就是必须已经获得对象锁。而sleep()方法没有这个限制可以在任何地方使用。另外，wait()方法会释放占有的对象锁，使得该线程进入等待池中，等待下一次获取资源。而sleep()方法只是会让出CPU并不会释放掉对象锁；
- sleep()方法在休眠时间达到后，如果再次获得CPU时间片就会继续执行，而wait()方法必须等待Object.notift/Object.notifyAll通知后，才会离开等待池，并且再次获得CPU时间片才会继续执行。

###### yield

public static native void yield()这是一个**静态方法**，一旦执行，它会是**当前线程让出CPU**，但是，需要注意的是，让出的CPU并不是代表当前线程不再运行了，如果在下一次竞争中，又获得了CPU时间片当前线程依然会继续运行。另外，让出的时间片只会**分配给当前线程相同优先级的线程**。什么是线程优先级？下面就来具体聊一聊。

现代操作系统基本采用**时分**的形式调度运行的线程，操作系统会分出一个个时间片，线程会分配到若干时间片，当前时间片用完后就会发生线程调度，并等待下次分配。线程分配到的时间多少也就决定了线程使用处理器资源的多少，而线程优先级就是**决定线程需要或多或少分配一些处理器资源的线程属性**。

在Java程序中，通过一个整型成员变量**Priority**来控制优先级，优先级的范围从1~10.在构建线程的时候可以通过**setPriority(int)**方法进行设置，默认优先级为5，优先级高的线程相较于优先级低的线程优先获得处理器时间片。需要注意的是在不同JVM以及操作系统上，线程规划存在差异，有些操作系统甚至会忽略线程优先级的设定。

另外需要注意的是，**sleep()和yield()方法，同样都是当前线程会交出处理器资源**，而它们不同的是，**sleep()交出来的时间片其他线程都可以去竞争**，也就是说都有机会获得当前线程让出的时间片。**而yield()方法只允许与当前线程具有相同优先级的线程能够获得释放出来的CPU时间片**。


##### 4.进程的优先级

理论上来说系统会根据优先级来决定首先使哪个线程进入运行状态。当 CPU 比较闲的时候，设置线程优先级几乎不会有任何作用，而且很多操作系统**压根不会理会你设置的线程优先级**，所以不要让业务过度依赖于线程的优先级。

另外，线程优先级具有继承特性比如 **A 线程启动 B 线程，则 B 线程的优先级和 A 是一样的**。线程优先级还具有**随机性**也就是说线程优先级高的不一定每一次都先执行完。

Thread 类中包含的成员变量代表了线程的某些优先级。如Thread.MIN_PRIORITY（常数 1），Thread.NORM_PRIORITY（常数 5）,Thread.MAX_PRIORITY（常数 10）。其中每个线程的优先级都在1到10 之间，1的优先级为最低，10的优先级为最高，在默认情况下优先级都是Thread.NORM_PRIORITY（常数 5）。

一般情况下，不会对线程设定优先级别，**更不会让某些业务严重地依赖线程的优先级别**，比如权重，借助优先级设定某个任务的权重，这种方式是不可取的，一般定义线程的时候使用默认的优先级就好了。设定优先级：

```Java
public final void setPriority(int newPriority) {
    ThreadGroup g;
    checkAccess();
    //线程游戏优先级不能小于 1 也不能大于 10，否则会抛出异常
    if (newPriority > MAX_PRIORITY || newPriority < MIN_PRIORITY) {
        throw new IllegalArgumentException();
    }
    //如果指定的线程优先级大于该线程所在线程组的最大优先级，那么该线程的优先级将设为线程组的最大优先级
    if((g = getThreadGroup()) != null) {
        if (newPriority > g.getMaxPriority()) {
            newPriority = g.getMaxPriority();
        }
        setPriority0(priority = newPriority);
    }
}
```

##### 5.守护线程和用户线程

守护线程和用户线程简介:

**用户 (User) 线程：**运行在**前台**，执行具体的任务，如程序的主线程、连接网络的子线程等都是用户线程
**守护 (Daemon) 线程：**运行在**后台**，为其他前台线程服务。也可以说守护线程是 JVM 中非守护线程的 “佣人”。一旦所有用户线程都结束运行，守护线程会随 JVM 一起结束工作
main 函数所在的线程就是一个用户线程啊，main 函数启动的同时在 JVM 内部同时还启动了好多守护线程，比如垃圾回收线程。

那么守护线程和用户线程有什么区别呢？

比较明显的区别之一是用户线程结束，JVM 退出，不管这个时候有没有守护线程运行。而守护线程不会影响 JVM 的退出。

注意事项：

setDaemon(true)必须在start()方法前执行，否则会抛出 IllegalThreadStateException 异常
在守护线程中产生的新线程也是守护线程
不是所有的任务都可以分配给守护线程来执行，比如读写操作或者计算逻辑
守护 (Daemon) 线程中不能依靠 finally 块的内容来确保执行关闭或清理资源的逻辑。因为我们上面也说过了一旦所有用户线程都结束运行，守护线程会随 JVM 一起结束工作，所以守护 (Daemon) 线程中的 finally 语句块可能无法被执行。
————————————————
版权声明：本文为CSDN博主「ThinkWon」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/ThinkWon/article/details/102027115