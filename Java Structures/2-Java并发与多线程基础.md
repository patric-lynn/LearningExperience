### Java并发与多线程

#### 1.并发编程基础

​		Java并发编程是整个Java开发体系中最难以理解，但也是最重要的知识点之一，因此学习起来比较费劲，从而导致很多人望而却步，但是无论是职场面试还是**高并发高流量的系统**的实现都离不开并发编程，能够真正掌握并发编程的人才在市场上供不应求。

##### 1.并发编程优点

###### 1.充分利用多核CPU的计算能力

​		**摩尔定律**：当价格不变时，集成电路上可容纳的元器件的数目，约每隔18-24个月便会增加一倍，性能也将提升一倍。换言之，每一美元所能买到的电脑性能，将每隔18-24个月翻一倍以上。这一定律揭示了信息技术进步的速度。

​		因此，多核的CPU的背景下，催生了并发编程的趋势，通过**并发编程的形式可以将多核CPU的计算能力发挥到极致，性能得到提升**。顶级计算机科学家Donald Ervin Knuth如此评价这种情况：在我看来，这种现象（并发）或多或少是由于硬件设计者无计可施导致的，他们将摩尔定律的责任推给了软件开发者。

###### 2.方便进行业务拆分，提升系统并发能力和性能

​		在特殊的业务场景下先天的就适合于并发编程。现在的系统动不动就要求**百万级甚至千万级的并发量**，而**多线程并发编程正是开发高并发系统的基础**，利用好多线程机制可以大大提高系统整体的并发能力以及性能。比如在图像处理领域，一张1024X768像素的图片，包含78万6千多个像素。将所有的像素遍历一边都需要很长的时间，面对如此复杂的计算量就需要充分利用多核CPU的计算能力。又比如当我们在网上购物时，**为了提升响应速度，减库存、生成订单等等这些操作就可以进行拆分**，利用多线程的技术完成。面对复杂业务模型，并行程序会比串行程序更适应业务需求，而并发编程更能吻合这种业务拆分 。



##### **2.并发编程的缺点**

​		并发编程的目的就是为了能**提高程序的执行效率**，提高程序运行速度，但是并发编程并不总是能提高程序运行速度的，而且并发编程可能会遇到很多问题，比如：内存泄漏、上下文切换、线程安全、死锁等问题。

###### 1.频繁的上下文切换

​		**任务从保存到再加载就是一次上下文切换**。

​		时间片是CPU分配给各个线程的时间，因为时间非常短，所以CPU不断通过**切换线程**，让我们觉得多个线程是同时执行的，时间片一般是几十毫秒。而每次切换时，需要保存当前的状态，以便能够进行恢复先前的状态，而这个切换时非常损耗性能，过于频繁反而无法发挥出多线程编程的优势。

​		**减少上下文切换的解决方案**

- **无锁并发编程**：可以参照concurrentHashMap**锁分段的思想**，不同的线程处理不同段的数据，这样在多线程竞争的条件下，可以减少上下文切换的时间。
- **CAS算法**：使用CAS算法来更新数据，**使用了乐观锁**，可以有效的减少一部分**不必要的锁竞争**带来的上下文切换。
- **使用最少线程**：避免创建**不需要的线程**，比如任务很少，但是创建了很多的线程，这样会造成大量的线程都处于等待状态。
- **协程**：在单线程里实现**多任务的调度**，并在单线程里**维持多个任务间的切换**。

​       由于上下文切换也是个**相对比较耗时的操作**，所以在"java并发编程的艺术"一书中有过一个实验，并发累加未必会比串行累加速度要快。 可以使用Lmbench3测量**上下文切换的时长，** vmstat测量**上下文切换次数。**

###### 2.线程安全问题

​		多线程编程中最难以把握的就是**临界区线程安全问题**，稍微不注意就会出现**死锁的情况**，一旦产生死锁就会造成系统功能不可用。

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

​		在上面的这个demo中，开启了**两个线程threadA， threadB**，其中threadA占用了resource_a， 并等待被threadB占用的resource _b。threadB占用了resource _b**正在等待被threadA占用的resource _a**。因此threadA，threadB出现线程安全的问题，形成死锁。同样可以通过jps，jstack证明这种推论：

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

​	    如上所述，完全可以看出当前死锁的情况。

​		那么，通常可以用如下方式避免死锁的情况：

- 避免一个线程**同时获得多个锁**；
- 避免一个线程在锁内部**占有多个资源**，尽量保证**每个锁只占用一个资源**；
- 尝试使用定时锁，使用**lock.tryLock(timeOut)**，当超时等待时当前线程不会阻塞；
- 对于数据库锁，加锁和解锁**必须在一个数据库连接里**，否则会出现解锁失败的情况。

​        所以，如何正确的使用多线程编程技术有很大的学问，比如如何保证线程安全，如何正确理解由于JVM内存模型在**原子性，有序性，可见性**带来的问题，比如**数据脏读，DCL**等问题。而在学习多线程编程技术的过程中也会让你收获颇丰。



##### 3.重要概念

###### 1.阻塞与非阻塞

​		阻塞与非阻塞的重点在于**进/线程等待消息时候的行为**，也就是在等待消息的时候，当前进/线程是**挂起状态**，还是**非挂起状态**。

- **阻塞**：调用在发出去后，在消息返回之前，**当前进/线程会被挂起，直到有消息返回**，当前进/线程才会被激活；

- **非阻塞**：调用在发出去后，不会阻塞当前进/线程，**而会立即返回**。

###### 2.同步与异步

-   **同步**：当一个同步调用发出去后，调用者要**一直等待调用结果的返回后**，才能进行后续的操作。

-   **异步**：当一个异步调用发出去后，调用者不用管被调用方法是否完成，**都会继续执行后面的代码**。 异步调用，要想获得结果，一般有两种方式：
-   **主动轮询**异步调用的结果；
  
- 被调用方通过**callback来通知调用方调用结果**；


​        比如，在超市购物，如果一件物品没了，你得等仓库人员跟你调货，直到仓库人员跟你把货物送过来，你才能继续去收银台付款，这就类似**同步调用**。而异步调用了，就像网购，你在网上付款下单后，什么事就不用管了，该干嘛就干嘛去了，当货物到达后你收到通知去取就好。

###### 3.临界区

​		临界区用来表示一种**公共资源或者说是共享数据**，可以被多个线程使用。但是每个线程使用时，一旦临界区资源被一个线程占有，那么**其他线程必须等待**。

###### 4.并发与并行

- **并发**： 同一时间段，多个任务**交替执行** (单位时间内**不一定同时执行**)；

- **并行**：单位时间内，多个任务同时执行。真正意义上的“**同时进行**”，真正的并行只能出现在**拥有多个CPU的系统中**；

- **串行**：线程串行的情况下，有n个任务或者你可以理解n个方法，由一个线程顺序执行。由于任务、方法都在一个线程执行所以**不存在线程不安全情况，也就不存在临界区的问题**。

###### 5.上下文切换

​		多线程编程中一般**线程的个数都大于 CPU 核心的个数**，而一个 CPU 核心在任意时刻只能被一个线程使用，为了让这些线程都能得到有效执行，CPU 采取的策略是为**每个线程分配时间片并轮转的形式**。当一个线程的时间片用完的时候就会**重新处于就绪状态让给其他线程使用**，这个过程就属于一次上下文切换。

​		概括来说就是：当前任务在执行完 CPU 时间片切换到另一个任务之前会先保存自己的状态，以便下次再切换回这个任务时，可以再加载这个任务的状态。**任务从保存到再加载的过程就是一次上下文切换。**

​		**上下文切换通常是计算密集型的**。也就是说，它需要相当可观的处理器时间，在每秒几十上百次的切换中，每次切换都需要**纳秒量级的时间**。所以，上下文切换对系统来说**意味着消耗大量的 CPU 时间**，事实上，可能是操作系统中时间消耗最大的操作。Linux 相比与其他操作系统（包括其他类 Unix 系统）有很多的优点，其中有一项就是，其上下文切换和模式切换的时间消耗非常少。





#### 2.Java并发编程理论

##### 1.Java内存模型

###### 1.理解Java内存模型（JMM）

​		**什么是线程安全？**在<<深入理解Java虚拟机>>中看到的定义。原文如下：

​		当多个线程访问同一个对象时，如果不用考虑这些线程在运行时环境下的**调度和交替运行**，也不需要进行额外的同步，或者在调用方进行**任何其他的协调操作**，调用这个对象的行为都**可以获取正确的结果**，那这个对象是**线程安全的**。

​		关于定义的理解是一个仁者见仁智者见智的事情。出现线程安全的问题一般是因为**主内存和工作内存数据不一致性和重排序**导致的，而解决线程安全的问题最重要的就是理解这两种问题是怎么来的，那么，理解它们的核心在于理解**Java内存模型（JMM）**。

​		在多线程条件下，多个线程肯定会相互协作完成一件事情，一般来说就会涉及到多个线程间相互通信告知彼此的状态以及当前的执行结果等，另外，为了性能优化，还会涉及到**编译器指令重排序**和**处理器指令重排序**。下面会一一来聊聊这些知识。



###### 2.Java内存模型：抽象结构

​		线程间协作通信可以类比人与人之间的协作的方式，在现实生活中，之前网上有个流行语“你妈喊你回家吃饭了”，就以这个生活场景为例，小明在外面玩耍，小明妈妈在家里做饭，做完饭后准备叫小明回家吃饭，那么就存在两种方式：

​		小明妈妈要去上班了十分紧急这个时候手机又没有电了，于是就在桌子上贴了一张纸条“饭做好了，放在…”小明回家后看到纸条如愿吃到妈妈做的饭菜，那么，如果将小明妈妈和小明作为两个线程，那么这张纸条就是这两个线程间通信的共享变量，通过**读写共享变量**实现两个线程间协作；

​		还有一种方式就是，妈妈的手机还有电，妈妈在赶去坐公交的路上给小明打了个电话，这种方式就是**通知机制**来完成协作。同样，可以引申到**线程间通信机制**。

​		通过上面这个例子，应该有些认识。在并发编程中主要需要解决两个问题：

​		**1. 线程之间如何通信；**（**共享内存和消息传递**）

​		**2.线程之间如何完成同步（这里的线程指的是并发执行的活动实体）**。

​		通信是指线程之间以何种机制来交换信息，主要有两种：**共享内存和消息传递**。这里，可以分别类比上面的两个举例。

​		**Java内存模型是共享内存的并发模型，线程之间主要通过读-写共享变量来完成隐式通信。**如果程序员不能理解Java的共享内存模型在编写并发程序时一定会遇到各种各样关于内存可见性的问题。

​		**哪些是共享变量**

​		在Java程序中所有**实例域，静态域和数组元素**都是放在**堆内存中**（所有线程均可访问到，是可以共享的），而局部变量，方法定义参数和异常处理器参数**不会在线程间共享**。共享数据会出现**线程安全的问题**，而非共享数据不会出现线程安全的问题。关于JVM运行时内存区域在后面的文章会讲到。

​		**JMM抽象结构模型**

​		我们知道CPU的处理速度和主存的读写速度不是一个量级的（CPU的处理速度快很多），为了平衡这种巨大的差距，每个CPU都会有缓存。因此，**共享变量会先放在主存中**，每个线程都有属于自己的工作内存，并且会把位于主存中的共享变量拷贝到自己的工作内存，之后的读写操作**均使用位于工作内存的变量副本**，并在某个时刻将工作内存的变量副本写回到主存中去。JMM就**从抽象层次定义了这种方式**，并且JMM决定了**一个线程对共享变量的写入何时对其他线程是可见的**。

​		从横向去看看，线程A和线程B就好像通过共享变量在进行**隐式通信**。这其中有个意思的问题，如果线程A更新后数据并没有及时写回到主存，而此时**线程B读到的是过期的数据**，这就出现了“**脏读**”现象。可以**通过同步机制（控制不同线程间操作发生的相对顺序）来解决或者通过volatile关键字**使得每次volatile变量都能够强制**刷新到主存，从而对每个线程都是可见的**。



###### 3.内存模型：主内存与工作内存

​		处理器上的寄存器的读写的速度比内存快几个数量级，为了解决这种速度矛盾，在它们之间加入了**高速缓存**。加入高速缓存带来了一个新的问题：**缓存一致性**。如果多个缓存共享同一块主内存区域，那么多个缓存的数据可能会不一致，需要一些协议来解决这个问题。

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310211610094.png" alt="image-20200310211610094" style="zoom:30%;" />

​		所有的变量都存储在主内存中，每个线程还有自己的工作内存，工作内存**存储在高速缓存或者寄存器中**，保存了该线程使用的**变量的主内存副本拷贝**。线程只能直接操作工作内存中的变量，不同线程之间的变量值传递需要通过主内存来完成。



###### 4.内存模型：交互操作

​		Java 内存模型定义了 **8 个操作**来完成主内存和工作内存的交互操作。

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310211718901.png" alt="image-20200310211718901" style="zoom:40%;" />

- **lock（锁定）**：作用于主内存中的变量，它把一个变量标识为**一个线程独占的状态**；
- **unlock（解锁）**：作用于主内存中的变量，它把一个处于锁定状态的**变量释放出来**，释放后的变量才可以被其他线程锁定
- **read（读取）**：作用于主内存的变量，它把一个变量的值从主内存**传输到线程的工作内存中**，以便后面的load动作使用；
- **load（载入）**：作用于工作内存中的变量，它把read操作**从主内存中得到的变量值放入工作内存中**的变量副本
- **use（使用）**：作用于工作内存中的变量，它把工作内存中一个变量的**值传递给执行引擎**，每当虚拟机遇到一个需要使用到变量的值的字节码指令时将会执行这个操作；
- **assign（赋值）**：作用于工作内存中的变量，它把一个从执行引擎接收到的值**赋给工作内存的变量**，每当虚拟机遇到一个给变量赋值的字节码指令时执行这个操作；
- **store（存储）**：作用于工作内存的变量，它把工作内存中一个变量的**值传送给主内存**中以便随后的write操作使用；
- **write（操作）**：作用于主内存的变量，它把store操作从工作内存中得到的变量的**值放入主内存的变量中**。



###### 5.Java内存模型三大特性

​		**原子性**

​		Java 内存模型保证了 read、load、use、assign、store、write、lock 和 unlock **操作具有原子性**，例如对一个 int 类型的变量执行 assign 赋值操作，**这个操作就是原子性的**。但是 Java 内存模型允许虚拟机将没有被 volatile 修饰的 **64 位数据（long，double）的读写操作划分为两次 32 位的操作来进行**，即 load、store、read 和 write 操作可以不具备原子性。

​		有一个错误认识就是，int 等原子性的类型在多线程环境中**不会出现线程安全问题**。前面的线程不安全示例代码中，cnt 属于 int 类型变量，1000 个线程对它进行自增操作之后，得到的值为 997 而不是 1000。

​		**如果让volatile保证原子性，必须符合以下两条规则：**

-   **运算结果并不依赖于变量的当前值，或者能够确保只有一个线程修改变量的值；**

-   **变量不需要与其他的状态变量共同参与不变约束**

    

    **可见性**

​        可见性指当一个线程修改了共享变量的值，其它线程能够**立即得知这个修改**。Java 内存模型是通过在变量修改后**将新值同步回主内存**，在变量读取前**从主内存刷新变量值**来实现可见性的。

​		主要有**三种**实现可见性的方式：

- **volatile**，通过在指令中添加lock指令，以实现内存可见性。
- **synchronized**，当线程获取锁时会从主内存中获取共享变量的最新值，释放锁的时候会将共享变量同步到主内存中。
- **final**，被 final 关键字修饰的字段在构造器中一旦初始化完成，并且没有发生 this 逃逸（其它线程通过 this 引用访问到初始化了一半的对象），那么其它线程就能看见 final 字段的值。

​        对前面的线程不安全示例中的 cnt 变量使用 volatile 修饰，**不能解决线程不安全问题**，因为 volatile 并**不能保证操作的原子性**。

​	

​		**有序性**

​		有序性是指：**在本线程内观察，所有操作都是有序的**。在一个线程观察另一个线程，**所有操作都是无序的**，无序是因为**发生了指令重排序**。在 Java 内存模型中，**允许编译器和处理器对指令进行重排序**，重排序过程不会影响到单线程程序的执行，**却会影响到多线程并发执行的正确性**。

- **volatile 关键字**通过添加**内存屏障的方式**来**禁止指令重排**，即重排序时**不能把后面的指令放到内存屏障之前**。

- **synchronized** 关键字**同样可以保证有序性**，它保证每个时刻**只有一个线程执行同步代码**，相当于是让线程顺序执行同步代码。

    
  
    **总结**

-   **synchronized：具有原子性，有序性和可见性**；
-   **volatile：具有有序性和可见性**；
-   **final：具有可见性**



​		**补充知识：内存屏障**

​		我们都知道，为了性能优化，JMM在不改变正确语义的前提下，会允许编译器和处理器对指令序列**进行重排序**，那如果想阻止重排序要怎么办了？答案是可以**添加内存屏障**。JMM内存屏障分为四类

![image-20200310212928858](/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310212928858.png)



###### 6.先行发生原则

​		上面提到了可以用 **volatile 和 synchronized 来保证有序性**。除此之外，JVM 还规定了**先行发生原则**，让一个操作无需控制就能先于另一个操作完成。

1. 单一线程原则：在一个线程内，在程序前面的操作**先行发生**于后面的操作。

2. 管程锁定规则：一个 unlock 操作**先行发生于**后面对同一个锁的 lock 操作。
3. volatile 变量规则：对一个 volatile 变量的**写操作先行发生于**后面对这个变量的读操作。
4. 线程启动规则：Thread 对象的 start() 方法调用**先行发生于此线程的每一个动作**。

5. 线程加入规则：Thread 对象的结束先行发生于 join() 方法返回。
6. 线程中断规则：对线程 interrupt() 方法的调用**先行发生于被中断线程的代码检测到中断事件的发生**，可以通过 interrupted() 方法检测到是否有中断发生。
7. 对象终结规则：一个对象的初始化完成（构造函数执行结束）先行发生于它的 finalize() 方法的开始。
8. 传递性： 如果操作 A 先行发生于操作 B，操作 B 先行发生于操作 C，那么操作 A 先行发生于操作 C。



##### 2.重排序与数据依赖

###### 1.为什么需要重排序

​		现在的CPU一般采用**流水线**来执行指令。一个指令的执行被分成：**取指、译码、访存、执行、写回**等若干个阶段。然后，多条指令可以同时存在于流水线中，同时被执行。

​		指令流水线并不是串行的，并不会因为一个耗时很长的指令在“执行”阶段呆很长时间，而导致后续的指令都卡在“执行”之前的阶段上。我们编写的程序都要经过优化后（**编译器和处理器会对我们的程序进行优化以提高运行效率**）才会被运行，优化分为很多种，其中有一种优化叫做**重排序**，**重排序需要遵守as-if-serial规则和happens-before规则**，不能说你想怎么排就怎么排，如果那样岂不是乱了套。**重排序的目的是为了性能**。



###### 2.重排序分类

​		一般重排序可以分为如下三种：

- **编译器优化的重排序**。编译器在不改变单线程程序语义的前提下，可以重新安排语句的执行顺序；
- **指令级并行的重排序**。现代处理器采用了指令级并行技术来将多条指令重叠执行。如果**不存在数据依赖性**，处理器可以**改变语句对应机器指令的执行顺序**；
- **内存系统的重排序**。由于处理器使用**缓存和读/写缓冲区**，这使得加载和存储操作看上去可能是在乱序执行的。



###### 3.重排序过程

​		一个好的内存模型实际上会放宽对处理器和编译器规则的束缚，也就是说软件技术和硬件技术都为同一个目标而进行奋斗：**在不改变程序执行结果的前提下，尽可能提高并行度**。Java内存模型（JMM）对底层尽量减少约束，使其能够发挥自身优势。因此，在执行程序时，为了提高性能，编译器和处理器常常会对指令进行重排序。



###### 4.重排序对多线程的影响

```Java
class ReorderExample {
    int a = 0;
    boolean flag = false;

    public void writer() {
        a = 1;                   //1
        flag = true;             //2
    }

    public void reader() {
        if (flag) {              //3
            int i =  a * a;      //4
            ……
        }
    }
}
```

​		flag为标志位，表示a有没有被写入，当A线程执行 writer 方法，B线程执行 reader 方法，线程B在执行4操作的时候，能否看到线程A对a的写入操作？	答案是：不一定！

​		由于操作1和操作2没有数据依赖关系，编译器和处理器**可以对这两个操作重排序**。

​		如果操作1和操作2做了重排序，程序执行时，**线程A首先写标记变量 flag，随后线程 B 读这个变量**。由于条件判断为真，线程 B 将读取变量a。此时，变量 a 还根本没有被线程 A 写入，**在这里多线程程序的语义被重排序破坏了**！



###### 5.数据依赖性

​		如果两个操作访问同一个变量，且这两个操作中**有一个为写操作**，此时这两个操作之间就存在数据依赖性。数据依赖分下列三种类型：
![image-20200310221219768](/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310221219768.png)

​		上面三种情况，只要重排序两个操作的执行顺序，程序的执行结果将会被改变。前面提到过，编译器和处理器可能**会对操作做重排序**。编译器和处理器在重排序时，会**遵守数据依赖性**，编译器和处理器不会改变存在数据依赖关系的两个操作的执行顺序。注意，这里所说的数据依赖性仅针对**单个处理器中执行的指令序列**和**单个线程中执行的操作**，不同处理器之间和**不同线程之间**的数据依赖性**不被**编译器和处理器考虑。如果两个操作访问同一个变量，且这两个操作中有一个为写操作，此时这两个操作之间就存在数据依赖性。所以有数据依赖性的语句**不能进行重排序**。



###### 6.单线程as-if-serial规则和多线程happens-before六大规则

​		**as-if-serial规则（单线程间）**

​		as-if-serial语义的意思指：不管怎么重排序（编译器和处理器为了提高并行度），（**单线程）程序的执行结果不能被改变**。

​		编译器，runtime 和处理器都**必须遵守as-if-serial语义**。为了遵守as-if-serial语义，编译器和处理器不会对存在数据依赖关系的操作做重排序，因为这种重排序会改变执行结果。但是，如果操作之间不存在数据依赖关系，这些操作**可能被编译器和处理器重排序**。为了具体说明，请看下面计算圆面积的代码示例：

```
double pi  = 3.14;    //A
double r   = 1.0;     //B
double area = pi * r * r; //C
```

​		A和C之间存在数据依赖关系，同时B和C之间也存在数据依赖关系。因此在最终执行的指令序列中，**C不能被重排序到A和B的前面**（C排到A和B的前面，程序的结果将会被改变）。**但A和B之间没有数据依赖关系，编译器和处理器可以重排序A和B之间的执行顺序**。as-if-serial语义把**单线程程序保护了起来**，遵守as-if-serial语义的编译器，runtime 和处理器共同让编写单线程程序的程序员产生了一个幻觉：单线程程序是按程序的顺序来执行的**。**as-if-serial语义使程序员无需担心重排序会干扰他们，也无需担心内存可见性问题。

​		

​		**happens-before六大规则（跨线程间）**

​		上面的内容讲述了重排序原则，一会是编译器重排序一会是处理器重排序，如果让程序员再去了解这些底层的实现以及具体规则，那么程序员的负担就太重了，严重影响了并发编程的效率。因此，JMM为程序员在上层提供了六条规则，这样我们就可以根据规则去推论跨线程的内存可见性问题，而**不用再去理解底层重排序的规则**。

​		**happens-before定义**

​		happens-before的概念最初由Leslie Lamport在其一篇影响深远的论文（《Time，Clocks and the Ordering of Events in a Distributed System》）中提出，有兴趣的可以google一下。JSR-133使用happens-before的概念来指定两个操作之间的执行顺序。由于这两个操作可以在一个线程之内，也可以是在不同线程之间。因此，JMM可以通过happens-before关系向程序员提供跨线程的内存可见性保证（**如果A线程的写操作a与B线程的读操作b之间存在happens-before关系，尽管a操作和b操作在不同的线程中执行，但JMM向程序员保证a操作将对b操作可见**）。具体的定义为：

-   1）如果一个操作happens-before另一个操作，那么**第一个操作的执行结果将对第二个操作可见**，而且第一个操作的执行顺序排在第二个操作之前。
-   2）两个操作之间存在happens-before关系，**并不意味着Java平台的具体实现必须要按照happens-before关系指定的顺序来执行**。如果重排序之后的执行结果，与按happens-before关系来执行的结果一致，那么这种重排序并不非法（也就是说，JMM允许这种重排序）。

​        上面的1）是JMM**对程序员的承诺**。从程序员的角度来说，可以这样理解happens-before关系：如果A happens-before B，那么Java内存模型将向程序员保证——**A操作的结果将对B可见**，且A的执行顺序排在B之前。注意，这只是Java内存模型向程序员做出的保证！

​		上面的2）是JMM**对编译器和处理器重排序的约束原则**。正如前面所言，JMM其实是在遵循一个基本原则：只要不改变程序的执行结果（指的是单线程程序和正确同步的多线程程序），**编译器和处理器怎么优化都行**。JMM这么做的原因是：程序员对于这两个操作是否真的被重排序并不关心，程序员关心的是程序执行时的语义不能被改变（即执行结果不能被改变）。因此，**happens-before关系本质上和as-if-serial语义是一回事**。

​		**具体规则**

​		具体的一共有六项规则：

- **程序顺序规则**：一个线程中的每个操作，happens-before于该线程中的任意后续操作。
- **监视器锁规则**：对一个锁的解锁，happens-before于随后对这个锁的加锁。
- **volatile变量规则**：对一个volatile域的写，happens-before于任意后续对这个volatile域的读。
- **传递性**：如果A happens-before B，且B happens-before C，那么A happens-before C。
- **start()规则**：如果线程A执行操作ThreadB.start()（启动线程B），那么A线程的ThreadB.start()操作happens-before于线程B中的任意操作。
- **join()规则**：如果线程A**执行操作ThreadB.join()并成功返回**，那么线程B中的任意操作happens-before于线程A从ThreadB.join()操作成功返回。
- **程序中断规则**：对线程interrupted()方法的调用**先行于被中断线程的代码检测到中断时间的发生**。
- **对象finalize规则**：一个对象的初始化完成（构造函数执行结束）先行于发生它的finalize()方法的开始。



##### 3.Java并发理论总结

###### 1.JMM的设计

​		JMM是语言级的内存模型，在我的理解中JMM处于中间层，包含了两个方面：（1）**内存模型**；（2）**重排序以及happens-before规则**。同时，为了禁止特定类型的重排序会对编译器和处理器指令序列加以控制。而上层会有基于JMM的关键字和J.U.C包下的一些具体类用来方便程序员能够迅速高效率的进行并发编程。站在JMM设计者的角度，在设计JMM时需要考虑两个关键因素:

​		**程序员对内存模型的使用**

​		程序员希望内存模型易于理解、易于编程。程序员希望基于一个强内存模型来编写代码。

​		**编译器和处理器对内存模型的实现**

​		编译器和处理器希望内存模型对它们的束缚越少越好，这样它们就可以做尽可能多的优化来提高性能。编译器和处理器希望实现一个弱内存模型。

###### 2.happens-before与JMM的关系

​		一个happens-before规则对应于**一个或多个编译器和处理器重排序规则**。对于Java程序员来说，happens-before规则简单易懂，它避免Java程序员为了理解JMM提供的内存可见性保证而去学习复杂的重排序规则以及这些规则的具体实现方法

###### 3.需要关注的问题

​		从内存抽象结构来说，可能出在数据“脏读”的现象，这就是**数据可见性**的问题，另外，重排序在多线程中不注意的话也容易存在一些问题，比如一个很经典的问题就是**DCL（双重检验锁）**，这就是需要禁止重排序，另外，在多线程下原子操作，例如i++不加以注意的**也容易出现线程安全的问题**。但总的来说，在多线程开发时需要从**原子性，有序性，可见性三个方面进行考虑**。J.U.C包下的并发工具类和并发容器也是需要花时间去掌握的，这些东西在以后得文章中多会一一进行讨论。





#### 3.Java线程与线程池

##### 1.进程和线程的区别

​		一个 Java 程序的运行是 main 线程和多个其他线程同时运行。

###### 1.进程与线程的区别总结

​		线程具有许多传统进程所具有的特征，故又称为**轻型进程(Light—Weight Process)**或**进程元**；而把传统的进程称为重型进程(Heavy—Weight Process)，它相当于只有一个线程的任务。在引入了线程的操作系统中，通常一个进程都有若干个线程，至少包含一个线程。

- **根本区别**：进程是操作系统**资源分配的基本单位**，而线程是处理器任务**调度和执行的基本单位**

- **资源开销**：每个进程都有**独立的代码和数据空间**（程序上下文），程序之间的切换会有较大的开销；线程可以看做轻量级的进程，同一类线程共享代码和数据空间，每个线程都有自己**独立的运行栈和程序计数器（PC）**，线程之间切换的开销小。

- **包含关系**：如果一个进程内有多个线程，则执行过程不是一条线的，而是多条线（线程）共同完成的；线程是进程的一部分，所以线程也被称为**轻权进程或者轻量级进程**。

- **内存分配**：同一进程的线程**共享本进程的地址空间和资源**，而进程之间的地址空间和资源**是相互独立的**

- **影响关系**：一个进程崩溃后，在保护模式下**不会对其他进程产生影响**，但是一个线程崩溃整个进程都死掉。所以**多进程要比多线程健壮**。

- **执行过程**：每个独立的进程有程序运行的入口、顺序执行序列和程序出口。但是线程**不能独立执行**，必须依存在应用程序中，由应用程序提供多个线程执行控制，两者均可并发执行



###### 2. JVM 中进程和线程之间的关系

​		一个进程中可以有多个线程，多个线程共享进程的堆和方法区 (JDK1.8 之后的元空间)资源，但是每个线程有自己的**程序计数器、虚拟机栈 和 本地方法栈**。

​		**1.程序计数器为什么是私有的?**

​		程序计数器主要有下面两个作用：

-   字节码解释器通过改变程序计数器来依次读取指令，从而实现**代码的流程控制**，如：顺序执行、选择、循环、异常处理。
-   在多线程的情况下，程序计数器用于**记录当前线程执行的位置**，从而当线程被切换回来的时候能够知道该线程上次运行到哪儿了。

​       需要注意的是，如果执行的是 **native 方法**，那么程序计数器记录的是 undefined 地址，只有执行的是 Java 代码时程序计数器记录的才是下一条指令的地址。所以，程序计数器私有主要是为了线程切换后能恢复到正确的执行位置。

​		**2.虚拟机栈和本地方法栈为什么是私有的?**  

- **虚拟机栈**：每个 Java 方法在执行的同时会创建一个栈帧用于**存储局部变量表、操作数栈、常量池引用**等信息。从方法调用直至执行完成的过程，就对应着一个栈帧在 Java 虚拟机栈中入栈和出栈的过程。

- **本地方法栈**：和虚拟机栈所发挥的作用非常相似，区别是： 虚拟机栈为**虚拟机执行 Java 方法** （也就是字节码）服务，而本地方法栈则为**虚拟机使用到的 Native 方法服务**。 在 HotSpot 虚拟机中和 Java 虚拟机栈合二为一。
  
- 所以，为了**保证线程中的局部变量不被别的线程访问到**，虚拟机栈和本地方法栈是线程私有的。
  
    **3.堆和方法区**

​       堆和方法区是**所有线程共享的资源**，其中堆是**进程中最大的一块内存**，主要用于**存放新创建的对象** (所有对象都在这里分配内存)，方法区主要用于存放**已被加载的类信息、常量、静态变量、即时编译器编译后的代码等数据**。

​		**4.多进程和多线程区别**

-   多进程：操作系统中同时运行的**多个程序**

-   多线程：在同一个进程中同时运行的**多个任务**


​       举个例子，多线程下载软件，可以同时运行多个线程，但是通过程序运行的结果发现，每一次结果都不一致。 因为多线程存在一个特性：**随机性**。造成的原因：CPU在瞬间**不断切换去处理各个线程而导致的**，可以理解成多个线程在抢CPU资源。

​		**多线程提高CPU使用率**

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310164711675.png" alt="image-20200310164711675" style="zoom:45%;" />

​		多线程并不能提高运行速度，但可以提高运行效率，**让CPU的使用率更高**。但是如果多线程有安全问题或出现频繁的上下文切换时，运算速度可能反而更低。

​		**5.Java中的多线程**

​		Java程序的进程里有几个线程：**主线程，垃圾回收线程**(后台线程)等

​		在 Java 中，当我们启动 main 函数时其实就是**启动了一个 JVM 的进程**，而 main 函数所在的线程就是这个进程中的一个线程，也称**主线程**。

​		Java支持多线程，当Java程序执行main方法的时候，就是在**执行一个名字叫做main的线程**，可以在main方法执行时，开启多个线程A,B,C，多个线程 main,A,B,C同时执行，相互抢夺CPU，Thread类是java.lang包下的一个常用类,**每一个Thread类的对象，就代表一个处于某种状态的线程**



##### 2.Java线程

###### 1.创建线程的四种方式

创建线程的四种方式

**1.继承Thread类（缺点：单继承）**

- 定义一个Thread类的子类，**重写run方法**，将相关逻辑实现，run()方法就是线程要执行的业务逻辑方法

- 创建自定义的线程子类对象

- 调用子类实例的start()方法来启动线程

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



**2.实现Runnable接口（用的最多）**

- 定义Runnable接口实现**类MyRunnable**，并**重写run()方法**
- 创建MyRunnable实例myRunnable，以myRunnable作为target创建Thread对象，**该Thread对象才是真正的线程对象**
- 调用线程对象的**start()方法**

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
        Thread thread = new Thread(myRunnable); //注意此处需要使用线程构造器初始化一个runnable
        thread.start();
        System.out.println(Thread.currentThread().getName() + " main()方法执行完成");
    }
}

执行结果：
main main()方法执行完成
Thread-0 run()方法执行中...
```



**3.使用Callable和Future创建线程**

- 创建**实现Callable接口的类**myCallable
- **以myCallable为参数创建FutureTask对象**
- **将FutureTask作为参数**创建Thread对象
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



**4.使用Executor框架创建线程池**

​		Executors提供了**一系列工厂方法用于创先线程池**，返回的线程池都实现了ExecutorService接口。

​		主要有newFixedThreadPool，newCachedThreadPool，newSingleThreadExecutor，newScheduledThreadPool，后续详细介绍这四种线程池

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



###### 2.线程的状态

​		Java 线程在运行的生命周期中的指定时刻只可能处于下面 6 种不同状态的其中一个状态

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310175648348.png" alt="image-20200310175648348" style="zoom:50%;" />

​		线程在生命周期中并不是固定处于某一个状态而是随着代码的执行在不同状态之间切换。

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310175725928.png" alt="image-20200310175725928" style="zoom:50%;" />

​		由上图可以看出：线程创建之后它将处于 **NEW（新建）** 状态，调用 `start()` 方法后开始运行，线程这时候处于 **READY（可运行）** 状态。可运行状态的线程获得了 CPU 时间片（timeslice）后就处于 **RUNNING（运行）** 状态。（操作系统隐藏 Java 虚拟机（JVM）中的 RUNNABLE 和 RUNNING 状态，它只能看到 RUNNABLE 状态，所以 Java 系统一般将这两个状态统称为 **RUNNABLE（运行中）** 状态 ）

​		当线程执行 **wait()**方法之后，线程进入 **WAITING（等待）**状态。进入等待状态的线程需要**依靠其他线程的通知**才能够返回到运行状态，而 **TIME_WAITING(超时等待)** 状态相当于**在等待状态的基础上增加了超时限制**，比如通过 **sleep（long millis）方法或 wait（long millis）**方法可以将 Java 线程置于 TIMED WAITING 状态。当超时时间到达后 Java 线程将**会返回到 RUNNABLE 状态**。**当线程调用同步方法时，在没有获取到锁的情况下，线程将会进入到 BLOCKED（阻塞） 状态。**线程在执行 Runnable 的run()方法之后**将会进入到 TERMINATED（终止） 状态**。



###### 3.线程状态的基本操作

​		除了新建一个线程外，线程在生命周期内还有需要进行一些基本操作，而这些操作会成为线程间一种通信方式，比如使用中断（interrupted）方式**通知实现线程间的交互**等等，下面就将具体说说这些操作。

**interrupted**

​		中断可以理解为**线程的一个标志位**，它表示了一个运行中的线程**是否被其他线程进行了中断操作**。中断好比其他线程对该线程打了一个招呼。其他线程**可以调用该线程的interrupt()方法对其进行中断操作**，同时该线程可以调用isInterrupted()来**感知其他线程对其自身的中断操作**，从而做出响应。另外，同样可以调用Thread的静态方法**interrupted() 对当前线程进行中断操作，该方法会清除中断标志位**。需要注意的是，当抛出InterruptedException时候，**会清除中断标志位**，也就是说在**调用isInterrupted会返回false**。

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

​		开启了两个线程分别为sleepThread和BusyThread， sleepThread睡眠1s，BusyThread执行死循环。然后分别对着两个线程进行中断操作，可以看出**sleepThread抛出InterruptedException后清除标志位**，而busyThread就不会清除标志位。

​		另外，同样可以通过中断的方式实现线程间的简单交互， while (sleepThread.isInterrupted()) 表示在Main线程中会持续监测sleepThread线程，一旦sleepThread的中断标志位清零，即**sleepThread.isInterrupted()**返回为false时才会继续Main线程才会继续往下执行。因此，**中断操作可以看做线程间一种简便的交互方式**。一般在结束线程时通过中断标志位或者标志位的方式可以有机会去清理资源，相对于武断而直接的结束线程，这种方式要优雅和安全。



**join**

​		join方法可以看做是线程间**协作**的一种方式，很多时候，一个线程的输入可能**非常依赖于另一个线程的输出**，这就像两个好基友，一个基友先走在前面突然看见另一个基友落在后面了，这个时候他就会在原处等一等这个基友，等基友赶上来后，就两人携手并进。其实线程间的这种**协作方式**也符合现实生活。在软件开发的过程中，从客户那里获取需求后，需要经过需求分析师进行需求分解后，这个时候产品，开发才会继续跟进。如果一个线程实例A执行了**threadB.join()**，其含义是：当前**线程A会等待threadB线程终止后threadA才会继续执行**。关于join方法一共提供如下这些方法:
<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310180135709.png" alt="image-20200310180135709" style="zoom:50%;" />

​		Thread类除了提供join()方法外，另外还提供了**超时等待**的方法，如果线程threadB在等待的时间内还没有结束的话，threadA会在超时之后继续执行。join方法源码关键是： 

```
while (isAlive()) {
    wait(0);
 }
```

​		可以看出来当前等待对象threadA会一直阻塞，直到被等待对象threadB结束后即isAlive()返回false的时候才会结束while循环，当threadB退出时会**调用notifyAll()方法通知所有的等待线程**。下面用一个具体的例子来说说join方法的使用：

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

​		在上面的例子中一个创建了10个线程，每个线程都会等待前一个线程结束才会继续运行。可以通俗的理解成接力，前一个线程将接力棒传给下一个线程，然后又传给下一个线程…



**sleep**

​		public static native void sleep(long millis)方法显然是Thread的**静态方法**，很显然它是让当前线程按照指定的时间休眠，其休眠时间的精度取决于处理器的计时器和调度器。需要注意的是如果当前线程获得了锁，sleep方法**并不会失去锁**。sleep方法经常拿来与**Object.wait()**方法进行比价，这也是面试经常被问的地方。

​	**sleep() VS wait()**

​	两者主要的区别：

- sleep()方法是**Thread的静态方法**，而**wait是Object实例方法**。
- wait()方法必须要在**同步方法**或者**同步块**中调用，也就是必须**已经获得对象锁**。而sleep()方法没有这个限制可以在任何地方使用。另外，wait()方法会**释放占有的对象锁**，使得该线程进入等待池中，等待下一次获取资源。而sleep()方法只是会让出CPU并不会释放掉对象锁；
- sleep()方法在休眠时间达到后，如果再次获得CPU时间片就会继续执行，而**wait()方法必须等待Object.notift/Object.notifyAll通知**后，才会离开等待池，并且再次获得CPU时间片才会继续执行。



**yield**

​		public static native void yield()这是一个**静态方法**，一旦执行，它会是**当前线程让出CPU**，但是，需要注意的是，让出的CPU并不是代表当前线程不再运行了，如果在下一次竞争中，又获得了CPU时间片当前线程依然会继续运行。另外，让出的时间片只会**分配给当前线程相同优先级的线程**。什么是线程优先级？下面就来具体聊一聊。

​		现代操作系统基本采用**时分**的形式调度运行的线程，操作系统会分出一个个时间片，线程会分配到若干时间片，当前时间片用完后就会发生线程调度，并等待下次分配。线程分配到的时间多少也就决定了线程使用处理器资源的多少，而线程优先级就是**决定线程需要或多或少分配一些处理器资源的线程属性**。

​		在Java程序中，通过一个整型成员变量**Priority**来控制优先级，优先级的范围从1~10.在构建线程的时候可以通过**setPriority(int)**方法进行设置，默认优先级为5，优先级高的线程相较于优先级低的线程优先获得处理器时间片。需要注意的是在不同JVM以及操作系统上，线程规划存在差异，有些操作系统甚至会忽略线程优先级的设定。

​		另外需要注意的是，**sleep()和yield()方法，同样都是当前线程会交出处理器资源**，而它们不同的是，**sleep()交出来的时间片其他线程都可以去竞争**，也就是说都有机会获得当前线程让出的时间片。**而yield()方法只允许与当前线程具有相同优先级的线程能够获得释放出来的CPU时间片**。



###### 4.进程的优先级

​		理论上来说系统会根据优先级来决定首先使哪个线程进入运行状态。当 CPU 比较闲的时候，设置线程优先级几乎不会有任何作用，而且很多操作系统**压根不会理会你设置的线程优先级**，所以不要让业务过度依赖于线程的优先级。

​		另外，线程优先级具有继承特性比如 **A 线程启动 B 线程，则 B 线程的优先级和 A 是一样的**。线程优先级还具有**随机性**也就是说线程优先级高的不一定每一次都先执行完。

​		Thread 类中包含的**成员变量**代表了线程的某些优先级。如Thread.MIN_PRIORITY（常数 1），Thread.NORM_PRIORITY（常数 5）,Thread.MAX_PRIORITY（常数 10）。其中每个线程的优先级都在1到10 之间，1的优先级为最低，10的优先级为最高，在默认情况下优先级都是Thread.NORM_PRIORITY（常数 5）。

​		一般情况下，不会对线程设定优先级别，**更不会让某些业务严重地依赖线程的优先级别**，比如权重，借助优先级设定某个任务的权重，这种方式是不可取的，一般定义线程的时候使用默认的优先级就好了。设定优先级：

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



###### 5.守护线程和用户线程

​		**守护线程和用户线程简介:**

- **用户 (User) 线程：**运行在**前台**，执行具体的任务，如程序的主线程、连接网络的子线程等都是用户线程
- **守护 (Daemon) 线程：**运行在**后台**，为其他前台线程服务。也可以说守护线程是 JVM 中非守护线程的 “佣人”。一旦所有用户线程都结束运行，守护线程会随 JVM 一起结束工作

main 函数所在的线程就是一个用户线程啊，main 函数启动的同时在 JVM 内部同时还启动了好多守护线程，比如垃圾回收线程。

​		**那么守护线程和用户线程有什么区别呢？**

​		比较明显的区别之一是用户线程结束，JVM 退出，不管这个时候有没有守护线程运行。而守护线程不会影响 JVM 的退出。

​		**注意事项**：

- setDaemon(true)必须在start()方法前执行，否则会抛出 IllegalThreadStateException 异常
- 在守护线程中产生的新线程也是守护线程
- 不是所有的任务都可以分配给守护线程来执行，比如读写操作或者计算逻辑
- 守护 (Daemon) 线程中**不能依靠 finally 块的内容来确保执行关闭或清理资源的逻辑**。因为我们上面也说过了一旦所有用户线程都结束运行，守护线程会随 JVM 一起结束工作，所以守护 (Daemon) 线程中的 finally 语句块**可能无法被执行**。

    **守护线程详解**

​        守护线程是一种特殊的线程，就和它的名字一样，它是系统的守护者，在后台默默地守护一些系统服务，比如垃圾回收线程，JIT线程就可以理解为守护线程。与之对应的就是用户线程，用户线程就可以认为是系统的工作线程，它会完成整个系统的业务操作。用户线程完全结束后就意味着整个系统的业务任务全部结束了，因此系统就没有对象需要守护的了，守护线程自然而然就会退。当一个Java应用，只有守护线程的时候，虚拟机就会自然退出。下面以一个简单的例子来表述Daemon线程的使用。

```Java
public class DaemonDemo {
    public static void main(String[] args) {
        Thread daemonThread = new Thread(new Runnable() {
            @Override
            public void run() {
                while (true) {
                    try {
                        System.out.println("i am alive");
                        Thread.sleep(500);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    } finally {
                        System.out.println("finally block");
                    }
                }
            }
        });
        daemonThread.setDaemon(true);
        daemonThread.start();
        //确保main线程结束前能给daemonThread能够分到时间片
        try {
            Thread.sleep(800);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

输出结果为：
i am alive
finally block
i am alive
```

​		上面的例子中daemodThread run()方法中是一个while死循环，会一直打印,但是当main线程结束后daemonThread就会退出所以不会出现死循环的情况。**main线程先睡眠800ms保证daemonThread能够拥有一次时间片的机会**，也就是说可以正常执行一次打印“i am alive”操作和一次finally块中"finally block"操作。紧接着main 线程结束后，daemonThread退出，**这个时候只打印了"i am alive"并没有打印finally块中的**。因此，这里需要注意的是**守护线程在退出的时候并不会执行finnaly块中的代码，所以守护 (Daemon) 线程中不能依靠 finally 块的内容来确保执行关闭或清理资源的逻辑**

​		线程可以通过setDaemon(true)的方法将线程设置为守护线程。并且需要注意的是设置守护线程要先于start()方法，否则会报

```
Exception in thread "main" java.lang.IllegalThreadStateException
```


​		但是该线程还是会执行，只不过会当做正常的用户线程执行。



###### 6.线程死锁

**认识线程死锁**

​		百度百科：死锁是指两个或两个以上的进程（线程）在执行过程中，由于竞争资源或者由于彼此通信而造成的一种阻塞的现象，若无外力作用，它们都将无法推进下去。此时称系统处于死锁状态或系统产生了死锁，这些永远在互相等待的进程（线程）称为死锁进程（线程）。

​		多个线程同时被阻塞，它们中的一个或者全部都在等待某个资源被释放。由于线程被无限期地阻塞，因此程序不可能正常终止。

​		如下图所示，线程 A 持有资源 2，线程 B 持有资源 1，他们同时都想申请对方的资源，所以这两个线程就会互相等待而进入死锁状态。

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200310185746133.png" alt="image-20200310185746133" style="zoom:50%;" />

​		下面通过一个例子来说明线程死锁，代码模拟了上图的死锁的情况 (代码来源于《并发编程之美》)：

```Java
public class DeadLockDemo {
    private static Object resource1 = new Object();//资源 1
    private static Object resource2 = new Object();//资源 2

    public static void main(String[] args) {
        new Thread(() -> {
            synchronized (resource1) {
                System.out.println(Thread.currentThread() + "get resource1");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread() + "waiting get resource2");
                synchronized (resource2) {
                    System.out.println(Thread.currentThread() + "get resource2");
                }
            }
        }, "线程 1").start();

        new Thread(() -> {
            synchronized (resource2) {
                System.out.println(Thread.currentThread() + "get resource2");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread() + "waiting get resource1");
                synchronized (resource1) {
                    System.out.println(Thread.currentThread() + "get resource1");
                }
            }
        }, "线程 2").start();
    }
}
运行结果：
Thread[线程 1,5,main]get resource1
Thread[线程 2,5,main]get resource2
Thread[线程 1,5,main]waiting get resource2
Thread[线程 2,5,main]waiting get resource1
```

​		线程 A 通过 synchronized (resource1) 获得 resource1 的监视器锁，然后通过Thread.sleep(1000)；让线程 A 休眠 1s 为的是让线程 B 得到CPU执行权，然后获取到 resource2 的监视器锁。线程 A 和线程 B 休眠结束了都开始企图请求获取对方的资源，然后这两个线程就会陷入互相等待的状态，这也就产生了死锁。上面的例子符合产生死锁的四个必要条件。

**形成死锁的四个必要条件：**

- 互斥条件：线程(进程)对于所分配到的资源具有排它性，即一个资源只能被一个线程(进程)占用，直到被该线程(进程)释放
- 请求与保持条件：一个线程(进程)因请求被占用资源而发生阻塞时，对已获得的资源保持不放。
- 不剥夺条件：线程(进程)已获得的资源在末使用完之前不能被其他线程强行剥夺，只有自己使用完毕后才释放资源。
- 循环等待条件：当发生死锁时，所等待的线程(进程)必定会形成一个环路（类似于死循环），造成永久阻塞

**如何避免线程死锁**

​		我们只要破坏产生死锁的四个条件中的其中一个就可以了。

**破坏互斥条件**

​		这个条件我们没有办法破坏，因为我们用锁本来就是想让他们互斥的（临界资源需要互斥访问）。

**破坏请求与保持条件**

​		一次性申请所有的资源。

**破坏不剥夺条件**

​		占用部分资源的线程进一步申请其他资源时，如果申请不到，可以主动释放它占有的资源。

**破坏循环等待条件**

​		靠按序申请资源来预防。按某一顺序申请资源，释放资源则反序释放。破坏循环等待条件。

​	我们对线程 2 的代码修改成下面这样就不会产生死锁了。

```Java
new Thread(() -> {
    synchronized (resource1) {
        System.out.println(Thread.currentThread() + "get resource1");
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(Thread.currentThread() + "waiting get resource2");
        synchronized (resource2) {
            System.out.println(Thread.currentThread() + "get resource2");
        }
    }
}, "线程 2").start();

运行结果：
Thread[线程 1,5,main]get resource1
Thread[线程 1,5,main]waiting get resource2
Thread[线程 1,5,main]get resource2
Thread[线程 2,5,main]get resource1
Thread[线程 2,5,main]waiting get resource2
Thread[线程 2,5,main]get resource2
```

​		线程 1 首先获得到 resource1 的监视器锁，这时候线程 2 就获取不到了。然后线程 1 再去获取 resource2 的监视器锁，可以获取到。然后线程 1 释放了对 resource1、resource2 的监视器锁的占用，线程 2 获取到就可以执行了。这样就**破坏了破坏循环等待条件**，因此避免了死锁。







##### 3.Java线程池

###### 1.ThreadPoolExecutor详解

**1.为什么要使用线程池**

​		在实际使用中，线程是很**占用系统资源**的，如果对线程管理不善很容易导致系统问题。因此，在大多数并发框架中都会使用线程池来管理线程，使用线程池管理线程主要有如下好处：

- **降低资源消耗**。通过复用已存在的线程和降低线程关闭的次数来尽可能降低系统性能损耗；
- **提升系统响应速度**。通过复用线程，省去创建线程的过程，因此整体上提升了系统的响应速度；
- **提高线程的可管理性**。线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，因此，需要使用线程池来管理线程。

**2.线程池的创建**

​		创建线程池主要是ThreadPoolExecutor类来完成，ThreadPoolExecutor的有许多重载的构造方法，通过参数最多的构造方法来理解创建线程池有哪些需要配置的参数。ThreadPoolExecutor的构造方法为：

```Java
ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory,
                              RejectedExecutionHandler handler)
```

​		下面对参数进行说明：

- **corePoolSize**：表示**核心线程池的大小**。当提交一个任务时，如果当前核心线程池的线程个数没有达到corePoolSize，即使当前核心线程池有空闲的线程，也会创建新的线程来执行所提交的任务。如果当前核心线程池的线程个数已经达到了corePoolSize，则不再重新创建线程。如果调用了prestartCoreThread()或者 prestartAllCoreThreads()，线程池创建的时候**所有的核心线程都会被创建并且启动**。
- **maximumPoolSize**：表示线程池**能创建线程的最大个数**。如果当阻塞队列已满时，并且当前线程池线程个数没有超过maximumPoolSize的话，就会创建新的线程来执行任务。
- **keepAliveTime**：空闲线程存活时间。如果当前线程池的线程个数已经超过了corePoolSize，并且线程空闲时间超过了keepAliveTime的话，就会**将这些空闲线程销毁**，这样可以尽可能**降低系统资源消耗**。
- **unit**：时间单位。为keepAliveTime指定时间单位。
- **workQueue**：阻塞队列。用于保存任务的**阻塞队列**，关于阻塞队列可以看这篇文章。可以使用ArrayBlockingQueue, LinkedBlockingQueue, SynchronousQueue, PriorityBlockingQueue。
- **threadFactory**：创建线程的**工厂类**。可以通过指定线程工厂为每个创建出来的线程设置更有意义的名字，如果出现并发问题，也方便查找问题原因。
- **handler**：饱和策略。当线程池的阻塞队列已满和指定的线程都已经开启，说明当前线程池已经处于饱和状态了，那么就需要采用一种策略来处理这种情况。采用的策略有这几种：
    - AbortPolicy： 直接拒绝所提交的任务，并抛出RejectedExecutionException异常；
    - CallerRunsPolicy：只用调用者所在的线程来执行任务；
    - DiscardPolicy：不处理直接丢弃掉任务；
    - DiscardOldestPolicy：丢弃掉阻塞队列中存放时间最久的任务，执行当前任务
      

**3.线程池执行逻辑**

​		通过ThreadPoolExecutor创建线程池后，提交任务后执行过程是怎样的，下面来通过源码来看一看。execute方法源码如下：

```Java
public void execute(Runnable command) {
    if (command == null)
        throw new NullPointerException();
    /*
     * Proceed in 3 steps:
     *
     * 1. If fewer than corePoolSize threads are running, try to
     * start a new thread with the given command as its first
     * task.  The call to addWorker atomically checks runState and
     * workerCount, and so prevents false alarms that would add
     * threads when it shouldn't, by returning false.
     *
     * 2. If a task can be successfully queued, then we still need
     * to double-check whether we should have added a thread
     * (because existing ones died since last checking) or that
     * the pool shut down since entry into this method. So we
     * recheck state and if necessary roll back the enqueuing if
     * stopped, or start a new thread if there are none.
     *
     * 3. If we cannot queue task, then we try to add a new
     * thread.  If it fails, we know we are shut down or saturated
     * and so reject the task.
     */
    int c = ctl.get();
	//如果线程池的线程个数少于corePoolSize则创建新线程执行当前任务
    if (workerCountOf(c) < corePoolSize) {
        if (addWorker(command, true))
            return;
        c = ctl.get();
    }
	//如果线程个数大于corePoolSize或者创建线程失败，则将任务存放在阻塞队列workQueue中
    if (isRunning(c) && workQueue.offer(command)) {
        int recheck = ctl.get();
        if (! isRunning(recheck) && remove(command))
            reject(command);
        else if (workerCountOf(recheck) == 0)
            addWorker(null, false);
    }
	//如果当前任务无法放进阻塞队列中，则创建新的线程来执行任务
    else if (!addWorker(command, false))
        reject(command);
}
```

​		ThreadPoolExecutor的execute方法执行逻辑请见注释。下图为ThreadPoolExecutor的execute方法的执行示意图：

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200314132937966.png" alt="image-20200314132937966" style="zoom:40%;" />

​	execute方法执行逻辑有这样几种情况：

- 如果当前运行的线程少于corePoolSize，则会**创建新的线程**来执行新的任务；
- 如果运行的线程个数等于或者大于corePoolSize，则会将提交的任务存放到**阻塞队列workQueue**中；
- 如果当前workQueue队列已满的话，则会**创建新的线程来执行任务**；
- 如果线程个数已经超过了maximumPoolSize，则会使用**饱和策略RejectedExecutionHandler**来进行处理。

​		需要注意的是，线程池的设计思想就是使用了核心线程池corePoolSize，阻塞队列workQueue和线程池线程最大个数maximumPoolSize，这样的缓存策略来处理任务，实际上这样的设计思想在需要框架中都会使用。

**4.线程池的关闭**

​		关闭线程池，可以通过shutdown和shutdownNow这两个方法。它们的原理都是遍历线程池中所有的线程，然后依次中断线程。shutdown和shutdownNow还是有不一样的地方：

- shutdownNow首先将线程池的状态设置为**STOP**，然后尝试停止所有的**正在执行和未执行任务**的线程，并返回等待执行任务的列表；
- shutdown只是将线程池的状态设置为**SHUTDOWN**状态，然后中断所有**没有正在执行任务**的线程

​		可以看出shutdown方法**会将正在执行的任务继续执行完**，而shutdownNow会**直接中断正在执行的任务**。调用了这两个方法的任意一个，**isShutdown方法都会返回true**，当所有的线程都关闭成功，才表示线程池成功关闭，这时调用isTerminated方法才会返回true。

**5.线程池的工作原理**

​		当一个并发任务提交给线程池，线程池分配线程去执行任务的过程如下图所示：

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200314133338847.png" alt="image-20200314133338847" style="zoom:40%;" />

​		从图可以看出，线程池执行所提交的任务过程主要有这样几个阶段：

- 先判断线程池中核心线程池所有的线程**是否都在执行任务**。如果不是，则新创建一个线程执行刚提交的任务，否则，核心线程池中所有的线程都在执行任务，则进入第2步；
- 判断当前阻塞队列是否已满，如果未满，则将提交的任务**放置在阻塞队列**中；否则，则进入第3步；
- 判断线程池中所有的线程是否都在执行任务，如果没有，则创建一个新的线程来执行任务，否则，则交给饱和策略进行处理

**6.线程池阻塞队列**

**作用**：用来存储等待执行的任务

**线程的公平访问队列**：指阻塞的线程可以按照阻塞的**先后顺序访问队列**，即先阻塞先访问线程。为了保证公平性，通常会降低吞吐量

**常见阻塞队列**

​		**ArrayBlockingQueue**：一个用数组实现的**有界阻塞队列**，按照**先入先出(FIFO)**的原则对元素进行排序。**不保证**线程公平访问队列，**使用较少**

​		**PriorityBlockingQueue**：支持**优先级的无界阻塞队列**，使用较少

​		**LinkedBlockingQueue**：一个用**链表实现的有界阻塞队列**，队列默认和最长长度为**Integer.MAX_VALUE**。

​		队列按照先入先出的原则对元素进行排序，**使用较多**

- 吞吐量通常要**高于** ArrayBlockingQueue
- Executors.**newFixedThreadPool()** 使用了这个队列

​		**SynchronousQueue**：**不储存元素(无容量)的阻塞队列**，每个put操作必须等待一个take操作，
否则不能继续添加元素。支持**公平访问队列**，常用于**生产者，消费者模型**，吞吐量较高，使用较多

- 每个插入操作必须等到另一个线程调用**移除操作**，否则插入操作一直处于**阻塞状态**
- 吞吐量通常要**高于** LinkedBlockingQueue
- Executors.**newCachedThreadPool**使用了这个队列

**7.线程池的饱和策略**

**定义**：当队列和线程池都满了，说明线程池处于饱和状态，必须采取一种策略处理新提交的任务。

**常见策略**

AbortPolicy：中断策略，直接抛出异常

CallerRunsPolicy：调用者运行策略，让调用者所在线程来运行策略

DiscardOldestPolicy：舍弃最旧任务策略，丢弃队列中最旧的任务，然后重试任务的提交执行( execute() )

DiscardPolicy：舍弃策略，不处理，直接丢弃

**8.如何合理配置线程池参数？**

要想合理的配置线程池，就必须首先分析任务特性，可以从以下几个角度来进行分析：

1. 任务的性质：CPU密集型任务，IO密集型任务和混合型任务。
2. 任务的优先级：高，中和低。
3. 任务的执行时间：长，中和短。
4. 任务的依赖性：是否依赖其他系统资源，如数据库连接。

​		任务性质不同的任务可以用不同规模的线程池分开处理。**CPU密集型任务配置尽可能少的线程数量**，如配置CPU个数+1的线程数的线程池。**IO密集型任务则由于需要等待IO操作**，线程并不是一直在执行任务，则配置尽可能多的线程，如配置两倍CPU个数+1。混合型的任务，如果可以拆分，则将其拆分成一个CPU密集型任务和一个IO密集型任务，只要这两个任务执行的时间相差不是太大，那么分解后执行的吞吐率要高于串行执行的吞吐率，如果这两个任务执行时间相差太大，则没必要进行分解。

​		我们可以通过`Runtime.getRuntime().availableProcessors()`方法获得当前设备的CPU个数。

​		优先级不同的任务可以使用优先级队列PriorityBlockingQueue来处理。它可以**让优先级高的任务先得到执行**，需要注意的是如果一直有优先级高的任务提交到队列里，那么优先级低的任务可能永远不能执行。执行时间不同的任务可以交给不同规模的线程池来处理，或者也可以使用优先级队列，让执行时间短的任务先执行。

​		依赖数据库连接池的任务，因为线程提交SQL后需要等待数据库返回结果，如果等待的时间越长CPU空闲时间就越长，那么线程数应该设置越大，这样才能更好的利用CPU。并且，阻塞队列**最好是使用有界队列**，如果采用无界队列的话，一旦任务积压在阻塞队列中的话就会**占用过多的内存资源**，甚至会使得系统崩溃。

​		当然具体合理线程池值大小，需要结合系统实际情况，在大量的尝试下比较才能得出，以上只是前人总结的规律。

**最佳线程数目** = （（线程等待时间+线程CPU时间）/线程CPU时间 ）* CPU数目

​		比如平均每个线程CPU运行时间为0.5s，而线程等待时间（非CPU运行时间，比如IO）为1.5s，CPU核心数为8，那么根据上面这个公式估算得到：((0.5+1.5)/0.5)*8=32。这个公式进一步转化为：

**最佳线程数目 = （线程等待时间与线程CPU时间之比 + 1）* CPU数目**

**可以得出一个结论**：

​		线程等待时间所占比例越高，需要越多线程。线程CPU时间所占比例越高，需要越少线程。

​		以上公式与之前的CPU和IO密集型任务设置线程数基本吻合。



###### **2.四种常见线程池线程池架构**

​		Java里面线程池的顶级接口是Executor，Executor并不是一个线程池，而只是一个执行线程的工具。真正的线程池接口是ExecutorService。

​		比较重要的几个类：

| 类/接口                     | 描述                                                         |
| --------------------------- | ------------------------------------------------------------ |
| ExecutorService             | 真正的线程池接口                                             |
| ScheduledExecutorService    | 能和Timer/TimerTask类似，解决那些需要任务重复执行的问题      |
| ThreadPoolExecutor          | ExecutorService的默认实现                                    |
| ScheduledThreadPoolExecutor | 继承ThreadPoolExecutor的ScheduledExecutorService接口实现，周期性任务调度的类实现 |

​		要配置一个线程池是比较复杂的，尤其是对于线程池的原理不是很清楚的情况下，很有可能配置的线程池不是较优的，因此在Executors类里面提供了一些静态工厂，生成一些常用的线程池。

​		Java通过**Executors工厂类提供四种线程池**，分别为：

1. **newCachedThreadPool** ：创建一个**可缓存线程池**，如果线程池长度超过处理需要，**可灵活回收空闲线程**，若无可回收，否则新建线程。（线程最大并发数**不可控制**）
2. **newFixedThreadPool**：创建一个**固定大小的线程池**，可控制线程**最大并发数**，超出的线程会在队列中等待。
3. **newScheduledThreadPool** ： 创建一个**定时线程池**，支持定时及周期性任务执行。
4. **newSingleThreadExecutor** ：创建一个**单线程化的线程池**，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行。

**我们先创建一个统一的线程任务，方便测试四种线程池**

```Java
public class MyRunnable implements Runnable {

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + " is running...");
    }

}
```

**newSingleThreadExecutor**

```Java
public class SingleThreadExecutorTest {

    public static void main(String[] args) {
        ExecutorService executorService = Executors.newSingleThreadExecutor();
        MyRunnable myRunnable = new MyRunnable();
        for (int i = 0; i < 5; i++) {
            executorService.execute(myRunnable);
        }

        System.out.println("线程任务开始执行");
        executorService.shutdown();
    }

}
```

输出结果

```Java
线程任务开始执行
pool-1-thread-1 is running...
pool-1-thread-1 is running...
pool-1-thread-1 is running...
pool-1-thread-1 is running...
pool-1-thread-1 is running...
```

底层实现

```Java
public static ExecutorService newSingleThreadExecutor() {
    return new FinalizableDelegatedExecutorService
        (new ThreadPoolExecutor(1, 1,
                                0L, TimeUnit.MILLISECONDS,
                                new LinkedBlockingQueue<Runnable>()));
}
```

​		从参数可以看出来，SingleThreadExecutor 相当于特殊的 FixedThreadPool，它的执行流程如下：

1. 线程池中没有线程时，**新建一个线程执行任务**
2. 有一个线程以后，**将任务加入阻塞队列**，不停的加
3. 唯一的这一个线程**不停地去队列里取任务执行**

**SingleThreadExecutor 用于串行执行任务的场景，每个任务必须按顺序执行，不需要并发执行**。



**newFixedThreadPool**

```Java
public class FixedThreadPoolTest {

    public static void main(String[] args) {
        ExecutorService executorService = Executors.newFixedThreadPool(2);
        MyRunnable myRunnable = new MyRunnable();
        for (int i = 0; i < 5; i++) {
            executorService.execute(myRunnable);
        }

        System.out.println("线程任务开始执行");
        executorService.shutdown();
    }

}
```

输出结果

```Java
线程任务开始执行
pool-1-thread-1 is running...
pool-1-thread-1 is running...
pool-1-thread-2 is running...
pool-1-thread-1 is running...
pool-1-thread-2 is running...
```

底层实现

```Java
public static ExecutorService newFixedThreadPool(int nThreads) {
    return new ThreadPoolExecutor(nThreads, nThreads,
                                  0L, TimeUnit.MILLISECONDS,
                                  new LinkedBlockingQueue<Runnable>());
}
```

​		可以看到，FixedThreadPool 的**核心线程数和最大线程数都是指定值**，也就是说当线程池中的线程数超过核心线程数后，任务都会被放到阻塞队列中。此外 **keepAliveTime 为 0，也就是多余的空余线程会被立即终止**（由于这里没有多余线程，这个参数也没什么意义了）。而这里选用的**阻塞队列是 LinkedBlockingQueue**，使用的是默认容量 Integer.MAX_VALUE，相当于没有上限。

​		因此这个线程池执行任务的流程如下：

1. 线程数少于核心线程数，也就是设置的线程数时，新建线程执行任务
2. 线程数等于核心线程数后，**将任务加入阻塞队列**
3. 由于队列容量非常大，可以一直加
4. 执行完任务的线程反复去队列中取任务执行

**FixedThreadPool 用于负载比较重的服务器，为了资源的合理利用，需要限制当前线程数量**。



**newCachedThreadPool**

```Java
public class CachedThreadPoolTest {

    public static void main(String[] args) {
        ExecutorService executorService = Executors.newCachedThreadPool();
        MyRunnable myRunnable = new MyRunnable();
        for (int i = 0; i < 5; i++) {
            executorService.execute(myRunnable);
        }

        System.out.println("线程任务开始执行");
        executorService.shutdown();
    }

}
```

输出结果

```Java
线程任务开始执行
pool-1-thread-1 is running...
pool-1-thread-4 is running...
pool-1-thread-2 is running...
pool-1-thread-5 is running...
pool-1-thread-3 is running...
```

底层实现

```Java
public static ExecutorService newCachedThreadPool() {
    return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                  60L, TimeUnit.SECONDS,
                                  new SynchronousQueue<Runnable>());
}
```

​		可以看到，CachedThreadPool **没有核心线程**，**非核心线程数无上限**，也就是全部使用外包，但是每个外包空闲的时间**只有 60 秒**，超过后就会被回收。

​		CachedThreadPool 使用的队列是 **SynchronousQueue**，这个队列的作用就是**传递任务**，并不会保存。

​		因此当提交任务的速度大于处理任务的速度时，**每次提交一个任务，就会创建一个线程**。极端情况下会创建过多的线程，**耗尽 CPU 和内存资源**。

​		它的执行流程如下：

1. 没有核心线程，直接向 SynchronousQueue 中提交任务
2. 如果有空闲线程，**就去取出任务执行**；如果没有空闲线程，就新建一个
3. 执行完任务的线程有 60 秒生存时间，如果在这个时间内**可以接到新任务，就可以继续活下去，否则就拜拜**
4. 由于空闲 60 秒的线程会被终止，**长时间保持空闲的 CachedThreadPool 不会占用任何资源**。

​       **CachedThreadPool 用于并发执行大量短期的小任务，或者是负载较轻的服务器**。



**newScheduledThreadPool**

```Java
public class ScheduledThreadPoolTest {

    public static void main(String[] args) {
        ScheduledExecutorService scheduledExecutorService = Executors.newScheduledThreadPool(3);
        MyRunnable myRunnable = new MyRunnable();
        for (int i = 0; i < 5; i++) {
            
            scheduledExecutorService.scheduleAtFixedRate(myRunnable, 1, 2, TimeUnit.SECONDS);
        }

        System.out.println("线程任务开始执行");
    }

}
```

输出结果

```Java
线程任务开始执行

pool-1-thread-1 is running...
pool-1-thread-2 is running...
pool-1-thread-1 is running...
pool-1-thread-3 is running...
pool-1-thread-2 is running...

pool-1-thread-1 is running...
pool-1-thread-3 is running...
pool-1-thread-2 is running...
pool-1-thread-1 is running...
pool-1-thread-3 is running...
```

底层实现

```Java
public ScheduledThreadPoolExecutor(int corePoolSize) {
    super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
          new DelayedWorkQueue());
}
```

​		ScheduledThreadPoolExecutor 的执行流程如下：

1. 添加一个任务
2. 线程池中的线程从 DelayQueue 中取任务
3. 然后执行任务

​       具体执行任务的步骤也比较复杂：

1. 线程**从 DelayQueue 中获取 time 大于等于当前时间的 ScheduledFutureTask**
2. 执行完后修改这个 **task 的 time 为下次被执行的时间**
3. 然后再把这个 task 放回队列中

**ScheduledThreadPoolExecutor 用于需要多个后台线程执行周期任务，同时需要限制线程数量的场景**。



###### 3.Executors和ThreaPoolExecutor创建线程池的区别

​		Executors 各个方法的弊端：

1. newFixedThreadPool 和 newSingleThreadExecutor:
    主要问题是**堆积的请求处理队列可能会耗费非常大的内存**，甚至 OOM。
2. newCachedThreadPool 和 newScheduledThreadPool:
    主要问题是**线程数最大数是 Integer.MAX_VALUE**，可能会创建数量非常多的线程，甚至 OOM。

ThreaPoolExecutor

1. 创建线程池方式只有一种，就是**走它的构造函数**，参数自己指定



###### 4.ScheduledThreadPoolExecutor详解

- ScheduledThreadPoolExecutor可以用来在**给定延时后执行异步任务或者周期性执行任务**，相对于任务调度的Timer来说，其功能更加强大，Timer只能使用一个后台线程执行任务，而ScheduledThreadPoolExecutor则可以通过构造函数来指定后台线程的个数。

1. ScheduledThreadPoolExecutor继承了`ThreadPoolExecutor`，也就是说ScheduledThreadPoolExecutor拥有execute()和submit()提交异步任务的基础功能。ScheduledThreadPoolExecutor类实现了`ScheduledExecutorService`，该接口定义了ScheduledThreadPoolExecutor能够延时执行任务和周期执行任务的功能；
2. ScheduledThreadPoolExecutor也两个重要的内部类：**DelayedWorkQueue**和**ScheduledFutureTask**。可以看出DelayedWorkQueue实现了BlockingQueue接口，也就是一个阻塞队列，ScheduledFutureTask则是继承了FutureTask类，也表示该类用于返回异步任务的结果。这两个关键类，下面会具体详细来看。

**1.构造方法**

​		ScheduledThreadPoolExecutor有如下几个构造方法：

```Java
public ScheduledThreadPoolExecutor(int corePoolSize) {
    super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
          new DelayedWorkQueue());
}

public ScheduledThreadPoolExecutor(int corePoolSize,
                                   ThreadFactory threadFactory) {
    super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
          new DelayedWorkQueue(), threadFactory);
}

public ScheduledThreadPoolExecutor(int corePoolSize,
                                   RejectedExecutionHandler handler) {
    super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
          new DelayedWorkQueue(), handler);
}

public ScheduledThreadPoolExecutor(int corePoolSize,
                                   ThreadFactory threadFactory,
                                   RejectedExecutionHandler handler) {
    super(corePoolSize, Integer.MAX_VALUE, 0, NANOSECONDS,
          new DelayedWorkQueue(), threadFactory, handler);
}
```

​		可以看出由于ScheduledThreadPoolExecutor继承了ThreadPoolExecutor，它的构造方法实际上是调用了ThreadPoolExecutor，理解ThreadPoolExecutor构造方法的几个参数的意义后，理解这就很容易了。可以看出，ScheduledThreadPoolExecutor的核心线程池的线程个数为**指定的corePoolSize**，当核心线程池的线程个数达到corePoolSize后，就会将任务提交给有界阻塞队列**DelayedWorkQueue**，对DelayedWorkQueue在下面进行详细介绍，线程池允许最大的线程个数为Integer.MAX_VALUE，也就是说理论上这是一个大小无界的线程池。



**特有方法**

ScheduledThreadPoolExecutor实现了`ScheduledExecutorService`接口，该接口定义了**可延时执行异步任务和可周期执行异步任务的特有功能**，相应的方法分别为：

```Java
public ScheduledFuture<?> schedule(Runnable command,
                                       long delay, TimeUnit unit);
public <V> ScheduledFuture<V> schedule(Callable<V> callable,
                                           long delay, TimeUnit unit);
public ScheduledFuture<?> scheduleAtFixedRate(Runnable command,
                                                  long initialDelay,
                                                  long period,
                                                  TimeUnit unit);
public ScheduledFuture<?> scheduleWithFixedDelay(Runnable command,
                                                     long initialDelay,
                                                     long delay,
                                                     TimeUnit unit);
```

**可周期性执行的任务-ScheduledFutureTask**

​		ScheduledThreadPoolExecutor最大的特色是能够**周期性执行异步任务**，当调用`schedule,scheduleAtFixedRate和scheduleWithFixedDelay方法`时，实际上是将提交的任务转换成的**ScheduledFutureTask**类，从源码就可以看出。以schedule方法为例：

```Java
public ScheduledFuture<?> schedule(Runnable command,
                                   long delay,
                                   TimeUnit unit) {
    if (command == null || unit == null)
        throw new NullPointerException();
    RunnableScheduledFuture<?> t = decorateTask(command,
        new ScheduledFutureTask<Void>(command, null,
                                      triggerTime(delay, unit)));
    delayedExecute(t);
    return t;
}
```

​		可以看出，通过`decorateTask`会将传入的Runnable转换成`ScheduledFutureTask`类。**线程池最大作用是将任务和线程进行解耦**，线程主要是任务的执行者，而任务也就是现在所说的**ScheduledFutureTask**。紧接着，会想到任何线程执行任务，总会调用`run()`方法。为了保证ScheduledThreadPoolExecutor能够延时执行任务以及能够周期性执行任务，ScheduledFutureTask重写了run方法：

```Java
public void run() {
    boolean periodic = isPeriodic();
    if (!canRunInCurrentRunState(periodic))
        cancel(false);
    else if (!periodic)
		
        ScheduledFutureTask.super.run();
		
    else if (ScheduledFutureTask.super.runAndReset()) {
        setNextRunTime();
        reExecutePeriodic(outerTask);
    }
}
```

​		从源码可以很明显的看出，在重写的run方法中会先`if (!periodic)`判断当前任务是否是周期性任务，如果不是的话就直接调用`run()方法`；否则的话执行`setNextRunTime()`方法重设下一次任务执行的时间，并通过`reExecutePeriodic(outerTask)`方法将下一次待执行的任务放置到`DelayedWorkQueue`中。

​		因此，可以得出结论：**`ScheduledFutureTask`最主要的功能是根据当前任务是否具有周期性，对异步任务进行进一步封装。如果不是周期性任务（调用schedule方法）则直接通过`run()`执行，若是周期性任务，则需要在每一次执行完后，重设下一次执行的时间，然后将下一次任务继续放入到阻塞队列中。**



**3.DelayedWorkQueue**

​		在ScheduledThreadPoolExecutor中还有另外的一个重要的类就是DelayedWorkQueue。为了实现其ScheduledThreadPoolExecutor能够延时执行异步任务以及能够周期执行任务，DelayedWorkQueue进行相应的封装。DelayedWorkQueue是一个基于堆的数据结构，类似于DelayQueue和PriorityQueue。在执行定时任务的时候，每个任务的执行时间都不同，所以DelayedWorkQueue的工作就是按照执行时间的升序来排列，执行时间距离当前时间越近的任务在队列的前面。

> 为什么要使用DelayedWorkQueue呢？

​		定时任务执行时需要取出最近要执行的任务，所以任务在队列中每次出队时一定要是当前队列中执行时间最靠前的，所以自然要使用优先级队列。

​		DelayedWorkQueue是一个优先级队列，它可以保证每次出队的任务都是当前队列中执行时间最靠前的，由于它是基于堆结构的队列，堆结构在执行插入和删除操作时的最坏时间复杂度是 O(logN)。

> DelayedWorkQueue的数据结构

```Java
private static final int INITIAL_CAPACITY = 16;
private RunnableScheduledFuture<?>[] queue =
    new RunnableScheduledFuture<?>[INITIAL_CAPACITY];
private final ReentrantLock lock = new ReentrantLock();
private int size = 0;
```

​		可以看出DelayedWorkQueue底层是采用数组构成的，很详细。关于DelayedWorkQueue我们可以得出这样的结论：**DelayedWorkQueue是基于堆的数据结构，按照时间顺序将每个任务进行排序，将待执行时间越近的任务放在在队列的队头位置，以便于最先进行执行**。



**4.ScheduledThreadPoolExecutor执行过程**

​		现在我们对ScheduledThreadPoolExecutor的两个内部类ScheduledFutueTask和DelayedWorkQueue进行了了解，实际上这也是线程池工作流程中最重要的两个关键因素：**任务以及阻塞队列**。现在我们来看下ScheduledThreadPoolExecutor提交一个任务后，整体的执行过程。以ScheduledThreadPoolExecutor的schedule方法为例，具体源码为：

```Java
public ScheduledFuture<?> schedule(Runnable command,
                                   long delay,
                                   TimeUnit unit) {
    if (command == null || unit == null)
        throw new NullPointerException();
	
    RunnableScheduledFuture<?> t = decorateTask(command,
        new ScheduledFutureTask<Void>(command, null,
                                      triggerTime(delay, unit)));
    
	delayedExecute(t);
    return t;
}
```

​		方法很容易理解，为了满足ScheduledThreadPoolExecutor能够延时执行任务和能周期执行任务的特性，会先将实现Runnable接口的类转换成ScheduledFutureTask。然后会调用`delayedExecute`方法进行执行任务，这个方法也是关键方法，来看下源码：

```Java
private void delayedExecute(RunnableScheduledFuture<?> task) {
    if (isShutdown())
		
        reject(task);
    else {
		
        super.getQueue().add(task);
        if (isShutdown() &&
            !canRunInCurrentRunState(task.isPeriodic()) &&
            remove(task))
            task.cancel(false);
        else
			
            ensurePrestart();
    }
}
```

`delayedExecute`方法的主要逻辑请看注释，可以看出该方法的重要逻辑会是在`ensurePrestart()`方法中，它的源码为：

```Java
void ensurePrestart() {
    int wc = workerCountOf(ctl.get());
    if (wc < corePoolSize)
        addWorker(null, true);
    else if (wc == 0)
        addWorker(null, false);
}
```

​		可以看出该方法逻辑很简单，关键在于它所调用的`addWorker方法`，该方法主要功能：**新建`Worker类`，当执行任务时，就会调用被`Worker所重写的run方法`，进而会继续执行`runWorker`方法。在`runWorker`方法中会调用`getTask`方法从阻塞队列中不断的去获取任务进行执行，直到从阻塞队列中获取的任务为null的话，线程结束终止**。addWorker方法是ThreadPoolExecutor类中的方法。



**5.总结**

1. ScheduledThreadPoolExecutor继承了ThreadPoolExecutor类，因此，整体上功能一致，线程池主要**负责创建线程（Worker类）**，线程从阻塞队列中**不断获取新的异步任务**，直到阻塞队列中已经没有了异步任务为止。但是相较于ThreadPoolExecutor来说，ScheduledThreadPoolExecutor具有延时执行任务和可周期性执行任务的特性，ScheduledThreadPoolExecutor重新设计了任务类`ScheduleFutureTask`，ScheduleFutureTask重写了`run`方法使其具有可延时执行和可周期性执行任务的特性。另外，阻塞队列`DelayedWorkQueue`是可根据优先级排序的队列，采用了堆的底层数据结构，使得与当前时间相比，待执行时间越靠近的任务放置队头，以便线程能够获取到任务进行执行；

2. 线程池无论是ThreadPoolExecutor还是ScheduledThreadPoolExecutor，在设计时的三个关键要素是：**任务，执行者以及任务结果**。它们的设计思想也是完全将这三个关键要素进行了解耦。

    **执行者**

    任务的执行机制，完全交由`Worker类`，也就是进一步了封装了Thread。向线程池提交任务，无论为ThreadPoolExecutor的execute方法和submit方法，还是ScheduledThreadPoolExecutor的schedule方法，都是先将任务移入到阻塞队列中，然后通过addWork方法新建了Work类，并通过runWorker方法启动线程，并不断的从阻塞对列中获取异步任务执行交给Worker执行，直至阻塞队列中无法取到任务为止。

    **任务**

    在ThreadPoolExecutor和ScheduledThreadPoolExecutor中任务是指实现了Runnable接口和Callable接口的实现类。ThreadPoolExecutor中会将任务转换成`FutureTask`类，而在ScheduledThreadPoolExecutor中为了实现可延时执行任务和周期性执行任务的特性，任务会被转换成`ScheduledFutureTask`类，该类继承了FutureTask，并重写了run方法。

    **任务结果**

    在ThreadPoolExecutor中提交任务后，获取任务结果可以通过Future接口的类，在ThreadPoolExecutor中实际上为FutureTask类，而在ScheduledThreadPoolExecutor中则是`ScheduledFutureTask`类



###### 5.FutureTask详解

​		在Executors框架体系中，**FutureTask用来表示可获取结果的异步任务**。FutureTask实现了Future接口，FutureTask提供了**启动和取消异步任务**，查询异步任务是否计算结束以及获取最终的异步任务的结果的一些常用的方法。通过`get()`方法来获取异步任务的结果，但是**会阻塞当前线程直至异步任务执行结束**。一旦任务执行结束，任务不能重新启动或取消，除非调用`runAndReset()`方法。在FutureTask的源码中为其定义了这些状态：

```Java
private static final int NEW          = 0;
private static final int COMPLETING   = 1;
private static final int NORMAL       = 2;
private static final int EXCEPTIONAL  = 3;
private static final int CANCELLED    = 4;
private static final int INTERRUPTING = 5;
private static final int INTERRUPTED  = 6;
```

​		另外，在《Java并发编程的艺术》一书，作者根据FutureTask.run()方法的执行的时机，FutureTask分为了3种状态：

1. **未启动**。FutureTask.run()方法还没有被执行之前，FutureTask处于未启动状态。当创建一个FutureTask，还没有执行FutureTask.run()方法之前，FutureTask处于未启动状态。
2. **已启动**。FutureTask.run()方法被执行的过程中，FutureTask处于已启动状态。
3. **已完成**。FutureTask.run()方法执行结束，或者调用FutureTask.cancel(…)方法取消任务，或者在执行任务期间抛出异常，这些情况都称之为FutureTask的已完成状态。

​       由于FutureTask具有这三种状态，因此执行FutureTask的get方法和cancel方法，当前处于不同的状态对应的结果也是大不相同。这里对get方法和cancel方法做个总结：

> get方法

​		当FutureTask处于未启动或已启动状态时，执行FutureTask.get()方法将导致调用线程阻塞。如果FutureTask处于已完成状态，调用FutureTask.get()方法将导致调用线程立即返回结果或者抛出异常

> cancel方法

当FutureTask处于**未启动状态**时，执行FutureTask.cancel()方法将此任务永远不会执行；

当FutureTask处于**已启动状态**时，执行FutureTask.cancel(true)方法将以中断线程的方式来阻止任务继续进行，如果执行FutureTask.cancel(false)将不会对正在执行任务的线程有任何影响；

当**FutureTask**处于已完成状态时，执行FutureTask.cancel(…)方法将返回false。



**FutureTask的基本使用**

​		FutureTask除了实现Future接口外，还**实现了Runnable接口**。因此，FutureTask**可以交给Executor执行**，也可以由调用的线程直接执行（FutureTask.run()）。另外，FutureTask的获取也可以通过ExecutorService.submit()方法返回一个FutureTask对象，然后在通过FutureTask.get()或者FutureTask.cancel方法。

​		**应用场景**：**当一个线程需要等待另一个线程把某个任务执行完后它才能继续执行，此时可以使用FutureTask**。假设有多个线程执行若干任务，每个任务最多只能被执行一次。当多个线程试图执行同一个任务时，只允许一个线程执行任务，其他线程需要等待这个任务执行完后才能继续执行。





#### 4.Java并发关键字

​		**并发关键字总论**

-   **synchronized：具有原子性，有序性和可见性**；
-   **volatile：具有有序性和可见性**；
-   **final：具有可见性**

##### 1.Java并发关键字-synchronized

​		共享数据的线程安全问题怎样处理？很自然而然的想法就是每一个线程依次去**读写这个共享变量**，这样就不会有任何数据安全的问题，因为每个线程所操作的都是**当前最新的版本数据**。那么，在Java关键字synchronized就具有**使每个线程依次排队操作共享变量的功能**。很显然，这种**同步机制效率很低**，但synchronized是其他并发容器实现的基础，对它的理解也会大大提升对并发编程的感觉，从功利的角度来说，这也是面试高频的考点。好了，下面，就来具体说说这个关键字。


###### synchronized实现原理

**对象锁（monitor）机制**

​		使用synchronized进行同步，其关键就是必须要**对对象的监视器monitor进行获取**，当线程获取monitor后才能继续往下执行，否则就只能等待。而这个**获取的过程是互斥**的，即同一时刻只有一个线程能够获取到monitor。上面的demo中在执行完同步代码块之后紧接着再会去执行一个静态同步方法，而这个方法锁的对象依然就这个类对象，那么这个**正在执行的线程还需要获取该锁吗？答案是不必的**，执行静态同步方法的时候只有一条monitorexit指令，并没有monitorenter获取锁的指令。这就是**锁的重入性**，即在同一锁程中，线程不需要再次获取同一把锁。**synchronized先天具有重入性**。每个对象拥有一个计数器，当线程获取该对象锁后，计数器就会加一，释放锁后就会将计数器减一。




##### 2.Java并发关键字-volatile

###### volatile实现原理  ①禁止指令重排序  ②内存屏障（内存可见性）

​		volatile是怎样实现的？比如一个很简单的Java代码：

```Java
instance = new Instancce() //instance是volatile变量
```

​		在生成汇编代码时会在volatile修饰的共享变量进行写操作的时候会多出**Lock前缀的指令**（具体的大家可以使用一些工具去看一下，这里我就只把结果说出来）。我们想这个Lock指令肯定有神奇的地方，那么Lock前缀的指令在多核处理器下会发现什么事情了？主要有这两个方面的影响：

- 将当前处理器**缓存行的数据写回系统内存**；
- 这个写回内存的操作会使得其他CPU里**缓存了该内存地址的数据无效.**

​       为了提高处理速度，处理器不直接和内存进行通信，而是先将系统内存的数据**读到内部缓存**（L1，L2或其他）后再进行操作，但操作完**不知道何时会写到内存**。如果对声明了volatile的变量进行写操作，JVM就会向处理器发送一条**Lock前缀的指令**，将这个变量所在缓存行的**数据写回到系统内存**。但是，就算写回到内存，如果其他处理器缓存的值还是旧的，再执行计算操作就会有问题。所以，在多处理器下，为了保证各个处理器的缓存是一致的，就会实现**缓存一致性协议**，每个处理器通过**嗅探在总线上传播的数据来检查自己缓存的值是不是过期了**，当处理器发现自己缓存行**对应的内存地址被修改**，就会将当前处理器的**缓存行设置成无效状态**，当处理器对这个数据进行修改操作的时候，会重新**从系统内存中把数据读到处理器缓存里**。因此，经过分析我们可以得出如下结论：

- Lock前缀的指令会**引起处理器缓存写回内存**；
- 一个处理器的缓存回写到内存会**导致其他处理器的缓存失效**；
- 当处理器发现本地缓存失效后，就会**从内存中重读该变量数据**，即可以获取当前最新值。

​       这样针对volatile变量通过这样的机制就使得每个线程都能获得该变量的最新值。



##### 3.Java并发关键字-final

​		在Java中变量，可以分为成员变量以及方法局部变量。因此也是按照这种方式依次来说，以避免漏掉任何一个死角。

###### **final成员变量**

​		通常每个类中的成员变量可以分为**类变量**（static修饰的变量）以及**实例变量**。针对这两种类型的变量赋初值的时机是不同的，类变量可以在**声明变量的时候直接赋初值**或者在**静态代码块中给类变量赋初值**。而实例变量可以在声明变量的时候给实例变量赋初值，在非静态初始化块中以及构造器中赋初值。类变量有两个时机赋初值，而实例变量则可以有三个时机赋初值。当final变量未初始化时系统**不会进行隐式初始化**，会出现报错。

###### final局部变量

​		final局部变量由程序员**进行显式初始化**，如果final局部变量已经进行了初始化则后面**就不能再次进行更改**，如果final变量**未进行初始化，可以进行赋值**，**当且仅有一次**赋值，一旦赋值之后**再次赋值就会出错**。



#### 5.Java锁机制

##### 1.锁基础概念

###### 1.锁与互斥

​		在计算机科学中，**锁(lock)或互斥(mutex)是一种同步机制**，用于在有许多执行线程的环境中强制对资源的访问限制。锁旨在**强制实施互斥排他**、并发控制策略。锁通常需要**硬件支持**才能有效实施。这种支持通常采取一个或多个原子指令的形式，如"test-and-set", "fetch-and-add" or "compare-and-swap"”。这些指令允许单个进程测试锁是否空闲，如果空闲，则通过单个原子操作获取锁。锁的一个重要属性是 **粒度 Granularity [grænjʊ‘lærɪtɪ]**，在引入锁粒度之前，需要了解关于锁的三个概念：

​		**1.锁开销 lock overhead**

​		锁占用的**内存空间，**cpu初始化和销毁锁、获取和释放锁的时间。程序使用的锁越多，相应的锁开销越大

​		**2.锁竞争 lock contention**

​		一个进程或线程试图获取另一个进程或线程持有的锁，就会发生**锁竞争**。锁粒度越小，发生锁竞争的可能性就越小

​		**3.死锁 deadlock**

​		至少两个任务中的每一个都等待另一个任务持有的锁的情况

​		锁粒度是**衡量锁保护的数据量大小**，通常选择粗粒度的锁(锁的数量少，每个锁保护大量的数据)，在当单进程访问受保护的数据时锁开销小，但是当多个进程同时访问时性能很差。因为增大了锁的竞争。相反，使用细粒度的锁(锁数量多，每个锁保护少量的数据)**增加了锁的开销但是减少了锁竞争**。例如数据库中，锁的粒度有**表锁、页锁、行锁、字段锁、字段的一部分锁**。



###### 2.Java对象头与Monitor

**Java对象头**

​		在操作同步资源之前需要给同步资源先加锁，这把**锁就是存在Java对象头里的**，Java对象头是什么？以Hotspot虚拟机为例，Hotspot的对象头主要包括两部分数据：**Mark Word**（标记字段）、**Class Pointer**（类型指针）。

​		**Markword**

​		markword是java**对象头中的一部分**，这里只做markword的简单介绍，因为对象的markword和java各种类型的锁密切相关；markword数据的长度在32位和64位的虚拟机（未开启压缩指针）中分别为**32bit和64bit**，它的**最后2bit是锁状态标志位**，用来标记当前对象的状态，对象的所处的状态，决定了markword存储的内容，如下表所示:

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200327003928796.png" alt="image-20200327003928796" style="zoom:45%;" />

​		**Class Pointer**

​		对象指向它的**类元数据**的指针，虚拟机通过这个指针**来确定这个对象是哪个类的实例**。

​		**Monitor监视器**

​		Monitor可以理解为一个**同步工具**或一种**同步机制**，通常被描述为**一个对象**。每一个Java对象就有一把看不见的锁，称为**内部锁**或者**Monitor锁**。

​		Monitor是**线程私有**的数据结构，每一个线程都有一个可用**monitor record列表**，同时还有一个**全局的可用列表**。每一个被锁住的对象都会和**一个monitor关联**，同时monitor中有一个**Owner字段存放拥有该锁的线程的唯一标识**，表示该锁被这个线程占用。

​		synchronized通过Monitor来**实现线程同步**，Monitor则依赖于底层的操作系统的**Mutex Lock**（互斥锁）来实现的线程同步。这种方式就是synchronized**最初实现同步**的方式，这就是JDK 6之前synchronized**效率低**的原因。这种**依赖于操作系统Mutex Lock所实现的锁我们称之为“重量级锁”**，JDK 6中为了减少获得锁和释放锁带来的性能消耗，引入了“偏向锁”和“轻量级锁”。所以目前**Java锁**一共有**4种状态**，级别从低到高依次是：**无锁、偏向锁、轻量级锁和重量级锁**。锁状态**只能升级不能降级**。



###### 3.两种基本技术：AQS与CAS

**AQS**

​		AbstractQueuedSynchronized **抽象队列式同步器**，AQS定义了一套**多线程访问共享资源的同步器框架**，许多同步类实现都依赖于它，如常用的**ReentrantLock**/**Semaphore**/**CountDownLatch**…

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200327010453513.png" alt="image-20200327010453513" style="zoom:45%;" />

​		AQS维护了一个**volatile int state**（代表共享资源）和一个**FIFO线程等待队列**（多线程争用资源被阻塞时会进入此队列）。state的访问方式有三种:

-   getState()
-   setState()
-   compareAndSetState()

​       AQS定义**两种资源共享方式**：**Exclusive**（独占，只有一个线程能执行，如ReentrantLock）和**Share**（共享，多个线程可同时执行，如Semaphore/CountDownLatch）。

**CAS**

​		CAS(Compare and Swap 比较并交换)是**乐观锁技术**，当多个线程尝试使用CAS同时更新同一个变量时，只有其中**一个线程能更新变量的值**，而其它线程都失败，失败的线程并不会被挂起，而是被告知这次**竞争中失败**，并可以再次尝试。　　　

​		CAS操作中包含**三个操作数**——需要读写的**内存位置(V)**、进行比较的**预期原值(A)**和**拟写入的新值(B)**。如果内存位置V的值与预期原值A相匹配，那么处理器会自动将该位置值更新为新值B，**否则处理器不做任何操作**。无论哪种情况，它都会在CAS 指令之前返回该位置的值（在CAS的一些特殊情况下将仅返回CAS是否成功，而不提取当前值）。CAS有效地说明了“ 我认为位置V应该包含值A；如果包含该值，则将 B放到这个位置；否则，不要更改该位置，只告诉我这个位置现在的值即可”。这其实和乐观锁的**冲突检查 + 数据更新**的原理是一样的。CAS虽然很高效，但是它也存在三大问题，这里也简单说一下：

-   **ABA问题**。CAS需要在操作值的时候检查内存值是否发生变化，没有发生变化才会更新内存值。但是如果内存值原来是A，后来变成了B，然后又变成了A，那么CAS进行检查时会发现值没有发生变化，但是实际上是有变化的。ABA问题的解决思路就是在变量前面添加版本号，每次变量更新的时候都把版本号加一，这样变化过程就从“A－B－A”变成了“1A－2B－3A”。JDK从1.5开始提供了AtomicStampedReference类来解决ABA问题，具体操作封装在compareAndSet()中。compareAndSet()首先检查当前引用和当前标志与预期引用和预期标志是否相等，如果都相等，则以原子方式将引用值和标志的值设置为给定的更新值。
-   **循环时间长开销大**。CAS操作如果长时间不成功，会导致其一直自旋，给CPU带来非常大的开销。
-   **只能保证一个共享变量的原子操作**。对一个共享变量执行操作时，CAS能够保证原子操作，但是对多个共享变量操作时，CAS是无法保证操作的原子性的。Java从1.5开始JDK提供了AtomicReference类来保证引用对象之间的原子性，可以**把多个变量放在一个对象里来进行CAS操作**。

##### 2.Java锁具体分类

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200327152608627.png" alt="image-20200327152608627" style="zoom:70%;" />

​		以下是很多锁的名词，这些分类**并不全是**是指锁的**状态**，有的指锁的**特性**，有的指锁的**设计**，下面总结的内容是对每个锁的名词进行一定的解释。

###### 1.乐观锁 与 悲观锁（自旋锁/偏向锁/轻量级锁/重量级锁）

**乐观锁**：乐观锁是一种**乐观思想**，即认为**读多写少，遇到并发写的可能性低**，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，采取在写时先读出当前版本号，然后加锁操作（比较跟上一次的版本号，如果一样则更新），如果失败则要重复读-比较-写的操作。java中的乐观锁基本都是通过**CAS操作**实现的，CAS是一种更新的**原子操作**，比较当前值跟传入值**是否一样**，一样则更新，否则失败。

-   **自旋锁/无锁**：是指尝试获取锁的线程**不会立即阻塞**，而是采用循环的方式去尝试获取锁，这样的好处是减少线程上下文切换的消耗，缺点是**循环会消耗CPU**。如果持有锁的线程能在**很短时间内释放**锁资源，那么那些等待竞争锁的线程就**不需要做内核态和用户态之间的资源消耗与切换进入阻塞挂起状态**，它们只需要等一等（自旋），等持有锁的线程释放锁后即可**立即获取锁**。但是线程自旋是**需要消耗cpu的**，所以需要设定一个自旋等待的**最大时间**, 如果持有锁的线程执行的时间超过自旋等待的最大时间仍没有释放锁，这时争用线程会**停止自旋**进入阻塞状态。自旋锁的实现原理是**CAS**，实质上是无锁状态，**没有对资源进行锁定**。

    <img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200327154613539.png" alt="image-20200327154613539" style="zoom:48%;" />

    -   **1. 自旋锁优缺点与使用场景**：自旋锁尽可能的**减少线程的阻塞**，这对于锁的竞争不激烈，且占用锁时间非常短的代码块来说性能能大幅度的提升，因为自旋的消耗会小于线程阻塞挂起再唤醒的操作的消耗，这些操作会导致线程发生两次上下文切换；是如果锁的竞争激烈，或者**持有锁的线程需要长时间占用锁执行同步块**，这时候就不适合使用自旋锁了。

    -   **2. JVM对于自旋周期的选择**：jdk1.5这个限度是**写死的**，在1.6引入了**适应性自旋锁**，适应性自旋锁意味着自旋的时间**不再是固定的了**，而是由**前一次在同一个锁上的自旋时间**以及**锁的拥有者的状态来决定**，基本认为**一个线程上下文切换的时间**是最佳的一个自旋周期，同时JVM还针对当前CPU的负荷情况做了优化: 

        1.如果平均负载小于CPUs则**一直自旋**，如果CPU处于**节电模式**则停止自旋**进入阻塞**；

        2.如果有**超过(CPUs/2)个线程**正在自旋，则后来线程直接阻塞；

        3.如果正在自旋的线程发现**Owner发生了变化迟自旋时间**（自旋计数）或**进入阻塞**；

        4.自旋时间的最坏情况是CPU的**存储延迟**（CPU A存储了一个数据，到CPU B得知这个数据直接的时间差）；自旋时会**适当放弃**线程优先级之间的差异；

        5.如果对于某个锁，**自旋很少成功获得过**，那在以后尝试获取这个锁时将可能**省略掉自旋过程**，**直接阻塞线程**，避免浪费处理器资源。

-   **偏向锁**：是指一段同步代码**一直被一个线程**所访问，那么该线程会**自动获取锁**。降低获取锁的**代价**。

    ​		**Java偏向锁(Biased Locking)**是Java6引入的一项**多线程优化**。 它通过**消除资源无竞争情况下的同步原语**，进一步提高了程序的运行性能。它会偏向于**第一个访问锁的线程**，如果在运行过程中，同步锁只有一个线程访问，不存在多线程争用的情况，则线程是**不需要触发同步**的，这种情况下，就会给线程**加一个偏向锁**。如果在运行过程中，遇到了其他线程抢占锁，则持有偏向锁的线程**会被挂起**，JVM会消除它身上的偏向锁，将锁**恢复到标准的轻量级锁**。偏向锁使用了一种**等到竞争出现才释放锁的机制**，所以当其他线程**尝试竞争偏向锁**时，持有偏向锁的线程**才会释放锁**，偏向锁只需要在置换ThreadID的时候**依赖一次CAS原子指令即可**。

    -   **1.偏向锁获取过程**：

        1.访问MarkWord中偏向锁的标识是否设置成1，锁标志位是否为01，确认为**可偏向状态**。

        2.如果为可偏向状态，则测试线程ID是否**指向当前线程**，如果是，进入步骤5，否则，进入步骤3。

        3.如果线程ID并未指向当前线程，则通过**CAS操作竞争锁**。如果竞争成功，则将Mark Word中线程ID设置为当前线程ID，然后**执行5**；如果竞争失败，**执行4**。

        4.如果CAS获取偏向锁失败，则**表示有竞争**。当到达**全局安全点**（safepoint）时获得偏向锁的线程被挂起，偏向锁升级为轻量级锁，然后**被阻塞在安全点的线程**继续往下执行**同步代码**。

        5.执行同步代码。

    -   **2.偏向锁的释放：** 偏向锁的撤销在上述第四步中有提到。偏向锁只有遇到其他线程尝试竞争偏向锁时，持有偏向锁的线程**才会释放锁**，线程**不会主动去释放偏向锁**。偏向锁的撤销，需要**等待全局安全点**（在这个时间点上没有字节码正在执行），它会首先**暂停拥有偏向锁的线程**，判断锁对象**是否处于被锁定状态**，撤销偏向锁后恢复到**未锁定**（标志位为“01”）或**轻量级锁**（标志位为“00”）的状态。撤销偏向锁的时候会导致**stop the word**。

    -   **3.偏向锁的使用场景：**在锁基本**无竞争的情况下使用**，一旦有了竞争就升级为**轻量级锁**，升级为轻量级锁的时候需要**撤销偏向锁**，撤销偏向锁的时候**会导致stop the word操作**；在有锁的竞争时，偏向锁会**多做很多额外操作**，尤其是撤销偏向锁的时候**会导致进入安全点**，安全点会导致**stw**，导致性能下降，这种情况下应当禁用。 **jvm开启/关闭偏向锁** 

        ​	**1.开启偏向锁**：-XX:+UseBiasedLocking -XX:BiasedLockingStartupDelay=0

        ​	**2.关闭偏向锁**：-XX:-UseBiasedLocking

-   **轻量级锁**：是指当锁是偏向锁的时候，被**另一个线程所访问**，偏向锁就会升级为**轻量级锁**，其他线程会通过**自旋的形式尝试获取锁，不会阻塞，提高性能**。轻量级锁是由偏向所**升级来的**，偏向锁运行在一个线程进入同步块的情况下，当第二个线程**加入锁争用**的时候，偏向锁就会**升级为轻量级锁**；

    -   **1.轻量级锁的加锁过程**：

        1.在代码进入同步块的时候，如果同步对象锁状态为**无锁状态**（锁标志位为“01”状态，是否为偏向锁为“0”），虚拟机首先将在当前线程的**栈帧**中建立一个名为**锁记录**（Lock Record）的空间，用于存储锁对象目前的Mark Word的拷贝，拷贝对象头中的Mark Word复制到锁记录中。即 Displaced Mark Word。

        2.拷贝成功后，虚拟机将使用**CAS操作**尝试将对象的Mark Word**更新为指向Lock Record的指针**，并将Lock record里的**owner指针指向object mark word**。如果更新成功，则执行步骤4，否则执行步骤5。

        3.如果这个更新动作成功了，那么这个线程就**拥有了该对象的锁**，并且对象Mark Word的锁标志位设置为“00”，即表示**此对象处于轻量级锁定状态**。

        4.如果这个更新操作失败了，虚拟机首先会检查对象的Mark Word**是否指向当前线程的栈帧**，如果是就说明当前线程**已经拥有了这个对象的锁**，那就可以直接进入同步块继续执行。**否则说明多个线程竞争锁**，轻量级锁**就要膨胀为重量级锁**，锁标志的状态值变为“**10**”，Mark Word中存储的就是指向重量级锁（互斥量）的指针，后面等待锁的线程**也要进入阻塞状态**。即若当前只有一个等待线程，则该线程通过**自旋进行等待**。但是当自旋**超过一定的次数**，或者一个线程在持有锁，一个在自旋，又有**第三个来访**时，轻量级锁**升级为重量级锁**。

    -   **2.轻量级锁的释放：**由**轻量锁切换到重量锁的过程**是发生在轻量锁**释放锁的时机**，之前在获取锁的时候它拷贝了锁对象头的markword，在释放锁的时候如果它发现在它持有锁的期间有其他线程来尝试获取锁了，并且**该线程对markword做了修改**，两者比对发现不一致，**则切换到重量锁**。

    

**悲观锁**：悲观锁是就是悲观思想，即**认为写多，遇到并发写的可能性高**，每次去拿数据的时候都认为别人会修改，所以每次在**读写数据的时候都会上锁**，这样别人想读写这个数据就会**block直到拿到锁**。java中的悲观锁就是**Synchronized **和 **Lock的实现类**。

-   **重量级锁**：依赖于**操作系统Mutex Lock所实现的锁**我们称之为“**重量级锁**”。当锁为轻量级锁的时候，另一个线程虽然是**自旋**，但自旋不会一直持续下去，**当自旋一定次数的时候，还没有获取到锁，就会进入阻塞**，该锁**膨胀为重量级锁**。重量级锁会让其他申请的线程**进入阻塞，性能降低**。synchronized属于重量级锁，在JDK1.5之前**使用synchronized关键字保证同步**。1.5以后**引入AQS**框架后的重量级锁则是**先尝试cas乐观锁去获取锁**，获取不到，才会转换为悲观锁，如**RetreentLock。**

    -   **1.synchronized的使用场景**：

        1.它可以把任意一个**非NULL的对象**当作锁；

        2.作用于方法时，锁住的是**对象的实例**(this)；

        3.当作用于**静态方法时，锁住的是Class实例**，又因为Class的相关数据存储在永久代PermGen（jdk1.8则是metaspace），永久代是全局共享的，因此静态方法锁相当于**类的一个全局锁**，会锁**所有调用该方法的线程**；

        4.synchronized作用于一个对象实例时，锁住的是所有以**该对象为锁的代码块**。

    -   **2.重量级锁的加锁过程**：

        1.检测Mark Word里面是不是**当前线程的ID**，如果是，表示当前线程**处于偏向锁**

        2.如果不是，则**使用CAS**将当前线程的ID替换Mard Word，如果成功则表示**当前线程获得偏向锁**，置偏向标志位1

        3.如果失败，则说明**发生竞争**，撤销偏向锁，进而**升级为轻量级锁**。

        4.当前线程使用CAS将对象头的Mark Word替换为锁记录指针，如果成功，当前线程获得锁

        5.如果失败，表示**其他线程竞争锁**，当前线程便**尝试使用自旋来获取锁**。

        6.如果自旋成功则依然处于轻量级状态。

        7.如果自旋失败，则**升级为重量级锁**。

        具体过程如下图所示：

        <img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200327161854920.png" alt="image-20200327161854920" style="zoom:40%;" />

        

**轻量级锁与重量级锁的优缺点对比**：

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200327145413763.png" alt="image-20200327145413763" style="zoom:45%;" />

​		综上，自旋锁**通过CAS操作**尝试获取锁；偏向锁通过**对比Mark Word解决加锁问题**，避免执行CAS操作；而轻量级锁是通过**用CAS操作和自旋来解决加锁问题**，避免线程阻塞和唤醒而影响性能；重量级锁则是将**除了拥有锁的线程以外的线程都阻塞**。

​		**偏向锁/轻量级锁/重量级锁**都是指锁的状态，并且是**针对synchronized**。在Java 5通过引入**锁升级**的机制来实现高效synchronized。这三种锁的状态是**通过对象监视器在对象头中的字段**来表明的。Java SE 1.6为了减少**获得锁和释放锁**带来的性能消耗，引入了**“偏向锁”和“轻量级锁”**，在Java SE 1.6中，锁一共有4种状态，级别从低到高依次是：**无锁状态、偏向锁状态、轻量级锁状态和重量级锁状态**，这几个状态会随着竞争情况逐渐升级。锁可以升级但**不能降级**，意味着偏向锁升级成轻量级锁后不能降级成偏向锁。这种锁升级却不能降级的策略，目的是为了**提高获得锁和释放锁的效率**。**重量级锁**是悲观锁的一种，**自旋锁、偏向锁与轻量级锁**属于**乐观锁**。



###### 2.公平锁/非公平锁

**公平锁**：是指多个线程按照**申请锁的顺序**来获取锁。线程直接进入队列中排队，队列中的**第一个线程才能获得锁**。

-   优点是等待锁的**线程不会饿死**。
-   缺点是整体**吞吐效率相对非公平锁要低**，等待队列中除第一个线程以外的**所有线程都会阻塞**，CPU唤醒阻塞线程的**开销比非公平锁大**。

**非公平锁**：是指多个线程获取锁的顺序**并不是按照申请锁的顺序**，有可能后申请的线程比先申请的线程**优先获取锁**。原理是多个线程加锁时**直接尝试获取锁**，获取不到才会到等待队列的队尾等待。但如果此时锁刚好可用，那么这个线程可以**无需阻塞直接获取到锁**，所以非公平锁有可能出现后申请锁的线程先获取锁的场景。

-   优点是可以**减少唤起线程的开销**，整体的吞吐**效率高**，因为线程有几率**不阻塞直接获得锁**，CPU不必唤醒所有线程。
-   有可能会造成**优先级反转**或者处于等待队列中的线程**可能会饿死**，或者等**很久才会获得锁**。

​		**ReentrantLock**可重入锁通过**构造函数指定该锁是否是公平锁**，默认是非公平锁。非公平锁的优点在于**吞吐量比公平锁大**。根据下图源代码对比，我们可以明显的看出公平锁与非公平锁的**lock()方法**唯一的区别就在于公平锁在**获取同步状态时多了一个限制条件**：hasQueuedPredecessors()。hasQueuedPredecessors()方法主要做一件事情：主要是判断当前线程**是否位于同步队列中的第一个**。如果是则返回true，否则返回false

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200327162747604.png" alt="image-20200327162747604" style="zoom:40%;" />

​		**synchronized**也是一种**非公平锁**。由于其并不像ReentrantLock是**通过AQS的来实现线程调度**，所以并没有任何办法使其变成公平锁。



###### 3.独享锁/共享锁  与  互斥锁/读写锁

**独享锁**：是指该锁一次只能被**一个线程所持有**；如果线程T对数据A加上**排它锁**后，则其他线程**不能**再对A加任何类型的锁。获得排它锁的线程**即能读数据又能修改数据**。JDK中的**synchronized**和JUC中**Lock**的实现类就是互斥锁。

**共享锁**：是指该锁可被**多个线程所持有**。如果线程T对数据A加上共享锁后，则其他线程**只能对A再加共享锁**，不能加**排它锁**。获得共享锁的线程**只能读数据**，不能修改数据。

**独享锁/共享锁** 是一种理论说法，独享锁与共享锁也是**通过AQS来实现**的，通过**实现不同的方法**，来实现独享或者共享。

**互斥锁/读写锁** 则是其具体的实现。互斥锁在Java中的具体实现就是**synchronized**与**ReentrantLock**；读写锁在Java中的具体实现就是**ReadWriteLock**。

-   **synchronized**或**ReentrantLock**是独享锁，为**同一个线程**所**持有**或**重复拥有**。
-   **ReadWriteLock**中的**读锁是共享锁**，**写锁是独享锁**。读锁的共享锁可保证**并发读是非常高效**的，**读写、写读 、写写**的过程是互斥的。读写锁的源代码与原理如下：
    -   **写锁**：tryAcquire()除了**重入条件**（当前线程为获取了写锁的线程）之外，增加了一个读锁是否存在的判断。如果存在读锁，则**写锁不能被获取**，原因在于：必须确保**写锁的操作对读锁可见**，如果允许读锁在已被获取的情况下对写锁的获取，那么正在运行的其他读线程**就无法感知到当前写线程的操作**。因此，只有等待其他读线程都释放了读锁，写锁才能被当前线程获取，而写锁一旦被获取，则其他**读写线程的后续访问均被阻塞**。写锁的释放与ReentrantLock的释放过程基本类似，**每次释放均减少写状态**，当写状态为0时表示**写锁已被释放**，然后等待的读写线程才能够**继续访问读写锁**，同时前次写线程的修改对后续的读写线程可见。
    -   **读锁**：在tryAcquireShared(int unused)方法中，如果其他**线程获取了写锁**，则当前线程**获取读锁失败**，进入**等待状态**。如果当前线程**已经获取了写锁或者写锁未被获取**，则当前线程（线程安全，依靠CAS保证）增加**读状态**，成功**获取读锁**。读锁的每次释放（线程安全的，可能有多个读线程同时释放读锁）**均减少读状态**，减少的值是“1<<16”。所以读写锁才能实现**读读的过程共享**，而读写、写读、写写的过程**互斥**。



###### 4.可重入锁与非重入锁

**可重入锁**：又名**递归锁**，是指同一个线程在**外层方法**获取锁的时候，在**进入内层方法会自动获取锁**。可重入锁的一个好处是可一定程度避免死锁。

-   **ReentrantLock **名字是Reentrant Lock即是**重新进入锁**。
-   **synchronized **也是一个可重入锁。
-   **Segment**：**分段锁**其实是一种**锁的设计**，并不是具体的一种锁，分段锁的设计目的是**细化锁的粒度**，当操作不需要更新整个数组的时候，就**仅仅针对数组中的一项**进行加锁操作,其属于一种可重入锁。

​		**ConcurrentHashMap**  并发的实现就是通过分段锁的形式来实现**高效的并发操作**，其分段锁称为**Segment**，它类似于HashMap（JDK7与JDK8中HashMap的实现）的结构，即内部拥有一个Entry数组，数组中的**每个元素又引出一个链表**；同时每个元素又是一个**ReentrantLock**（Segment继承了ReentrantLock)。当需要put元素的时候，并不是对整个HashMap进行加锁，而是**先通过hashcode来知道他要放在那一个分段中，然后对这个分段进行加锁**，所以当多线程put的时候，只要不是放在一个分段中，就实现了**真正的并行的插入**。但是，在统计size的时候，也就是**获取HashMap全局信息的时候**，就需要**获取所有的分段锁**才能统计。



**非重入锁**：同一线程不能自动获得锁，当前线程出现死锁，整个等待队列中的所有线程**都无法被唤醒**。我们通过重入锁**ReentrantLock**以及**非可重入锁NonReentrantLock**的源码来对比分析一下为什么非可重入锁在重复调用同步资源时会出现死锁。

-   首先ReentrantLock和NonReentrantLock都**继承父类AQS**，其父类AQS中维护了一个**同步状态status**来计数重入次数，status**初始值为0**。
-   当线程尝试获取锁时，可重入锁**先尝试获取并更新status值**，如果status == 0表示**没有其他线程在执行同步代码，则把status置为1**，当前线程开始执行。如果status != 0，则**判断当前线程是否是获取到这个锁的线程，如果是的话执行status+1**，且当前线程可以再次获取锁。而非可重入锁是**直接去获取并尝试更新当前status的值**，如果status != 0的话会导致其获取锁失败，当前线程阻塞。
-   释放锁时，可重入锁同样先获取当前status的值，在当前线程是持有锁的线程的前提下，如果status-1 == 0，则表示当前线程**所有重复获取锁的操作都已经执行完毕**，然后该线程才会**真正释放锁**。而非可重入锁则是在确定当前线程是持有锁的线程之后，**直接将status置为0，将锁释放**。

##### 

