### Java面试经典问题（From CSDN）

#### Java基础知识面试题

##### 1.基本概念与语法

###### java中的编译器和解释器：

​		Java中引入了虚拟机的概念，即在机器和编译程序之间加入了一层抽象的虚拟机器。这台虚拟的机器在任何平台上都提供给编译程序一个的共同的接口。编译程序只需要面向虚拟机，生成虚拟机能够理解的代码，然后由解释器来将虚拟机代码转换为特定系统的机器码执行。在Java中，这种供虚拟机理解的代码叫做**字节码**（即扩展为.class的文件），它不面向任何特定的处理器，只**面向虚拟机**。每一种平台的解释器是不同的，但是实现的虚拟机是相同的。Java源程序经过编译器编译后变成字节码，字节码由虚拟机解释执行，虚拟机将每一条要执行的字节码送给解释器，解释器将其翻译成特定机器上的机器码，然后在特定的机器上运行，这就是上面提到的Java的特点的编译与解释并存的解释。

```
Java源代码---->编译器---->jvm可执行的Java字节码(即虚拟指令)---->jvm---->jvm中解释器----->机器可执行的二进制机器码---->程序运行。
```

###### switch 是否能作用在 byte 上，是否能作用在 long 上，是否能作用在 String 上

​		在 Java 5 以前，switch(expr)中，expr 只能是 byte、short、char、int。从 Java5 开始，Java 中引入了枚举类型，expr 也可以是 enum 类型，从 Java 7 开始，expr 还可以是字符串（String），但是长整型（long）在目前所有的版本中都是不可以的。

###### Java 有没有 goto语句

​		goto 是 Java 中的保留字，在目前版本的 Java 中没有使用。

###### final 有什么用？

​		用于修饰类、属性和方法；

- 被final修饰的类不可以被继承
- 被final修饰的方法不可以被重写
- 被final修饰的变量不可以被改变，被final修饰不可变的是变量的引用，而不是引用指向的内容，引用指向的内容是可以改变的

###### this关键字的用法

​		this是自身的一个对象，代表对象本身，可以理解为：指向对象本身的一个指针。

​		this的用法在java中大体可以分为3种：

-   1.普通的直接引用，this相当于是指向当前对象本身。

-   2.形参与成员名字重名，用this来区分：
-   3.引用本类的构造函数

###### super关键字的用法

​		super可以理解为是指向自己超（父）类对象的一个指针，而这个超类指的是离自己最近的一个父类。

​		super也有三种用法：

-   1.普通的直接引用

    与this类似，super相当于是指向当前对象的父类的引用，这样就可以用super.xxx来引用父类的成员。

-   2.子类中的成员变量或方法与父类中的成员变量或方法同名时，用super进行区分

-   3.引用父类构造函数

    -   super（参数）：调用父类中的某一个构造函数（应该为构造函数中的第一条语句）。

    - this（参数）：调用本类中另一种形式的构造函数（应该为构造函数中的第一条语句）。


###### this与super的区别

- super:　它引用当前对象的直接父类中的成员（用来访问直接父类中被隐藏的父类中成员数据或函数，基类与派生类中有相同成员定义时如：super.变量名 super.成员函数据名（实参）
- this：它代表当前对象名（在程序中易产生二义性之处，应使用this来指明当前对象；如果函数的形参与类中的成员数据同名，这时需用this来指明成员变量名）
- super()和this()类似,区别是，super()在子类中**调用父类的构造方法**，this()在本类内**调用本类的其它构造方法**。
- super()和this()均需放在构造方法内第一行。
- 尽管可以用this调用一个构造器，但却不能调用两个。
- this和super**不能同时出现在一个构造函数里面**，因为this必然会调用其它的构造函数，其它的构造函数必然也会有super语句的存在，所以在同一个构造函数里面有相同的语句，就失去了语句的意义，编译器也不会通过。
- this()和super()都指的是对象，所以，**均不可以在static环境中使用**。包括：static变量,static方法，static语句块。
- 从本质上讲，this是一个指向本对象的指针, 然而super是一个Java关键字。

###### 在 Java 中，如何跳出当前的多重嵌套循环

​		在Java中，要想跳出多重循环，可以在外面的循环语句前定义一个标号，然后在里层循环体的代码中使用带有标号的break 语句，即可跳出外层循环。



##### 2.面向对象

###### **关于继承如下 3 点请记住**：

1. 子类拥有父类非 private 的属性和方法。
2. 子类可以拥有自己属性和方法，即子类可以对父类进行扩展（重载）。
3. 子类可以用自己的方式实现父类的方法（覆写）。

###### 多态的实现

​		Java实现多态有三个必要条件：继承、重写、向上转型。

-   继承：在多态中必须存在有继承关系的子类和父类。

-   重写：子类对父类中某些方法进行重新定义，在调用这些方法时就会调用子类的方法。

-   向上转型：在多态中需要将子类的引用赋给父类对象，只有这样该引用才能够具备技能调用父类的方法和子类的方法。


​        只有满足了上述三个条件，我们才能够在同一个继承结构中使用统一的逻辑实现代码处理不同的对象，从而达到执行不同的行为。

​		对于Java而言，它多态的实现机制遵循一个原则：当超类对象引用变量引用子类对象时，被引用对象的类型而不是引用变量的类型决定了调用谁的成员方法，但是这个被调用的方法必须是在超类中定义过的，也就是说被子类覆盖的方法。

###### 构造方法有哪些特性？

-   名字与类名相同；

-   没有返回值，但不能用void声明构造函数；

-   生成类的对象时自动执行，无需调用。


###### 静态方法和实例方法有何不同？

​		静态方法和实例方法的区别主要体现在两个方面：

- 在外部调用静态方法时，可以使用"类名.方法名"的方式，也可以使用"对象名.方法名"的方式。而实例方法只有后面这种方式。也就是说，调用静态方法可以无需创建对象。
- 静态方法在访问本类的成员时，只允许访问静态成员（即静态成员变量和静态方法），而不允许访问实例成员变量和实例方法；实例方法则无此限制

###### 内部类的优点

​		我们为什么要使用内部类呢？因为它有以下优点：

- 一个内部类对象可以访问创建它的外部类对象的内容，包括私有数据！
- 内部类不为同一包的其他类所见，具有很好的封装性；
- 内部类有效实现了“多重继承”，优化 java 单继承的缺陷。
- 匿名内部类可以很方便的定义回调。

###### 内部类有哪些应用场景

- 一些多算法场合
- 解决一些非面向对象的语句块。
- 适当使用内部类，使得代码更加灵活和富有扩展性。
- 当某个类除了它的外部类，不再被其他的类使用时。

###### hashCode()与equals()的相关规定

-   如果两个对象相等，则hashcode一定也是相同的

-   两个对象相等，对两个对象分别调用equals方法都返回true

-   两个对象有相同的hashcode值，它们也不一定是相等的

-   因此，equals 方法被覆盖过，则 hashCode 方法也必须被覆盖

-   hashCode() 的默认行为是对堆上的对象产生独特值。如果没有重写 hashCode()，则该 class 的两个对象无论如何都不会相等（即使这两个对象指向相同的数据）




##### 3.Java包与IO，反射，字符串

###### import java和javax有什么区别

​		刚开始的时候 JavaAPI 所必需的包是 java 开头的包，javax 当时只是扩展 API 包来说使用。然而随着时间的推移，javax 逐渐的扩展成为 Java API 的组成部分。但是，将扩展从 javax 包移动到 java 包将是太麻烦了，最终会破坏一堆现有的代码。因此，最终决定 javax 包将成为标准API的一部分。

​		所以，实际上java和javax没有区别。这都是一个名字。

###### java 中 IO 流分为几种?

-   按照流的流向分，可以分为输入流和输出流；
-   按照操作单元划分，可以划分为字节流和字符流；
-   按照流的角色划分为节点流和处理流。
-   Java Io流共涉及40多个类，这些类看上去很杂乱，但实际上很有规则，而且彼此之间存在非常紧密的联系， Java I0流的40多个类都是从如下4个抽象类基类中派生出来的。
-   InputStream/Reader: 所有的输入流的基类，前者是字节输入流，后者是字符输入流。
-   OutputStream/Writer: 所有输出流的基类，前者是字节输出流，后者是字符输出流。

###### BIO,NIO,AIO 有什么区别?

​		简答

-   BIO：Block IO 同步阻塞式 IO，就是我们平常使用的传统 IO，它的特点是模式简单使用方便，并发处理能力低。
-   NIO：Non IO 同步非阻塞 IO，是传统 IO 的升级，客户端和服务器端通过 Channel（通道）通讯，实现了多路复用。
-   AIO：Asynchronous IO 是 NIO 的升级，也叫 NIO2，实现了异步非堵塞 IO ，异步 IO 的操作基于事件和回调机制。

###### Java获取反射的三种方法

-   1.通过new对象实现反射机制 
-   2.通过路径实现反射机制 
-   3.通过类名实现反射机制

###### String有哪些特性

-   不变性：String 是只读字符串，是一个典型的 immutable 对象，对它进行任何操作，其实都是创建一个新的对象，再把引用指向该对象。不变模式的主要作用在于当一个对象需要被多线程共享并频繁访问时，可以保证数据的一致性。

-   常量池优化：String 对象创建之后，会在字符串常量池中进行缓存，如果下次创建同样的对象时，会直接返回缓存的引用。

-   final：使用 final 来定义 String 类，表示 String 类不能被继承，提高了系统的安全性。



###### 如何将字符串反转？

​		使用 StringBuilder 或者 stringBuffer 的 reverse() 方法。

###### 数组有没有 length()方法？String 有没有 length()方法

​		数组没有 length()方法 ，有 length 的属性。String 有 length()方法。JavaScript中，获得字符串的长度是通过 length 属性得到的，这一点容易和 Java 混淆。

###### 在使用 HashMap 的时候，用 String 做 key 有什么好处？

​		HashMap 内部实现是通过 key 的 hashcode 来确定 value 的存储位置，因为字符串是不可变的，所以当创建字符串时，它的 hashcode 被缓存下来，不需要再次计算，所以相比于其他对象更快。







#### Java集合容器面试题

##### 1.集合容器概述

###### **集合框架概念**

​	集合框架是为表示和操作集合而规定的一种**统一的标准**的体系结构，是用于存储数据的容器。

​	任何集合框架都包含三大块内容：对外的接口、接口的实现和对集合运算的算法。

​	**接口**：表示集合的抽象数据类型。接口允许我们操作集合时不必关注具体实现，从而达到“多态”。在面向对象编程语言中，接口通常用来形成规范。

​	**实现**：集合接口的具体实现，是重用性很高的数据结构。

​	**算法**：在一个实现了某个集合框架中的接口的对象身上完成某种有用的计算的方法，例如查找、排序等。这些算法通常是多态的，因为相同的方法可以在同一个接口被多个类实现时有不同的表现。事实上，算法是可复用的函数。它减少了程序设计的辛劳。



###### 集合和数组的区别

- 数组是固定长度的；集合可变长度的。
- 数组可以存储基本数据类型，也可以存储引用数据类型；集合只能存储引用数据类型。
- 数组存储的元素必须是同一个数据类型；集合存储的对象可以是不同数据类型。



###### 常用的集合类

**Map接口**和**Collection接口**是所有集合框架的父接口：

![image-20200313110241171](参考图片/image-20200313110241171.png)

**Map接口**，一个键值对集合，存储键、值和之间的映射。 Key无序，唯一；value 不要求有序，允许重复。Map没有继承于Collection接口，从Map集合中检索元素时，只要给出键对象，就会返回对应的值对象。实现类主要有：HashMap、TreeMap、Hashtable、**ConcurrentHashMap**以及Properties等

Collection接口的子接口包括：**Set接口和List接口**

- Set接口，一个**无序**（存入和取出顺序有可能不一致）容器，不可以存储重复元素，只允许存入一个null元素，必须保证元素唯一性。实现类主要有：HashSet、TreeSet、LinkedHashSet等
- List接口，一个**有序**（元素存入集合的顺序和取出的顺序一致）容器，元素可以重复，可以插入多个null元素，元素都有索引。实现类主要有：ArrayList、LinkedList、Stack以及Vector等



###### 怎么确保一个集合不能被修改？

​		可以使用 Collections. unmodifiableCollection(Collection c) 方法来创建一个只读集合，这样改变集合的任何操作都会抛出 Java. lang.UnsupportedOperationException 异常。



##### 2.Collection接口

###### 1.List接口

**迭代器 Iterator 是什么？**

​		Iterator 接口提供遍历任何 Collection 的接口。我们可以从一个 Collection 中使用迭代器方法来获取迭代器实例。迭代器取代了 Java 集合框架中的 Enumeration，迭代器允许调用者在迭代过程中移除元素。

**Iterator 怎么使用？有什么特点？**

​		Iterator 使用代码如下：

```
List<String> list = new ArrayList<>();
Iterator<String> it = list. iterator();
while(it. hasNext()){
  String obj = it. next();
  System. out. println(obj);
}
```

​		Iterator 的特点是只能单向遍历，但是更加安全，因为它可以确保，在当前遍历的集合元素被更改的时候，就会抛出 ConcurrentModificationException 异常。

**如何边遍历边移除 Collection 中的元素？**

​		边遍历边修改 Collection 的唯一正确方式是使用 Iterator.remove() 方法，如下：

```
Iterator<Integer> it = list.iterator();
while(it.hasNext()){
   *// do something*
   it.remove();
}
```


​		一种最常见的错误代码如下：

```
for(Integer i : list){
   list.remove(i)
}
```

**Iterator 和 ListIterator 有什么区别？**

- Iterator 可以遍历 Set 和 List 集合，而 ListIterator 只能遍历 List。
- Iterator 只能单向遍历，而 ListIterator 可以双向遍历（向前/后遍历）。
- ListIterator 实现 Iterator 接口，然后添加了一些额外的功能，比如添加一个元素、替换一个元素、获取前面或后面元素的索引位置。



###### 集合访问方法

**Java Collections 框架中提供了一个 RandomAccess 接口，用来标记 List 实现是否支持 Random Access。**

-   如果一个数据集合实现了该接口，就意味着它支持 Random Access，按位置读取元素的平均时间复杂度为 O(1)，如ArrayList。如果没有实现该接口，表示不支持 Random Access，如LinkedList。
-   推荐的做法就是，**支持 Random Access 的列表可用 for 循环遍历**，**否则建议用 Iterator 或 foreach 遍历。**



###### 如何实现数组和 List 之间的转换？

- 数组转 List：使用 Arrays. asList(array) 进行转换。
- List 转数组：使用 List 自带的 toArray() 方法。

```Java
// list to array
List<String> list = new ArrayList<String>();
list.add("123");
list.add("456");
list.toArray();

// array to list
String[] array = new String[]{"123","456"};
Arrays.asList(array);
```



###### ArrayList 和 Vector (过时)的区别是什么？

​		这两个类都实现了 **List 接口**（List 接口继承了 Collection 接口），他们都是**有序集合**

-   线程安全：Vector 使用了 **Synchronized** 来实现线程同步，是线程安全的，而 ArrayList 是非线程安全的。
-   性能：ArrayList 在**性能方面要优于 Vector。**
-   扩容：ArrayList 和 Vector **都会根据实际的需要动态的调整容量**，只不过在 Vector 扩容每次会增加 1 倍，而 ArrayList 只会增加 50%。Vector类的所有方法都是同步的。可以由两个线程安全地访问一个Vector对象、但是一个线程访问Vector的话代码要在同步操作上耗费大量的时间。Arraylist和linkedlist**都不是同步的**，所以在不需要保证线程安全时时建议使用Arraylist。



###### 多线程场景下如何使用 ArrayList？

​		ArrayList 不是线程安全的，如果遇到多线程场景，可以通过 Collections 的 synchronizedList 方法将其转换成线程安全的容器后再使用。例如像下面这样：

```Java
List<String> synchronizedList = Collections.synchronizedList(list);
synchronizedList.add("aaa");
synchronizedList.add("bbb");

for (int i = 0; i < synchronizedList.size(); i++) {
    System.out.println(synchronizedList.get(i));
}
```



###### 2.Set接口

###### List 和 Set 的区别

​		List , Set 都是继承自Collection 接口

-   List 特点：一个有序（元素存入集合的顺序和取出的顺序一致）容器，元素可以重复，可以插入多个null元素，元素都有索引。常用的实现类有 ArrayList、LinkedList 和 Vector。

-   Set 特点：一个无序（存入和取出顺序有可能不一致）容器，不可以存储重复元素，只允许存入一个null元素，必须保证元素唯一性。Set 接口常用实现类是 HashSet、LinkedHashSet 以及 TreeSet。


​        Set和List对比

-  List 支持for循环，也就是通过下标来遍历，也可以用迭代器，但是set只能用迭代，因为他无序，无法用下标来取得想要的值。
- Set：检索元素效率低下，删除和插入效率高，插入和删除不会引起元素位置改变。
- List：和数组类似，List可以动态增长，查找元素效率高，插入删除元素效率低，因为会引起其他元素位置改变



###### 说一下 HashSet 的实现原理？

​		HashSet 是**基于 HashMap 实现**的，HashSet的值存放于HashMap的key上，HashMap的value统一为PRESENT，因此 HashSet 的实现比较简单，相关 HashSet 的操作，基本上都是**直接调用底层 HashMap** 的相关方法来完成，**HashSet 不允许重复的值**。



###### HashSet如何检查重复？HashSet是如何保证数据不可重复的？

-   向HashSet 中add ()元素时，判断元素是否存在的依据，**不仅要比较hash值**，同时还要**结合equles 方法**比较。
-   HashSet 中的add ()方法会使用HashMap **的put()方法**。
-   HashMap 的 key 是唯一的，由源码可以看出 HashSet 添加进去的值就是作为HashMap 的key，并且在HashMap中如果K/V相同时，**会用新的V覆盖掉旧的V，然后返回旧的V**。所以不会重复（ **HashMap 比较key是否相等是先比较hashcode 再比较equals** ）。


```Java
private static final Object PRESENT = new Object();
private transient HashMap<E,Object> map;

public HashSet() {
    map = new HashMap<>();
}

public boolean add(E e) {
    // 调用HashMap的put方法,PRESENT是一个至始至终都相同的虚值
	return map.put(e, PRESENT)==null;
}
```

**hashCode（）与equals（）的相关规定：**

- 如果两个对象相等，则hashcode一定也是相同的
- 两个对象相等,对两个equals方法返回true
- 两个对象有相同的hashcode值，它们也不一定是相等的
- 综上，equals方法被覆盖过，则hashCode方法也必须被覆盖
- hashCode()的默认行为是对堆上的对象产生独特值。如果没有重写hashCode()，则该class的两个对象无论如何都不会相等（即使这两个对象指向相同的数据）。

**==与equals的区别**

- ==判断两个变量或实例是不是指向同一个内存空间; equals判断两个变量所指向的内存空间的值是不是相同;
- ==是指对内存地址进行比较 equals()是对字符串的内容进行比较



###### HashSet与HashMap的区别

![image-20200313122658787](参考图片/image-20200313122658787.png)



###### 3.Queue

###### BlockingQueue是什么？

​		Java.util.concurrent.BlockingQueue是一个队列，在进行检索或移除一个元素的时候，它会等待队列变为非空；当在添加一个元素时，它会等待队列中的可用空间。BlockingQueue接口是Java集合框架的一部分，主要用于实现生产者-消费者模式。我们不需要担心等待生产者有可用的空间，或消费者有可用的对象，因为它都在BlockingQueue的实现类中被处理了。Java提供了集中BlockingQueue的实现，比如ArrayBlockingQueue、LinkedBlockingQueue、PriorityBlockingQueue,、SynchronousQueue等。

###### 在 Queue 中 poll()和 remove()有什么区别？

- 相同点：都是返回第一个元素，并在队列中删除返回的对象。
- 不同点：如果没有元素 poll()会返回 null，而 remove()会直接抛出 NoSuchElementException 异常。



##### 3.Map接口

###### HashMap 的实现原理？

**HashMap概述**： HashMap是基于哈希表的**Map接口的非同步实现**。此实现提供所有可选的映射操作，并**允许使用null值和null键**。此类不保证映射的顺序，特别是它不保证该顺序恒久不变。

**HashMap的数据结构**： 在Java编程语言中，最基本的结构就是两种，一个是**数组**，另外一个是**模拟指针**（引用），所有的数据结构都可以用这两个基本结构来构造的，HashMap也不例外。HashMap实际上是一个“**链表散列**”的数据结构，即数组和链表的结合体。

HashMap 基于 Hash 算法实现的：

- 当我们往Hashmap中**put元素**时，利用key的**hashCode重新hash**计算出当前对象的元素在**数组中的下标**
- 存储时，如果出现hash值**相同的key**，此时有两种情况。(1)如果key相同，则覆盖原始key的value值；(2)如果key不同（**出现冲突**），则将当前的key-value**放入链表中**（拉链法）
- 获取时，**get方法**，直接找到**hash值对应的下标**，在进一步**判断key是否相同**，从而找到对应值。
- 理解了以上过程就不难明白HashMap是如何解决hash冲突的问题，核心就是**使用了数组的存储方式**，然后将**冲突的key的对象放入链表中**，一旦发现冲突就在链表中做进一步的对比。

需要注意Jdk 1.8中对HashMap的实现做了优化，当链表中的节点数据超过八个之后，该链表会**转为红黑树**来提高查询效率，从原来的O(n)到O(logn)
![image-20200313123307280](参考图片/image-20200313123307280.png)



###### HashMap的put方法的具体流程？

​		当我们put的时候，首先计算 **key的hash值**，这里调用了 **hash方法**，hash方法实际是让**key.hashCode()**与key.hashCode()>>>16进行**异或操作**，高16bit补0，**一个数和0异或不变**，所以 hash 函数大概的作用就是：高16bit不变，低16bit和高16bit做了一个异或，目的是减少碰撞。按照函数注释，因为bucket数组大小是2的幂，计算下标index = (table.length - 1) & hash，如果不做 hash 处理，相当于散列生效的只有几个低 bit 位，为了减少散列的碰撞，设计者综合考虑了速度、作用、质量之后，使用**高16bit和低16bit异或来简单处理减少碰撞**，而且JDK8中用了复杂度 O（logn）的树结构来提升碰撞下的性能。

**hash()函数**

​		如果使用hashCode取余，那么相当于参与运算的只有hashCode的低位，高位是没有起到任何作用的，所以我们的思路就是让hashCode取值出的高位也参与运算，进一步降低hash碰撞的概率，使得数据分布更平均，我们把这样的操作称为扰动，在JDK 1.8中的hash()函数如下：

```Java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);// 与自己右移16位进行异或运算（高低位异或）
}
```

这比在JDK 1.7中，更为简洁，相比在1.7中的4次位运算，5次异或运算（9次扰动），在1.8中，只进行了1次位运算和1次异或运算（2次扰动）；



###### HashMap的扩容操作是怎么实现的？

①.在jdk1.8中，resize方法是在hashmap中的键值对大于阀值时或者初始化时，就调用resize方法进行扩容；

②.每次扩展的时候，都是扩展2倍；

③.扩展后Node对象的位置要么在原位置，要么移动到原偏移量两倍的位置。



###### **为什么HashMap中String、Integer这样的包装类适合作为K？**

答：String、Integer等包装类的特性能够保证Hash值的**不可更改性和计算准确性**，能够有效的减少Hash碰撞的几率

- 都是final类型，即不可变性，保证key的不可更改性，不会存在获取hash值不同的情况
- 内部已重写了equals()、hashCode()等方法，遵守了HashMap内部的规范（不清楚可以去上面看看putValue的过程），不容易出现Hash值计算错误的情况；



###### 如果使用Object作为HashMap的Key，应该怎么办呢？

答：重写hashCode()和equals()方法

-   重写hashCode()是因为需要计算存储数据的存储位置，需要注意不要试图从散列码计算中排除掉一个对象的关键部分来提高性能，这样虽然能更快但可能会导致更多的Hash碰撞；
-   重写equals()方法，需要遵守自反性、对称性、传递性、一致性以及对于任何非null的引用值x，x.equals(null)必须返回false的这几个特性，目的是为了保证key在哈希表中的唯一性；



###### **HashMap 与 HashTable 有什么区别？**

- 线程安全： HashMap 是非线程安全的，HashTable 是**线程安全**的；HashTable 内部的方法基本都经过 synchronized 修饰。（如果你要保证线程安全的话就使用 ConcurrentHashMap 吧！）；
- 效率： 因为线程安全的问题，HashMap 要比 HashTable 效率高一点。另外，HashTable 基本被淘汰，不要在代码中使用它；
- 对Null key 和Null value的支持： HashMap 中，**null 可以作为键，这样的键只有一个**，可以有一个或多个键所对应的值为 null。但是在 HashTable 中 put 进的键值只要有一个 null，直接抛NullPointerException。
- **初始容量大小和每次扩充容量大小的不同**： ①创建时如果不指定容量初始值，Hashtable 默认的初始大小为11，之后每次扩充，容量变为原来的2n+1。HashMap 默认的初始化大小为16。之后每次扩充，容量变为原来的2倍。②创建时如果给定了容量初始值，那么 Hashtable 会直接使用你给定的大小，而 HashMap 会将其扩充为2的幂次方大小。也就是说 HashMap 总是使用2的幂作为哈希表的大小，后面会介绍到为什么是2的幂次方。
- 底层数据结构： JDK1.8 以后的 HashMap 在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为8）时，将链表转化为红黑树，以减少搜索时间。Hashtable 没有这样的机制。
- 推荐使用：在 Hashtable 的类注释可以看到，Hashtable 是保留类不建议使用，推荐在单线程环境下使用 HashMap 替代，如果需要多线程使用则用 ConcurrentHashMap 替代。
  
    

###### HashMap 和 ConcurrentHashMap 的区别

- ConcurrentHashMap对整个桶数组进行了分割分段(Segment)，然后在每一个分段上都用lock锁进行保护，相对于HashTable的synchronized锁的粒度更精细了一些，并发性能更好，而HashMap没有锁机制，不是线程安全的。（JDK1.8之后ConcurrentHashMap启用了一种全新的方式实现,利用CAS算法。）
- HashMap的键值对允许有null，但是ConCurrentHashMap都不允许。



###### ConcurrentHashMap 底层具体实现知道吗？实现原理是什么？

**JDK1.7**

-   首先将数据分为一段一段的存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据时，其他段的数据也能被其他线程访问。在JDK1.7中，ConcurrentHashMap采用Segment + HashEntry的方式进行实现，结构如下：

-   一个 ConcurrentHashMap 里包含一个 Segment 数组。Segment 的结构和HashMap类似，是一种数组和链表结构，一个 Segment 包含一个 HashEntry 数组，每个 HashEntry 是一个链表结构的元素，每个 Segment 守护着一个HashEntry数组里的元素，当对 HashEntry 数组的数据进行修改时，必须首先获得对应的 Segment的锁。


**JDK1.8**

​		在**JDK1.8中，放弃了Segment臃肿的设计，取而代之的是采用Node + CAS + Synchronized来保证并发安全进行实现**，synchronized只锁定当前链表或红黑二叉树的首节点，这样只要hash不冲突，就不会产生并发，效率又提升N倍。





##### 4.辅助工具类

###### Array 和 ArrayList 有何区别？

- Array 可以存储**基本数据类型和对象**，ArrayList 只能存储**对象**。
- Array 是指定**固定大小**的，而 ArrayList 大小是**自动扩展**的。
- Array 内置方法没有 ArrayList 多，比如 addAll、removeAll、iteration 等方法只有 ArrayList 有。

对于基本类型数据，集合使用自动装箱来减少编码工作量。但是，当处理固定大小的基本数据类型的时候，这种方式相对比较慢。

###### 如何实现 Array 和 List 之间的转换？

- Array 转 List： Arrays. asList(array) ；
- List 转 Array：List 的 toArray() 方法。

###### Collection 和 Collections 有什么区别？

-   java.util.Collection 是一个**集合接口**（集合类的一个顶级接口）。它提供了对集合对象进行基本操作的**通用接口方法**。Collection接口在Java 类库中有**很多具体的实现**。Collection接口的意义是为各种具体的集合提供了最大化的统一操作方式，其直接**继承接口有List与Set**。
-   Collections则是集合类的一个**工具类/帮助类**，其中提供了一系列**静态方法**，用于对集合中元素进行**排序、搜索以及线程安全**等各种操作。



###### TreeMap 和 TreeSet 在排序时如何比较元素？

​		TreeSet 要求存放的对象所属的类必须**实现 Comparable 接口**，该接口提供了比较元素的 **compareTo()**方法，当插入元素时会**回调该方法比较元素的大小**。TreeMap 要求存放的键值对映射的键必须实现 Comparable 接口从而根据键对元素进行排序。



###### Collections 工具类中的 sort()方法如何比较元素？

​		Collections 工具类的 sort 方法有两种重载的形式，

- 第一种要求传入的待排序容器中存放的对象比较实现 Comparable 接口以实现元素的比较；
- 第二种不强制性的要求容器中的元素必须可比较，但是要求传入第二个参数，参数是Comparator 接口的子类型（需要重写 compare 方法实现元素的比较），相当于一个临时定义的排序规则，其实就是通过接口注入比较元素大小的算法，也是对回调模式的应用（Java 中对函数式编程的支持）。





#### Java异常处理面试题

##### 1.异常架构与关键字

###### 异常架构

![image-20200313130643585](参考图片/image-20200313130643585.png)

1. **Throwable**

Throwable 是 Java 语言中所有错误与异常的超类。

Throwable 包含两个子类：Error（错误）和 Exception（异常），它们通常用于指示发生了异常情况。

Throwable 包含了其线程创建时线程执行堆栈的快照，它提供了 printStackTrace() 等接口用于获取堆栈跟踪数据等信息。

2. **Error（错误）**

定义：Error 类及其子类。程序中无法处理的错误，表示运行应用程序中出现了严重的错误。

特点：此类错误一般表示代码运行时 JVM 出现问题。通常有 Virtual MachineError（虚拟机运行错误）、NoClassDefFoundError（类定义错误）等。比如 OutOfMemoryError：内存不足错误；StackOverflowError：栈溢出错误。此类错误发生时，JVM 将终止线程。

这些错误是不受检异常，非代码性错误。因此，当此类错误发生时，应用程序不应该去处理此类错误。按照Java惯例，我们是不应该实现任何新的Error子类的！

3. **Exception（异常）**

程序本身可以捕获并且可以处理的异常。Exception 这种异常又分为两类：运行时异常和编译时异常。

**运行时异常**

-   定义：RuntimeException 类及其子类，表示 JVM 在运行期间可能出现的异常。

-   特点：Java 编译器不会检查它。也就是说，当程序中可能出现这类异常时，倘若既"没有通过throws声明抛出它"，也"没有用try-catch语句捕获它"，还是会编译通过。比如NullPointerException空指针异常、ArrayIndexOutBoundException数组下标越界异常、ClassCastException类型转换异常、ArithmeticExecption算术异常。此类异常属于不受检异常，一般是由程序逻辑错误引起的，在程序中可以选择捕获处理，也可以不处理。虽然 Java 编译器不会检查运行时异常，但是我们也可以通过 throws 进行声明抛出，也可以通过 try-catch 对它进行捕获处理。如果产生运行时异常，则需要通过修改代码来进行避免。例如，若会发生除数为零的情况，则需要通过代码避免该情况的发生！

-   RuntimeException 异常会由 Java 虚拟机自动抛出并自动捕获（就算我们没写异常捕获语句运行时也会抛出错误！！），此类异常的出现绝大数情况是代码本身有问题应该从逻辑上去解决并改进代码。


**编译时异常**

-   定义: Exception 中除 RuntimeException 及其子类之外的异常。

-   特点: Java 编译器会检查它。如果程序中出现此类异常，比如 ClassNotFoundException（没有找到指定的类异常），IOException（IO流异常），要么通过throws进行声明抛出，要么通过try-catch进行捕获处理，否则不能通过编译。在程序中，通常不会自定义该类异常，而是直接使用系统提供的异常类。该异常我们必须手动在代码里添加捕获语句来处理该异常。


4. **受检异常与非受检异常**

Java 的所有异常可以分为受检异常（checked exception）和非受检异常（unchecked exception）。

**受检异常**

编译器要求必须处理的异常。正确的程序在运行过程中，经常容易出现的、符合预期的异常情况。一旦发生此类异常，就必须采用某种方式进行处理。除 RuntimeException 及其子类外，其他的 Exception 异常都属于受检异常。编译器会检查此类异常，也就是说当编译器检查到应用中的某处可能会此类异常时，将会提示你处理本异常——要么使用try-catch捕获，要么使用方法签名中用 throws 关键字抛出，否则编译不通过。

**非受检异常**

编译器不会进行检查并且不要求必须处理的异常，也就说当程序中出现此类异常时，即使我们没有try-catch捕获它，也没有使用throws抛出该异常，编译也会正常通过。该类异常包括运行时异常（RuntimeException极其子类）和错误（Error）。



###### Java异常关键字

• try – 用于监听。将要被监听的代码(可能抛出异常的代码)放在try语句块之内，当try语句块内发生异常时，异常就被抛出。
• catch – 用于捕获异常。catch用来捕获try语句块中发生的异常。
• finally – finally语句块总是会被执行。它主要用于回收在try块里打开的物力资源(如数据库连接、网络连接和磁盘文件)。只有finally块，执行完成之后，才会回来执行try或者catch块中的return或者throw语句，如果finally中使用了return或者throw等终止方法的语句，则就不会跳回执行，直接停止。
• throw – 用于抛出异常。
• throws – 用在方法签名中，用于声明该方法可能抛出的异常。





##### 2.Java异常处理

![image-20200313130822175](参考图片/image-20200313130822175.png)

###### **声明异常**

​		通常，应该捕获那些知道如何处理的异常，将不知道如何处理的异常继续传递下去。传递异常可以在方法签名处使用 throws 关键字声明可能会抛出的异常。注意：

- 非检查异常（Error、RuntimeException 或它们的子类）**不可使用** throws 关键字来声明要抛出的异常。
- 一个方法出现编译时异常，就需要 **try-catch/ throws** 处理，否则会导致**编译错误**。

###### **抛出异常**

​		如果你觉得解决不了某些异常问题，且**不需要调用者处理**，那么你可以抛出异常。

​		throw关键字作用是在方法内部抛出一个Throwable类型的异常。任何Java代码都可以通过throw语句抛出异常。

###### **捕获异常**

​		程序通常在运行之前不报错，但是运行后可能会出现某些未知的错误，但是还不想直接抛出到上一级，那么就需要通过try…catch…的形式进行异常捕获，之后根据不同的异常情况来进行相应的处理。

###### try-with-resource

​		上面例子中，finally 中的 close 方法也可能抛出 IOException, 从而覆盖了原始异常。JAVA 7 提供了更优雅的方式来实现资源的自动释放，自动释放的资源需要是**实现了 AutoCloseable 接口的类**。

```Java
private  static void tryWithResourceTest(){
    try (Scanner scanner = new Scanner(new FileInputStream("c:/abc"),"UTF-8")){
        // code
    } catch (IOException e){
        // handle exception
    }
}
```

​		try 代码块退出时，会自动调用 **scanner.close** 方法，和把 scanner.close 方法放在 finally 代码块中不同的是，若 scanner.close 抛出异常，则会被抑制，**抛出的仍然为原始异常**。被抑制的异常会由 addSusppressed 方法添加到原来的异常，如果想要获取被抑制的异常列表，可以调用 getSuppressed 方法来获取。



##### 3.Java异常常见面试题

###### JVM 是如何处理异常的？

​		在一个方法中如果发生异常，这个方法会创建一个异常对象，并**转交给 JVM**，该异常对象包含**异常名称**，异常描述以及异常发生时应用程序的状态。创建异常对象并转交给 JVM 的过程称为抛出异常。可能有一系列的方法调用，最终才进入抛出异常的方法，这一系列方法调用的有序列表叫做**调用栈**。

​		JVM 会顺着调用栈去查找看是否有可以处理异常的代码，如果有，则调用异常处理代码。当 JVM 发现可以处理异常的代码时，会把发生的异常传递给它。如果 JVM 没有找到可以处理该异常的代码块，JVM 就会将该异常转交给默认的**异常处理器**（默认处理器为 JVM 的一部分），默认异常处理器打印出异常信息并终止应用程序。



###### NoClassDefFoundError 和 ClassNotFoundException 区别？

-   NoClassDefFoundError 是一个 Error 类型的异常，是由 JVM 引起的，不应该尝试捕获这个异常。

-   引起该异常的原因是 JVM 或 ClassLoader 尝试加载某类时在内存中找不到该类的定义，该动作发生在运行期间，即编译时该类存在，但是在运行时却找不到了，可能是变异后被删除了等原因导致；

-   ClassNotFoundException 是一个受查异常，需要显式地使用 try-catch 对其进行捕获和处理，或在方法签名中用 throws 关键字进行声明。当使用 Class.forName, ClassLoader.loadClass 或 ClassLoader.findSystemClass 动态加载类到内存的时候，通过传入的类路径参数没有找到该类，就会抛出该异常；另一种抛出该异常的可能原因是某个类已经由一个类加载器加载至内存中，另一个加载器又尝试去加载它。



##### 4.Java异常处理最佳实践

###### 优先明确的异常

-   你抛出的异常越明确越好，永远记住，你的同事或者几个月之后的你，将会调用你的方法并且处理异常。

-   因此需要保证提供给他们尽可能多的信息。这样你的 API 更容易被理解。你的方法的调用者能够更好的处理异常并且避免额外的检查。因此，总是尝试寻找最适合你的异常事件的类，例如，抛出一个 NumberFormatException 来替换一个 IllegalArgumentException 。避免抛出一个不明确的异常。



###### 使用描述性消息抛出异常

-   在抛出异常时，需要尽可能精确地描述问题和相关信息，这样无论是打印到日志中还是在监控工具中，都能够更容易被人阅读，从而可以更好地定位具体错误信息、错误的严重程度等。




###### 不要捕获 Throwable 类

-   Throwable 是所有异常和错误的超类。你可以在 catch 子句中使用它，但是你永远不应该这样做！

-   如果在 catch 子句中使用 Throwable ，它不仅会捕获所有异常，也将捕获所有的错误。JVM 抛出错误，指出不应该由应用程序处理的严重问题。 典型的例子是 OutOfMemoryError 或者 StackOverflowError 。两者都是由应用程序控制之外的情况引起的，无法处理。



###### 不要忽略异常

-   很多时候，开发者很有自信不会抛出异常，因此写了一个catch块，但是没有做任何处理或者记录日志。
    但现实是经常会出现无法预料的异常，或者无法确定这里的代码未来是不是会改动(删除了阻止异常抛出的代码)，而此时由于异常被捕获，使得无法拿到足够的错误信息来定位问题。

-   合理的做法是至少要记录异常的信息。


```Java
public void logAnException() {
    try {
        // do something
    } catch (NumberFormatException e) {
        log.error("This should never happen: " + e);
    }
}
```





#### Java虚拟机面试题

##### 1.Java内存区域

JVM包含**两个子系统和两个组件**，两个子系统为**Class loader(类装载)**、**Execution engine(执行引擎)**；两个组件为**Runtime data area(运行时数据区)**、**Native Interface(本地接口)**。

**Class loader(类装载)**：根据给定的**全限定名类名**(如：java.lang.Object)来装载class文件到Runtime data area中的method area。

**Execution engine（执行引擎）**：执行classes中的指令。

**Native Interface(本地接口)**：与native libraries交互，是**其它编程语言交互的接口**。

**Runtime data area(运行时数据区域)**：这就是我们常说的**JVM的内存**。

**作用** ：首先通过类加载器（ClassLoader）会把 **Java  class代码转换成字节码**，运行时数据区（Runtime Data Area）再**把字节码加载到内存中**，而字节码文件只是 JVM 的**一套指令集规范**，并不能直接交给底层操作系统去执行，因此需要特定的命**令解析器执行引擎**（Execution Engine），将字节码**翻译成底层系统指令**，再交由 CPU 去执行，而这个过程中需要调用**其他语言的本地库接口**（Native Interface）来实现整个程序的功能。



###### JVM 运行时数据区

**Java 虚拟机在执行 Java 程序的过程中会把它所管理的内存区域划分为若干个不同的数据区域**。这些区域都有各自的用途，以及创建和销毁的时间，有些区域随着虚拟机进程的启动而存在，有些区域则是依赖线程的启动和结束而建立和销毁。不同虚拟机的运行时数据区可能略微有所不同，但都会遵从 Java 虚拟机规范， Java 虚拟机规范规定的区域分为以下 5 个部分：

**线程独立所有：**

​	**程序计数器**（Program Counter Register）：当前线程所执行的字节码的行号指示器，字节码解析器的工作是通过改变这个计数器的值，来选取下一条需要执行的字节码**指令，分支、循环、跳转、异常处理、线程恢复**等基础功能，都需要依赖这个计数器来完成；

​	**Java 虚拟机栈**（Java Virtual Machine Stacks）：用于存储**局部变量表**、**操作数栈**、**动态链接**、**方法出口**等信息；
​	**本地方法栈**（Native Method Stack）：与虚拟机栈的作用是一样的，只不过虚拟机栈是**服务 Java 方法**的，而本地方法栈是为虚拟机**调用 Native 方法**服务的；

**线程共享部分：**

​	**Java 堆**（Java Heap）：Java 虚拟机中内存最大的一块，是**被所有线程共享的**，几乎所有的**对象实例**都在这里分配内存；
​	**方法区**（Methed Area）：用于存储已被虚拟机加载的**类信息**、**常量**（常量池）、**静态变量**、**即时编译后的代码**等数据。

###### JVM深拷贝和浅拷贝

浅拷贝（shallowCopy）只是**增加了一个指针**指向已存在的内存地址，

深拷贝（deepCopy）是**增加了一个指针并且申请了一个新的内存**，使这个增加的指针指向这个新的内存，

使用深拷贝的情况下，释放内存的时候**不会**因为出现浅拷贝时释放同一个内存的错误。

浅复制：仅仅是**指向被复制的内存地址**，如果原地址发生改变，那么浅复制出来的对象也会相应的改变。

深复制：在计算机中开辟一块新的**内存地址用于存放复制的对象**。


###### JVM堆与栈的区别

**物理地址**

- 堆的物理地址分配对对象是**不连续**的。因此性能慢些。在GC的时候**也要考虑到不连续的分配**，所以有各种算法。比如，标记-消除，复制，标记-压缩，分代（即新生代使用复制算法，老年代使用标记——压缩）。

- 栈使用的是数据结构中的栈，**先进后出的原则**，物理地址分配是连续的。所以**性能快**。


**内存分别**

- 堆因为是不连续的，所以分配的内存是在**运行期确认的**，因此大小不固定。一般**堆大小远远大于栈**。

- 栈是连续的，所以分配的内存大小要在**编译期就确认**，大小是固定的。


**存放内容**

- 堆存放的是**对象的实例和数组**。因此该区更关注的是**数据的存储**；

- 栈存放：局部变量，操作数栈，返回结果。该区更关注的是**程序方法的执行**。

    此外，静态变量存放在方法区，静态的对象还是存放于堆。

**程序的可见度**

- 堆对于整个**应用程序都是共享、可见的**。

- 栈只对于线程是可见的。所以也是**线程私有**。他的生命周期和线程相同。






##### 2.HotSpot虚拟机对象探秘

###### 对象的创建

说到对象的创建，首先让我们看看 `Java` 中提供的几种对象创建方式：

![image-20200313134926976](参考图片/image-20200313134926976.png)

​		虚拟机遇到一条new指令时，先**检查常量池**是否已经加载**相应的类**，如果没有，必须先执行相应的**类加载**。类加载通过后，接下来**分配内存**。若Java堆中内存是**绝对规整的**，使用“指针碰撞“方式分配内存；如果不是规整的，就从空闲列表中分配，叫做”**空闲列表**“方式。划分内存时还需要考虑一个问题-并发，也有两种方式: **CAS同步处理**，或者**本地线程分配缓冲**(Thread Local Allocation Buffer, TLAB)。然后内存空间初始化操作，接着是做一些必要的对象设置(元信息、哈希码…)，最后执行<init>方法。


###### 为对象分配内存

类加载完成后，接着会在Java堆中划分一块内存分配给对象。内存分配根据Java堆是否规整，有两种方式：

<img src="参考图片/image-20200313135235310.png" alt="image-20200313135235310" style="zoom:50%;" />

- **指针碰撞**：如果Java堆的内存是规整，即所有用过的内存放在一边，而空闲的的放在另一边。分配内存时将位于中间的指针指示器**向空闲的内存移动一段与对象大小相等的距离**，这样便完成分配内存工作。
- **空闲列表**：如果Java堆的内存不是规整的，则需要由虚拟机**维护一个列表来记录那些内存是可用的**，这样在分配的时候可以从列表中**查询到足够大的内存分配给对象**，并在分配后更新列表记录。

选择哪种分配方式是由 Java 堆是否规整来决定的，而 Java 堆是否规整又由所采用的垃圾收集器是否带有压缩整理功能决定。


###### 处理并发安全问题

对象的创建在虚拟机中是一个非常频繁的行为，哪怕只是修改一个指针所指向的位置，在并发情况下也是不安全的，可能出现正在给对象 A 分配内存，指针还没来得及修改，对象 B 又同时使用了原来的指针来分配内存的情况。解决这个问题有两种方案：

<img src="参考图片/image-20200313135332611.png" alt="image-20200313135332611" style="zoom:50%;" />

- 对分配内存空间的动作进行**同步处理**（采用 **CAS + 失败重试**来保障更新操作的原子性）；
- 把内存分配的动作按照线程**划分在不同的空间**之中进行，即每个线程在 Java 堆中**预先分配一小块内存**，称为**本地线程分配缓冲**（Thread Local Allocation Buffer, TLAB）。哪个线程要分配内存，就在哪个线程的 TLAB 上分配。只有 TLAB 用完并分配新的 TLAB 时，**才需要同步锁**。通过-XX:+/-UserTLAB参数来设定虚拟机是否使用TLAB。
  

###### 对象的访问定位

`Java`程序需要通过 `JVM` 栈上的引用访问堆中的具体对象。对象的访问方式取决于 `JVM` 虚拟机的实现。目前主流的访问方式有 **句柄** 和 **直接指针** 两种方式。

**句柄访问**

​		`	Java`堆中划分出一块内存来作为**句柄池**，引用中存储对象的**句柄地址**，而句柄中包含了**对象实例数据**与**对象类型数据**各自的**具体地址**信息

​		**优势**：引用中存储的是**稳定**的句柄地址，在对象被移动（垃圾收集时移动对象是非常普遍的行为）时只会改变**句柄中**的**实例数据指针**，而**引用**本身不需要修改。

**直接指针**

​		如果使用**直接指针**访问，**引用** 中存储的直接就是**对象地址**，那么`Java`堆对象内部的布局中就必须考虑如何放置访问**类型数据**的相关信息。

​		**优势**：速度更**快**，节省了**一次指针定位**的时间开销。由于对象的访问在`Java`中非常频繁，因此这类开销积少成多后也是非常可观的执行成本。HotSpot 中采用的就是这种方式。



###### Java会存在内存泄漏吗

​		内存泄漏是指不再被使用的对象或者变量一直被占据在内存中。理论上来说，Java是有GC垃圾回收机制的，也就是说，不再被使用的对象，会被GC自动回收掉，自动从内存中清除。

​		但是，即使这样，Java也还是**存在着内存泄漏**的情况，java导致内存泄露的原因很明确：长生命周期的对象持有短生命周期对象的引用就**很可能发生内存泄露**，尽管短生命周期对象已经不再需要，但是因为长生命周期对象持有它的引用而导致不能被回收，这就是java中内存泄露的发生场景。



##### 3.垃圾回收机制

###### 简述java内存分配与回收策略

​		这里我们来聊聊内存分配。对象的内存分配通常是在 **Java 堆上分配**（随着虚拟机优化技术的诞生，某些场景下也会在栈上分配，后面会详细介绍），对象主要分配在**新生代的 Eden 区**，如果启动了**本地线程缓冲**，将按照线程**优先在 TLAB 上分配**。少数情况下**也会直接在老年代上分配**。总的来说分配规则不是百分百固定的，其细节取决于哪一种垃圾收集器组合以及虚拟机相关参数有关，但是虚拟机对于内存的分配还是会遵循以下几种「普世」规则：

**对象优先在 Eden 区分配**

​		多数情况，对象都在新生代 Eden 区分配。当 Eden 区分配没有足够的空间进行分配时，虚拟机将会发起一次 **Minor GC**。如果本次 GC 后还是没有足够的空间，则将启用**分配担保机制**在老年代中分配内存。这里我们提到 Minor GC，如果你仔细观察过 GC 日常，通常我们还能从日志中发现 Major GC/Full GC。

- Minor GC 是指发生在新生代的 GC，因为 Java 对象大多都是朝生夕死，所有 Minor GC 非常频繁，一般回收速度也非常快；
- Major GC/Full GC 是指发生在老年代的 GC，出现了 Major GC 通常会伴随至少一次 Minor GC。Major GC 的速度通常会比 Minor GC 慢 10 倍以上。

**大对象直接进入老年代**

​		所谓大对象是指需要大量连续内存空间的对象，频繁出现大对象是致命的，会导致在内存还有不少空间的情况下**提前触发 GC** 以获取足够的连续空间来安置新对象。前面我们介绍过新生代使用的是标记-清除算法来处理垃圾回收的，如果大对象直接在新生代分配就会导致 Eden 区和两个 Survivor 区之间**发生大量的内存复制**。因此对于**大对象都会直接在老年代进行分配**。

**长期存活对象将进入老年代**

​		虚拟机采用**分代收集**的思想来管理内存，那么内存回收时就必须判断哪些对象应该放在新生代，哪些对象应该放在老年代。因此虚拟机给每个对象定义了一个对象年龄的计数器，如果对象在 Eden 区出生，并且能够被 Survivor 容纳，**将被移动到 Survivor 空间中**，这时设置对象年龄为 1。对象在 Survivor 区中每「熬过」一次 Minor GC 年龄就加 1，当年龄**达到一定程度（默认 15）** 就会被晋升到老年代。


###### 简述Java垃圾回收机制

在java中，程序员是不需要显示的去释放一个对象的内存的，而是由虚拟机自行执行。在JVM中，有一个垃圾回收线程，它是低优先级的，在正常情况下是不会执行的，只有在虚拟机空闲或者当前堆内存不足时，才会触发执行，扫面那些没有被任何引用的对象，并将它们添加到要回收的集合中，进行回收。



###### GC是什么？为什么要GC

GC 是垃圾收集的意思（Gabage Collection）,内存处理是编程人员容易出现问题的地方，忘记或者错误的内存回收会导致程序或系统的不稳定甚至崩溃，Java 提供的 GC 功能可以自动监测对象是否超过作用域从而达到自动回收内存的目的，Java 语言没有提供释放已分配内存的显示操作方法。



###### 垃圾回收的优点和原理。

java语言最显著的特点就是引入了**垃圾回收机制**，它使java程序员在编写程序时**不再考虑内存管理**的问题。

由于有这个垃圾回收机制，java中的**对象不再有“作用域”的概念**，只有**引用的对象才有“作用域”**。

​		垃圾回收有分代复制垃圾回收、标记垃圾回收、增量垃圾回收。垃圾回收机制有效的防止了内存泄露，可以有效的使用可使用的内存。垃圾回收器通常作为一个单独的**低级别**的线程运行，在不可预知的情况下**对内存堆中**已经死亡的或很长时间**没有用过的对象进行清除和回收**。

​		程序员**不能实时**的对某个对象或所有对象调用垃圾回收器进行垃圾回收。对于GC来说，当程序员创建对象时，GC就开始监控这个对象的地址、大小以及使用情况。通常，GC采用**有向图的方式**记录和管理堆(heap)中的所有对象。通过这种方式确定哪些对象是"可达的"，哪些对象是"不可达的"。当GC确定一些对象为"不可达"时，GC就有责任回收这些内存空间。**程序员可以手动执行System.gc()**，通知GC运行，但是Java语言规范并不保证GC一定会执行。

###### Java 中都有哪些引用类型？

- 强引用：发生 gc 的时候不会被回收。
- 软引用：有用但不是必须的对象，在发生内存溢出之前会被回收。
- 弱引用：有用但不是必须的对象，在下一次GC时会被回收。
- 虚引用（幽灵引用/幻影引用）：无法通过虚引用获得对象，用 PhantomReference 实现虚引用，虚引用的用途是在 gc 时返回一个通知。
  

###### 怎么判断对象是否可以被回收？

垃圾收集器在做垃圾回收的时候，首先需要判定的就是哪些内存是需要被回收的，哪些对象是「存活」的，是不可以被回收的；哪些对象已经「死掉」了，需要被回收。

一般有两种方法来判断：

- **引用计数器法**：为每个对象创建一个引用计数，有对象引用时计数器 +1，引用被释放时计数 -1，当计数器为 0 时就可以被回收。它有一个缺点不能解决循环引用的问题；
- **可达性分析算法**：从 GC Roots 开始向下搜索，搜索所走过的路径称为引用链。当一个对象到 GC Roots 没有任何引用链相连时，则证明此对象是可以被回收的。

###### 在Java中，对象什么时候被回收

- 当对象对当前使用这个对象的应用程序变得**不可触及**的时候，这个对象就可以被回收了。
- 垃圾回收不会发生在永久代，如果**永久代满了**或者是**超过了临界值**，会触发**完全垃圾回收(Full GC)**。如果你仔细查看垃圾收集器的输出信息，就会发现永久代也是被回收的。这就是为什么正确的永久代大小对避免Full GC是非常重要的原因。
  

###### 说一下 JVM 有哪些垃圾回收算法？

- **标记-清除算法**：标记无用对象，然后进行清除回收。缺点：**效率不高**，无法清除垃圾碎片。
- **复制算法**：按照容量划分**二个大小相等的内存区域**，当一块用完的时候将活着的对象复制到另一块上，然后再把已使用的内存空间一次清理掉。缺点：内存使用率不高，**只有原来的一半**。
- **标记-整理算法**：标记无用对象，让所有存活的对象**都向一端移动**，然后直接**清除掉端边界以外的内存**。
- **分代算法**：根据对象存活周期的不同**将内存划分为几块**，一般是**新生代和老年代**，新生代基本采用复制算法，老年代采用标记整理算法。


##### 4.虚拟机类加载机制

###### 描述一下JVM加载Class文件的原理机制

​		Java中的所有类，都需要由**类加载器装载到JVM中**才能运行。类加载器本身也是一个类，而它的工作**就是把class文件从硬盘读取到内存中**。在写程序的时候，我们几乎不需要关心类的加载，因为这些都是**隐式装载**的，除非我们有特殊的用法，**像是反射**，就需要显式的加载所需要的类。

类装载方式，有两种 ：

1.隐式装载， 程序在运行过程中当碰到通**过new 等方式生成对象**时，隐式调用类装载器加载对应的类到jvm中，

2.显式装载， 通过**class.forname()等方法**，显式加载需要的类

Java类的加载是动态的，它并**不会一次性**将所有类全部加载后再运行，而是保证程序运行的基础类(像是基类)完全加载到jvm中，至于其他类，则在需要的时候才加载。这当然就是为了节省内存开销。


###### 什么是类加载器，类加载器有哪些?

实现通过类的权限定名获取该类的二进制字节流的代码块叫做类加载器。

主要有一下四种类加载器:

- 启动类加载器(Bootstrap ClassLoader)用来加载**java核心类库**，无法被java程序直接引用。
- 扩展类加载器(extensions class loader):它用来加载 **Java 的扩展库**。Java 虚拟机的实现会提供一个扩展库目录。该类加载器在此目录里面查找并加载 Java 类。
- 系统类加载器（system class loader）：它根据 **Java 应用的类路径**（CLASSPATH）来加载 Java 类。一般来说，Java 应用的类都是由它来完成加载的。可以通过 ClassLoader.getSystemClassLoader()来获取它。
- 用户自定义类加载器，通过**继承 java.lang.ClassLoader类**的方式实现。



###### 说一下类装载的执行过程？

类装载分为以下 5 个步骤：

- 加载：根据查找路径**找到相应的 class 文件**然后导入；
- 验证：检查加载的 class 文件的**正确性**；
- 准备：给类中的静态变量**分配内存空间**；
- 解析：虚拟机将常量池中的符号引用替换成直接引用的过程。符号引用就理解为一个标示，而在直接引用直接指向内存中的地址；
- 初始化：对静态变量和静态代码块**执行初始化工作**。



###### 什么是双亲委派模型？

​		**双亲委派模型**：如果一个类加载器收到了**类加载的请求**，它首先不会自己去加载这个类，而是把这个请求**委派给父类加载器**去完成，每一层的类加载器都是如此，这样所有的加载请求都会被**传送到顶层的启动类加载器中**，只有当父加载无法完成加载请求（它的搜索范围中没找到所需的类）时，**子加载器才会尝试去加载类**。当一个类收到了类加载请求时，不会自己先去加载这个类，而是**将其委派给父类**，由父类去加载，如果此时父类不能加载，反馈给子类，**由子类去完成类的加载**。


##### 5.JVM调优

JDK 自带了很多监控工具，都位于 JDK 的 bin 目录下，其中最常用的是 jconsole 和 jvisualvm 这两款视图监控工具。

- jconsole：用于对 JVM 中的内存、线程和类等进行监控；
- jvisualvm：JDK 自带的全能分析工具，可以分析：内存快照、线程快照、程序死锁、监控内存的变化、gc 变化等。

常用的 JVM 调优的参数都有哪些？

- -Xms2g：初始化推大小为 2g；
- -Xmx2g：堆最大内存为 2g；
- -XX:NewRatio=4：设置年轻的和老年代的内存比例为 1:4；
- -XX:SurvivorRatio=8：设置新生代 Eden 和 Survivor 比例为 8:2；
- –XX:+UseParNewGC：指定使用 ParNew + Serial Old 垃圾回收器组合；
- -XX:+UseParallelOldGC：指定使用 ParNew + ParNew Old 垃圾回收器组合；
- -XX:+UseConcMarkSweepGC：指定使用 CMS + Serial Old 垃圾回收器组合；
- -XX:+PrintGC：开启打印 gc 信息；
- -XX:+PrintGCDetails：打印 gc 详细信息。
  