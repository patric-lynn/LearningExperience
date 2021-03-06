### Redis实战

1、什么是 Redis?.

2、Redis 的数据类型？

3、使用 Redis 有哪些好处？

4、Redis 相比 Memcached 有哪些优势？

5、Memcache 与 Redis 的区别都有哪些？

6、Redis 是单进程单线程的？

7、一个字符串类型的值能存储最大容量是多少？

8、Redis 的持久化机制是什么？各自的优缺点？

9、Redis 常见性能问题和解决方案：

10、redis 过期键的删除策略？

11、Redis 的回收策略（淘汰策略）?

12、为什么 edis 需要把所有数据放到内存中？

13、Redis 的同步机制了解么？

14、Pipeline 有什么好处，为什么要用 pipeline？

15、是否使用过 Redis 集群，集群的原理是什么？

16、Redis 集群方案什么情况下会导致整个集群不可用？

17、Redis 支持的 Java 客户端都有哪些？官方推荐用哪个？

18、Jedis 与 Redisson 对比有什么优缺点？

19、Redis 如何设置密码及验证密码？

20、说说 Redis 哈希槽的概念？

21、Redis 集群的主从复制模型是怎样的？

22、Redis 集群会有写操作丢失吗？为什么？

23、Redis 集群之间是如何复制的？

24、Redis 集群最大节点个数是多少？

25、Redis 集群如何选择数据库？

26、怎么测试 Redis 的连通性？

27、怎么理解 Redis 事务？

28、Redis 事务相关的命令有哪几个？

29、Redis key 的过期时间和永久有效分别怎么设置？

30、Redis 如何做内存优化？

31、Redis 回收进程如何工作的？

32、都有哪些办法可以降低 Redis 的内存使用情况呢？

33、Redis 的内存用完了会发生什么？

34、一个 Redis 实例最多能存放多少的 keys？List、Set、Sorted Set他们最多能存放多少元素？

35、MySQL 里有 2000w 数据，redis 中只存 20w 的数据，如何保证redis 中的数据都是热点数据？

36、Redis 最适合的场景？

37、假如 Redis 里面有 1 亿个 key，其中有 10w 个 key 是以某个固定的已知的前缀开头的，如果将它们全部找出来？

38、如果有大量的 key 需要设置同一时间过期，一般需要注意什么？

39、使用过 Redis 做异步队列么，你是怎么用的？

40、使用过 Redis 分布式锁么，它是什么回事？
![img](https://img-blog.csdnimg.cn/20191229172655836.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Rlc2lnbjQwNw==,size_16,color_FFFFFF,t_70)



#### 1、什么是 Redis?

Redis 是完全开源免费的，遵守 BSD 协议，是一个高性能的 key-value 数据库。

##### Redis 与其他 key - value 缓存产品有以下三个特点：

（1）Redis 支持**数据的持久化**，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。

（2）Redis 不仅仅支持简单的 key-value 类型的数据，同时还提供 list，set，zset，hash 等数据结构的存储。

（3）Redis 支持数据的备份，**即 master-slave 模式的数据备份**。

##### Redis 优势

（1）性能极高 – Redis 能读的速度是 110000 次/s,写的速度是 81000 次/s 。

（2）丰富的数据类型 – Redis 支持二进制案例的 Strings, Lists, Hashes, Sets 及Ordered Sets 数据类型操作。

（3）原子 – Redis 的所有操作都是原子性的，意思就是要么成功执行要么失败完全不执行。**单个操作是原子性的**。多个操作也支持事务，即原子性，通过 MULTI 和 EXEC指令包起来。

（4）丰富的特性 – Redis 还支持 publish/subscribe, 通知, key 过期等等特性。

##### Redis 与其他 key-value 存储有什么不同？

（1）Redis 有着更为**复杂的数据结构**并且提供对他们的原子性操作，这是一个不同于其他数据库的进化路径。Redis 的数据类型都是基于基本数据结构的同时对程序员透明，无需进行额外的抽象。

（2）Redis 运行在内存中但是可以持久化到磁盘，所以在对不同数据集进行高速读写时**需要权衡内存**，因为数据量不能大于硬件内存。在内存数据库方面的另一个优点是，相比在磁盘上相同的复杂的数据结构，在内存中操作起来非常简单，这样 Redis可以做很多内部复杂性很强的事情。同时，在磁盘格式方面他们是紧凑的以追加的方式产生的，因为他们并不需要进行随机访问。 

#### 2、Redis 的数据类型？

Redis 支持五种数据类型：string（字符串），**hash（哈希）**，list（列表），set（集合）及 **zset/sorted set：有序集合)**。

我们实际项目中比较常用的是 string，hash 。如果你是 Redis 中高级用户，还需要加上下面几种数据结构 HyperLogLog、Geo、Pub/Sub。

如果你说还玩过 Redis Module，像 BloomFilter，RedisSearch，Redis-ML，面试官得眼睛就开始发亮了。

#### 3、使用 Redis 有哪些好处？

（1）**速度快**，因为数据存在内存中，类似于 HashMap，HashMap 的优势就是查找和操作的时间复杂度都是 O1)

（2）支持丰富数据类型，支持 string，list，set，Zset，hash 等

（3）**支持事务**，操作都是原子性，所谓的原子性就是对数据的更改要么全部执行，要么全部不执行

（4）丰富的特性：可用于缓存，消息，按 key 设置过期时间，过期后将会自动删除 

#### 4、Redis 相比 Memcached 有哪些优势？

（1）Memcached 所有的值**均是简单的字符串**，redis 作为其替代者，支持更为丰富的数据类

（2）Redis 的速度比 Memcached 快

（3）Redis 可以持久化其数据

#### 5、Memcache 与 Redis 的区别都有哪些？

（1）存储方式 Memecache 把数据全部存在内存之中，**断电后会挂掉**，数据不能超过内存大小。 Redis 有部份存在硬盘上，这样能保证数据的持久性。

（2）数据支持类型 Memcache 对数据类型支持相对简单。 Redis **有复杂的数据类型**。

（3）使用**底层模型不同** 它们之间底层实现方式 以及与客户端之间通信的应用协议不一样。 Redis 直接自己构建了 **VM 机制** ，因为一般的系统调用系统函数的话，会浪费一定的时间去移动和请求。

#### 6、Redis 是单进程单线程的？

答：Redis 是**单线程模型**的，redis 利用队列技术将并发访问变为串行访问，消除了传统数据库串行控制的开销。

#### 7、一个字符串类型的值能存储最大容量是多少？

答：512M

#### 8、Redis 的持久化机制是什么？各自的优缺点？

Redis提供两种持久化机制 RDB 和 AOF 机制:

##### 1、RDB(Redis DataBase)持久化方式：

是指用**数据集快照**的方式(半持久化模式)记录 redis 数据库的**所有键值对**,在某个时间点将数据写入一个**临时文件**，持久化结束后，用这个临时文件替换上次持久化的文件，达到数据恢复。

###### 优点：

（1）**只有一个文件 dump.rdb**，方便持久化。

（2）容灾性好，一个文件可以保存到安全的磁盘。

（3）性能最大化，**fork 子进程来完成写操作**，让**主进程继续处理命令**，所以是 **IO最大化**。使用单独子进程来进行持久化，主进程不会进行任何 IO 操作，保证了 redis的高性能)

（4）相对于数据集大时，**比 AOF 的启动效率更高**。

###### 缺点：

数据安全性低。RDB 是间隔一段时间进行持久化，如果持久化之间 redis 发生故障，会发生数据丢失。所以这种方式更适合数据要求不严谨的时候

##### 2、AOF(Append-only file)持久化方式：

是指所有的命令行记录以 redis 命令请求协议的格式完全持久化存储)保存为 aof 文件。

###### 优点：

（1）数据安全，aof 持久化可以配置 **appendfsync 属性**，有 always，每进行一次命令操作就记录到 aof 文件中一次。

（2）通过 append 模式写文件，即使中途服务器宕机，可以通过 **redis-check-aof工具解决数据一致性问题**。

（3）**AOF 机制的 rewrite 模式**。AOF 文件没被 rewrite 之前（文件过大时会对命令进行合并重写），可以删除其中的某些命令（比如误操作的 flushall）)

###### 缺点：

（1）AOF 文件比 RDB 文件大，且**恢复速度慢**。

（2）数据集大的时候，**比 rdb 启动效率低**。

#### 9、Redis 常见性能问题和解决方案：

（1）**Master 最好不要写内存快照**，如果 Master 写内存快照，save 命令调度 rdbSave函数，会阻塞主线程的工作，当快照比较大时对性能影响是非常大的，会间断性暂停服务

（2）如果数据比较重要，**某个 Slave 开启 AOF 备份数据**，策略设置为每秒同步一次

（3）为了**主从复制**的速度和连接的稳定性，Master 和 Slave 最好在同一个局域网

（4）尽量避免在压力很大的主库上增加从

（5）主从复制**不要用图状结构，用单向链表结构更为稳定**，即：Master <- Slave1<- Slave2 <- Slave3…这样的结构方便解决单点故障问题，实现 Slave 对 Master的替换。如果 Master 挂了，可以立刻启用 Slave1 做 Master，其他不变。 

#### 10、redis 过期键的删除策略？

（1）**定时删除**:在设置键的过期时间的同时，**创建一个定时器** timer). 让定时器在键的过期时间来临时，立即执行对键的删除操作。

（2）**惰性删除**:放任键过期不管，但是每次从键空间中获取键时，都**检查取得的键是否过期**，如果过期的话，就删除该键;如果没有过期，就返回该键。

（3）**定期删除**:每**隔一段时间程序就对数据库进行一次检查**，删除里面的过期键。至于要删除多少过期键，以及要检查多少个数据库，则由算法决定。

#### 11、Redis 的内存回收策略（淘汰策略）?

-   volatile-lru：从已设置过期时间的数据集（server.db[i].expires）中挑选最近最少使用的数据淘汰

-   volatile-ttl：从已设置过期时间的数据集（server.db[i].expires）中挑选将要过期的数据淘汰

-   volatile-random：从已设置过期时间的数据集（server.db[i].expires）中任意选择数据淘汰

-   allkeys-lru：从数据集（server.db[i].dict）中挑选最近最少使用的数据淘汰

-   allkeys-random：从数据集（server.db[i].dict）中任意选择数据淘汰

-   no-enviction（驱逐）：禁止驱逐数据


​       注意这里的 6 种机制，volatile 和 allkeys 规定了是对已设置过期时间的数据集淘汰数据还是从全部数据集淘汰数据，后面的 lru、ttl 以及 random 是三种不同的淘汰策略，再加上一种 no-enviction 永不回收的策略。

​        使用策略规则：

（1）如果数据呈现幂律分布，也就是一部分数据访问频率高，一部分数据访问频率低，则使用 allkeys-lru

（2）如果数据呈现平等分布，也就是所有的数据访问频率都相同，则使用allkeys-random

#### 12、为什么 Redis 需要把所有数据放到内存中？

答 ：Redis 为了达到最快的读写速度将数据都读到内存中，并通过**异步的方式**将数据写入磁盘。所以 redis 具有快速和数据持久化的特征。如果不将数据放在内存中，磁盘 I/O 速度为严重影响 redis 的性能。在内存越来越便宜的今天，redis 将会越来越受欢迎。如果设置了最大使用的内存，则数据已有记录数达到内存限值后不能继续插入新值。

#### 13、Redis 的同步机制了解么？

##### **Redis主从同步**

​		数据可以**从主服务器向任意数量的从服务器上同步**，从服务器可以是关联其他从服务器的主服务器。这使得Redis可执行单层树复制。存盘可以有意无意的对数据进行写操作。由于完全实现了**发布/订阅机制**，使得从数据库在任何地方同步树时，可订阅一个频道并接收主服务器完整的消息发布 记录。同步对读取操作的可扩展性和数据冗余很有帮助。

[主从同步具体方式]https://blog.csdn.net/weixin_42711549/article/details/83061052?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2

[Redis主从复制的配置和实现原理]https://baijiahao.baidu.com/s?id=1644916874411447363&wfr=spider&for=pc

##### **主从同步工作原理**

​		Redis的主从结构可以采用一主多从或者级联结构，Redis主从复制可以根据是否是全量分为全量同步和增量同步。

**全同步是指salve启动时进行的初始化同步。**

**部分同步是指Redis运行过程中的修改同步。**

###### 一、**全同步/全量拷贝**

​		全同步过程如下：

1）在salve启动时，会向master发送一条SYNC指令。
2）master收到这条指令后，会启动一个备份进程将所有数据写到rdb文件中去。
3）更新master的状态（备份是否成功、备份时间等），**然后将rdb文件内容发送给等待中的salve**。

​		注意，master并不会立即将rdb内容发送给salve。而是**为每个等待中的salve注册写事件**，当salve对应的socket可以发送数据时，**再将rdb内容发送给salve**。具体过程为：

<img src="参考图片/image-20200405201713227.png" alt="image-20200405201713227" style="zoom:50%;" />

###### 二、**部分同步/增量拷贝**

​		当Redis的master/salve服务启动后，**首先进行全同步**。之后，所有的**写操作都在master上**，而所有的读操作都在salve上。因此**写操作需要及时同步到所有的salve上，这种同步就是部分同步**。

​		部分同步过程如下：

1）master收到一个操作，然后判断是否需要同步到salve。
2）如果需要同步，则**将操作记录到aof文件中**。
3）遍历所有的salve，将操作的指令和参数**写入到savle的回复缓存中**。
4）一旦slave对应的socket发送缓存中有空间写入数据，即将数据通过socket发出去。

​		具体过程为：

<img src="参考图片/image-20200405201817911.png" alt="image-20200405201817911" style="zoom:50%;" />

#### 14、Pipeline 有什么好处，为什么要用 pipeline？

​		答：可以将多次 IO 往返的时间缩减为一次，前提是 pipeline 执行的**指令之间没有因果相关性**。使用 redis-benchmark 进行压测的时候可以发现影响 redis 的 QPS峰值的一个重要因素是 pipeline 批次指令的数目。 

#### 15、是否使用过 Redis 集群，集群的原理是什么？

​		参考：[深入剖析Redis - Redis集群模式搭建与原理详解]https://www.jianshu.com/p/84dbb25cc8dc

##### 1.集群简介

`Redis` 集群是一个可以在多个 `Redis` 节点之间进行数据共享的设施`installation`。

`Redis` 集群**不支持那些需要同时处理多个键的 `Redis` 命令，** 因为执行这些命令需要在多个 `Redis` 节点之间移动数据， 并且在高负载的情况下， 这些命令将降低`Redis`集群的性能， 并导致不可预测的行为。

`Redis` 集群通过**分区`partition`来提供一定程度的可用性`availability`**： 即使集群中有一部分节点失效或者无法进行通讯， 集群也可以继续处理命令请求。

`Redis`集群提供了以下两个好处：

-   将数据自动切分`split`到多个节点的能力。
-   当集群中的一部分节点失效或者无法进行通讯时， 仍然可以继续处理命令请求的能力。

Redis Cluster 集群模式通常具有 **高可用**、**可扩展性**、**分布式**、**容错** 等特性。Redis 分布式方案一般有两种：

###### 1.1 客户端分区方案

**客户端** 就已经决定数据会被 **存储** 到哪个 redis 节点或者从哪个 redis 节点 **读取数据**。其主要思想是采用 **哈希算法** 将 Redis 数据的 key 进行散列，通过 hash 函数，特定的 key会 **映射** 到特定的 Redis 节点上。

**客户端分区方案** 的代表为 Redis Sharding，Redis Sharding 是 Redis Cluster 出来之前，业界普遍使用的 Redis **多实例集群** 方法。Java 的 Redis 客户端驱动库 Jedis，支持 Redis Sharding 功能，即 ShardedJedis 以及 **结合缓存池** 的 ShardedJedisPool。

**优点：**不使用 **第三方中间件**，**分区逻辑** 可控，**配置** 简单，节点之间无关联，容易 **线性扩展**，灵活性强。

**缺点：客户端无法 **动态增删** 服务节点，客户端需要自行维护 **分发逻辑**，客户端之间 **无连接共享**，会造成 **连接浪费**。

###### 1.2. 代理分区方案

**客户端** 发送请求到一个 **代理组件**，**代理** 解析 **客户端** 的数据，并将请求转发至正确的节点，最后将结果回复给客户端。

**优点**：简化 **客户端** 的分布式逻辑，**客户端** 透明接入，切换成本低，代理的 **转发** 和 **存储** 分离。

**缺点**：多了一层 **代理层**，加重了 **架构部署复杂度** 和 **性能损耗**。

**代理分区** 主流实现的有方案有 Twemproxy 和 Codis。

###### 1.3. 查询路由方案

**客户端随机地** 请求任意一个 Redis 实例，然后由 Redis 将请求 **转发** 给 **正确** 的 Redis 节点。Redis Cluster 实现了一种 **混合形式** 的 **查询路由**，但并不是 **直接** 将请求从一个 Redis 节点 **转发** 到另一个 Redis 节点，而是在 **客户端** 的帮助下直接 **重定向**（ redirected）到正确的 Redis 节点。

**优点**:**无中心节点**，数据按照 **槽** 存储分布在多个 Redis 实例上，可以平滑的进行节点 **扩容/缩容**，支持 **高可用** 和 **自动故障转移**，运维成本低。

**缺点**:严重依赖 Redis-trib 工具，缺乏 **监控管理**，需要依赖 Smart Client (**维护连接**，**缓存路由表**，MultiOp 和 Pipeline 支持)。Failover 节点的 **检测过慢**，不如 **中心节点** ZooKeeper 及时。Gossip 消息具有一定开销。无法根据统计区分 **冷热数据**。



##### 2.集群原理

1.  所有的`redis`节点彼此互联(`PING`-`PONG`机制),内部使用二进制协议优化传输速度和带宽。
2.  节点的`fail`是通过集群中超过半数的节点检测失效时才生效。
3.  客户端与`redis`节点直连,不需要中间`proxy`层.客户端不需要连接集群所有节点,连接集群中任何一个可用节点即可。
4.  `redis-cluster`把所有的物理节点映射到[`0-16383`]`slot`上,`cluster` 负责维护`node`<->`slot`<->`value`

`Redis`集群中内置了 `16384` 个哈希槽，当需要在 `Redis` 集群中放置一个 `key-value` 时，`redis` 先对`key` 使用 `crc16` 算法算出一个结果，然后把结果对 `16384` 求余数，这样每个 `key` 都会对应一个编号在 `0-16383` 之间的哈希槽，`redis` 会根据节点数量大致均等的将哈希槽映射到不同的节点。



##### 3.集群模式

* 主从模式
* Sentinel哨兵模式：着眼于**高可用**，在 master 宕机时会自动将 slave 提升为master，继续提供服务。
* Cluster集群模式：着眼于**扩展性**，在单个 redis 内存不足时，使用 Cluster 进行分片存储。

###### 1.主从模式

**主从模式介绍**

​		主从模式是三种模式中最简单的，在主从复制中，数据库分为两类：主数据库(master)和从数据库(slave)。

**其中主从复制有如下特点：**

* 主数据库可以进行读写操作，当读写操作导致数据变化时会自动将数据同步给从数据库
* 从数据库一般都是只读的，并且接收主数据库同步过来的数据
* 一个master可以拥有多个slave，但是一个slave只能对应一个master
* slave挂了不影响其他slave的读和master的读和写，重新启动后会将数据从master同步过来
* master挂了以后，不影响slave的读，但redis不再提供写服务，master重启后redis将重新对外提供写服务
* master挂了以后，不会在slave节点中重新选一个master

**工作机制：**

​		当slave启动后，主动向master发送SYNC命令。master接收到SYNC命令后在后台保存快照（RDB持久化）和缓存保存快照这段时间的命令，然后将保存的快照文件和缓存的命令发送给slave。slave接收到快照文件和命令后加载快照文件和缓存的执行命令。

​		复制初始化后，master每次接收到的写命令都会同步发送给slave，保证主从数据一致性。

**安全设置：**

​		当master节点设置密码后，

```
客户端访问master需要密码

启动slave需要密码，在配置文件中配置即可

客户端访问slave不需要密码
12345
```

**缺点：**

​		从上面可以看出，master节点在主从模式中唯一，若master挂掉，则redis无法对外提供写服务。

###### 2.Sentinel模式

**Sentinel模式介绍**

​		主从模式的弊端就是不具备高可用性，当master挂掉以后，**Redis将不能再对外提供写入操作**，因此sentinel应运而生。sentinel中文含义为哨兵，顾名思义，它的作用就是监控redis集群的运行状况，特点如下：

* sentinel模式是建立在主从模式的基础上，如果只有一个Redis节点，sentinel就没有任何意义
* 当master挂了以后，sentinel会在slave中选择一个做为master，并修改它们的配置文件，其他slave的配置文件也会被修改，比如slaveof属性会指向新的master
* 当master重新启动后，它将不再是master而是做为slave接收新的master的同步数据
* sentinel因为也是一个进程有挂掉的可能，所以sentinel也会启动多个形成一个sentinel集群
* 多sentinel配置的时候，sentinel之间也会自动监控
* 当主从模式配置密码时，sentinel也会同步将配置信息修改到配置文件中，不需要担心
* 一个sentinel或sentinel集群可以管理多个主从Redis，多个sentinel也可以监控同一个redis
* sentinel最好不要和Redis部署在同一台机器，不然Redis的服务器挂了以后，sentinel也挂了

​        工作机制为：     

* 每个sentinel以每秒钟一次的频率向它所知的master，slave以及其他sentinel实例发送一个 PING 命令 

* 如果一个实例距离最后一次有效回复 PING 命令的时间超过 down-after-milliseconds 选项所指定的值， 则这个实例会被sentinel标记为主观下线。 

* 如果一个master被标记为主观下线，则正在监视这个master的所有sentinel要以每秒一次的频率确认master的确进入了主观下线状态

* 当有足够数量的sentinel（大于等于配置文件指定的值）在指定的时间范围内确认master的确进入了主观下线状态， 则master会被标记为客观下线 

* 在一般情况下， 每个sentinel会以每 10 秒一次的频率向它已知的所有master，slave发送 INFO 命令 

* 当master被sentinel标记为客观下线时，sentinel向下线的master的所有slave发送 INFO 命令的频率会从 10 秒一次改为 1 秒一次 

* 若没有足够数量的sentinel同意master已经下线，master的客观下线状态就会被移除；
  若master重新向sentinel的 PING 命令返回有效回复，master的主观下线状态就会被移除

​       当使用sentinel模式的时候，客户端就不要直接连接Redis，而是**连接sentinel的ip和port**，由sentinel来提供具体的可提供服务的Redis实现，这样当master节点挂掉以后，sentinel就会感知并将新的master节点提供给使用者。

###### 3.Cluster模式

**Cluster模式介绍**

​		sentinel模式基本可以满足一般生产的需求，具备**高可用性**。但是当数据量过大到一台服务器存放不下的情况时，主从模式或sentinel模式就不能满足需求了，这个时候需要对存储的数据进行分片，将数据存储到多个Redis实例中。cluster模式的出现就是为了解决单机Redis容量有限的问题，将Redis的数据根据一定的规则分配到多台机器。

​		cluster可以说是sentinel和主从模式的结合体，通过cluster可以**实现主从**和**master重选**功能，所以如果配置两个副本三个分片的话，就需要六个Redis实例。因为Redis的数据是根据一定规则分配到cluster的不同机器的，当数据量过大时，可以新增机器进行扩容。

​		使用集群，只需要将redis配置文件中的`cluster-enable`配置打开即可。每个集群中至少需要三个主数据库才能正常运行，新增节点非常方便。

**cluster集群特点：**

* 多个redis节点**网络互联**，数据共享
* 所有的节点都是一主一从（也可以是一主多从），其中从不提供服务，仅作为备用
* 不支持同时处理多个key（如MSET/MGET），因为redis需要把key均匀分布在各个节点上，
  并发量很高的情况下同时创建key-value会降低性能并导致不可预测的行为
* 支持**在线增加、删除节点**
* 客户端可以连接任何一个主节点进行读写

#### 16、Redis 集群方案什么情况下会导致整个集群不可用？

​		答：有 A，B，C 三个节点的集群,在没有**复制模型**的情况下,如果节点 B 失败了，那么整个集群就会以为缺少 5501-11000 这个范围的槽而不可用。

#### 17、Redis 支持的 Java 客户端都有哪些？官方推荐用哪个？

​		答：Redisson、Jedis、lettuce 等等，官方推荐使用 Redisson。

#### 18、Jedis 与 Redisson 对比有什么优缺点？

​		答：Jedis 是 Redis 的 Java 实现的客户端，其 API 提供了比较全面的 Redis 命令的支持；Redisson 实现了分布式和可扩展的 Java 数据结构，和 Jedis 相比，功能较为简单，不支持字符串操作，不支持排序、事务、管道、分区等 Redis 特性。Redisson 的宗旨是促进使用者对 Redis 的关注分离，从而让使用者能够将精力更集中地放在处理业务逻辑上。 

#### 19、Redis 如何设置密码及验证密码？

​		设置密码：config set requirepass 123456

​		授权密码：auth 123456 

#### 20、说说 Redis 哈希槽的概念？

​		答：Redis 集群**没有使用一致性 hash**,而是引入了**哈希槽的概念**，Redis 集群有16384 个哈希槽，每个 key 通过 CRC16 校验后对 16384 取模来决定放置哪个槽，集群的每个节点负责一部分 hash 槽。 

#### 21、Redis 集群的主从复制模型是怎样的？

​		答：为了使在部分节点失败或者大部分节点无法通信的情况下集群仍然可用，集群使用了主从复制模型**,每个节点都会有 N-1 个复制品**.

#### 22、Redis 集群会有写操作丢失吗？为什么？

​		答 ：Redis 并不能保证**数据的强一致性**，这意味这在实际中集群在特定的条件下可能会丢失写操作。 

#### 23、Redis 集群之间是如何复制的？

​		答：**异步复制**

#### 24、Redis 集群最大节点个数是多少？

​		答：16384 个。

#### 25、Redis 集群如何选择数据库？

​		答：Redis 集群目前无法做数据库选择，默认在 0 数据库。

#### 26、怎么测试 Redis 的连通性？

​		答：使用 ping 命令。

#### 27、怎么理解 Redis 事务？

（1）事务是一个单独的隔离操作：事务中的所有命令都会序列化、按顺序地执行。事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。

（2）事务是一个原子操作：事务中的命令要么全部被执行，要么全部都不执行。

#### 28、Redis 事务相关的命令有哪几个？

​		答：MULTI、EXEC、DISCARD、WATCH

#### 29、Redis key 的过期时间和永久有效分别怎么设置？

​		答：EXPIRE 和 PERSIST 命令。

#### 30、Redis 如何做内存优化？

​		答：尽可能使用**散列表（hashes）**，散列表（是说散列表里面存储的数少）使用的**内存非常小**，所以你应该尽可能的将你的数据模型抽象到一个散列表里面。比如你的 web 系统中有一个用户对象，不要为这个用户的名称，姓氏，邮箱，密码设置单独的 key,而是应该把这个用户的所有信息存储到一张散列表里面。

#### 31、Redis 回收进程如何工作的？

​		答：一个客户端运行了新的命令，添加了新的数据。Redi 检查内存使用情况，如果大于 maxmemory 的限制, 则根据设定好的策略进行回收。一个新的命令被执行，等等。所以我们不断地穿越内存限制的边界，通过不断达到边界然后不断地回收回到边界以下。如果一个命令的结果导致大量内存被使用（例如很大的集合的交集保存到一个新的键），不用多久内存限制就会被这个内存使用量超越。 

#### 32、都有哪些办法可以降低 Redis 的内存使用情况呢？

​		答：如果你使用的是 32 位的 Redis 实例，可以好好利用 Hash,list,sorted set,set等集合类型数据，因为通常情况下很多小的 Key-Value 可以用更紧凑的方式存放到一起。 

#### 33、Redis 的内存用完了会发生什么？

​		答：如果达到设置的上限，Redis 的写命令会返回错误信息（但是读命令还可以正常返回。）或者你可以将 Redis 当缓存来使用配置淘汰机制，当 Redis 达到内存上限时会冲刷掉旧的内容。

#### 34、一个 Redis 实例最多能存放多少的 keys？List、Set、Sorted Set 他们最多能存放多少元素？

​		答：理论上 Redis 可以处理多达 2^32 的 keys，并且在实际中进行了测试，每个实例至少存放了 2 亿 5 千万的 keys。我们正在测试一些较大的值。任何 list、set、和 sorted set 都可以放 232 个元素。换句话说，Redis 的存储极限是系统中的可用内存值。 

#### 35、MySQL 里有 2000w 数据，redis 中只存 20w 的数据，如何保证 redis 中的数据都是热点数据？

答：Redis 内存数据集大小上升到一定大小的时候，就会施行数据淘汰策略。

相关知识：Redis 提供 6 种数据淘汰策略：

volatile-lru：从已设置过期时间的数据集（server.db[i].expires）中挑选最近最少使用的数据淘汰

volatile-ttl：从已设置过期时间的数据集（server.db[i].expires）中挑选将要过期的数据淘汰

volatile-random：从已设置过期时间的数据集（server.db[i].expires）中任意选择数据淘汰

allkeys-lru：从数据集（server.db[i].dict）中挑选最近最少使用的数据淘汰

allkeys-random：从数据集（server.db[i].dict）中任意选择数据淘汰

no-enviction（驱逐）：禁止驱逐数据

#### 36、Redis 最适合的场景？

##### 1、会话缓存（Session Cache）

​		最常用的一种使用 Redis 的情景是会话缓存（session cache）。用 Redis 缓存会话比其他存储（如 Memcached）的优势在于：Redis 提供持久化。当维护一个不是严格要求一致性的缓存时，如果用户的购物车信息全部丢失，大部分人都会不高兴的，现在，他们还会这样吗？ 幸运的是，随着 Redis 这些年的改进，很容易找到怎么恰当的使用 Redis 来缓存会话的文档。甚至广为人知的商业平台Magento 也提供 Redis 的插件。

##### 2、全页缓存（FPC）

​		除基本的会话 token 之外，Redis 还提供很简便的 FPC 平台。回到一致性问题，即使重启了 Redis 实例，因为有磁盘的持久化，用户也不会看到页面加载速度的下降，这是一个极大改进，类似 PHP 本地 FPC。 再次以 Magento 为例，Magento提供一个插件来使用 Redis 作为全页缓存后端。 此外，对 WordPress 的用户来说，Pantheon 有一个非常好的插件 wp-redis，这个插件能帮助你以最快速度加载你曾浏览过的页面。

##### 3、队列

​		Reids 在内存存储引擎领域的一大优点是提供 list 和 set 操作，这使得 Redis能作为一个很好的消息队列平台来使用。Redis 作为队列使用的操作，就类似于本地程序语言（如 Python）对 list 的 push/pop 操作。 如果你快速的在 Google中搜索“Redis queues”，你马上就能找到大量的开源项目，这些项目的目的就是利用 Redis 创建非常好的后端工具，以满足各种队列需求。例如，Celery 有一个后台就是使用 Redis 作为 broker，你可以从这里去查看。

##### 4，排行榜/计数器

​		Redis 在内存中对数字进行递增或递减的操作实现的非常好。集合（Set）和有序集合（Sorted Set）也使得我们在执行这些操作的时候变的非常简单，Redis 只是正好提供了这两种数据结构。所以，我们要从排序集合中获取到排名最靠前的 10个用户–我们称之为“user_scores”，我们只需要像下面一样执行即可： 当然，这是假定你是根据你用户的分数做递增的排序。如果你想返回用户及用户的分数，你需要这样执行： ZRANGE user_scores 0 10 WITHSCORES Agora Games 就是一个很好的例子，用 Ruby 实现的，它的排行榜就是使用 Redis 来存储数据的，你可以在这里看到。

##### 5、发布/订阅

​		最后（但肯定不是最不重要的）是 Redis 的发布/订阅功能。发布/订阅的使用场景确实非常多。我已看见人们在社交网络连接中使用，还可作为基于发布/订阅的脚本触发器，甚至用 Redis 的发布/订阅功能来建立聊天系统！ 

#### 37、假如 Redis 里面有 1 亿个 key，其中有 10w 个 key 是以某个固定的已知的前缀开头的，如果将它们全部找出来？

​		答：使用 keys 指令可以扫出指定模式的 key 列表。

​		对方接着追问：如果这个 redis 正在给线上的业务提供服务，那使用 keys 指令会有什么问题？

​		这个时候你要回答 redis 关键的一个特性：redis 的单线程的。keys 指令会导致线程阻塞一段时间，线上服务会停顿，直到指令执行完毕，服务才能恢复。这个时候可以使用 scan 指令，scan 指令可以无阻塞的提取出指定模式的 key 列表，但是会有一定的重复概率，在客户端做一次去重就可以了，但是整体所花费的时间会比直接用 keys 指令长。

#### 38、如果有大量的 key 需要设置同一时间过期，一般需要注意什么？

​		答：如果大量的 key 过期时间设置的过于集中，到过期的那个时间点，redis 可能会出现短暂的卡顿现象。一般需要在时间上加一个随机值，使得过期时间分散一些。

#### 39、使用过 Redis 做异步队列么，你是怎么用的？

​		答：一般使用 list 结构作为队列，rpush 生产消息，lpop 消费消息。当 lpop 没有消息的时候，要适当 sleep 一会再重试。如果对方追问可不可以不用 sleep 呢？list 还有个指令叫 blpop，在没有消息的时候，它会阻塞住直到消息到来。如果对方追问能不能生产一次消费多次呢？使用 pub/sub 主题订阅者模式，可以实现1:N 的消息队列。

##### 如果对方追问 pub/sub 有什么缺点？

​		在消费者下线的情况下，生产的消息会丢失，得使用专业的消息队列如 RabbitMQ等。

##### 如果对方追问 redis 如何实现延时队列？

​		我估计现在你很想把面试官一棒打死如果你手上有一根棒球棍的话，怎么问的这么详细。但是你很克制，然后神态自若的回答道：使用 sortedset，拿时间戳作为score，消息内容作为 key 调用 zadd 来生产消息，消费者用 zrangebyscore 指令获取 N 秒之前的数据轮询进行处理。到这里，面试官暗地里已经对你竖起了大拇指。但是他不知道的是此刻你却竖起了中指，在椅子背后。

#### 40、使用过 Redis 分布式锁么，它是什么回事？

​		先拿 setnx 来争抢锁，抢到之后，再用 expire 给锁加一个过期时间防止锁忘记了释放。

​		这时候对方会告诉你说你回答得不错，然后接着问如果在 setnx 之后执行 expire之前进程意外 crash 或者要重启维护了，那会怎么样？这时候你要给予惊讶的反馈：唉，是喔，这个锁就永远得不到释放了。紧接着你需要抓一抓自己得脑袋，故作思考片刻，好像接下来的结果是你主动思考出来的，然后回答：我记得 set 指令有非常复杂的参数，这个应该是可以同时把 setnx 和expire 合成一条指令来用的！对方这时会显露笑容，心里开始默念：摁，这小子还不错。

























### Redis重要问题

#### Redis的过期策略

##### 定期删除

redis 会将每个设置了过期时间的 key 放入到一个独立的字典中，以后会定期遍历这个字典来删除到期的 key。

定期删除策略

Redis 默认会每秒进行十次过期扫描（100ms一次），过期扫描不会遍历过期字典中所有的 key，而是采用了一种简单的贪心策略。

从过期字典中随机 20 个 key；
删除这 20 个 key 中已经过期的 key；
如果过期的 key 比率超过 1/4，那就重复步骤 1；

##### 惰性删除

除了定期遍历之外，它还会使用**惰性策略**来删除过期的 key，所谓惰性策略就是在客户端访问这个 key 的时候，redis 对 key 的过期时间进行检查，如果过期了就立即删除，不会给你返回任何东西。

定期删除是集中处理，惰性删除是零散处理。

为什么要采用定期删除+惰性删除2种策略呢？

**如果过期就删除。假设redis里放了10万个key，都设置了过期时间，你每隔几百毫秒，就检查10万个key，那redis基本上就死了，cpu负载会很高的，消耗在你的检查过期key上了**

但是问题是，定期删除可能会导致很多过期key到了时间并没有被删除掉，那咋整呢？所以就是惰性删除了。这就是说，在你获取某个key的时候，redis会检查一下 ，这个key如果设置了过期时间那么是否过期了？如果过期了此时就会删除，不会给你返回任何东西。

并不是key到时间就被删除掉，而是你查询这个key的时候，redis再懒惰的检查一下

通过上述两种手段结合起来，保证过期的key一定会被干掉。

所以说用了上述2种策略后，下面这种现象就不难解释了：数据明明都过期了，但是还占有着内存

#### Redis内存淘汰策略

这个问题可能有小伙伴们遇到过，放到Redis中的数据怎么没了？

​		因为Redis将数据放到内存中，内存是有限的，比如redis就只能用10个G，你要是往里面写了20个G的数据，会咋办？当然会干掉10个G的数据，然后就保留10个G的数据了。那干掉哪些数据？保留哪些数据？当然是干掉不常用的数据，保留常用的数据了

Redis提供的内存淘汰策略有如下几种：

-   noeviction **不会继续服务写请求** (DEL 请求可以继续服务)，读请求可以继续进行。这样可以保证不会丢失数据，但是会让线上的业务不能持续进行。这是默认的淘汰策略。
-   volatile-lru 尝试**淘汰设置了过期时间的 key**，**最少使用的 key 优先被淘汰**。没有设置过期时间的 key 不会被淘汰，这样可以保证需要持久化的数据不会突然丢失。（这个是使用最多的）
-   volatile-ttl 跟上面一样，除了淘汰的策略不是 LRU，而是 **key 的剩余寿命 ttl 的值**，ttl 越小越优先被淘汰。
-   volatile-random 跟上面一样，不过淘汰的 key 是**过期 key 集合中随机的 key**。
-   allkeys-lru 区别于 volatile-lru，这个策略要淘汰的 key 对象是**全体的 key 集合**，而不只是过期的 key 集合。这意味着**没有设置过期时间的 key 也会被淘汰**。
-   allkeys-random 跟上面一样，不过淘汰的策略是随机的 key。allkeys-random 跟上面一样，不过淘汰的策略是**随机的 key**。

#### Redis持久化策略

Redis的数据是存在内存中的，如果Redis发生宕机，那么数据会全部丢失，因此必须提供持久化机制。

Redis 的持久化机制有两种，第一种是**快照（RDB）**，第二种是 **AOF 日志**。快照是一次全量备份，AOF 日志是连续的增量备份。快照是内存数据的二进制序列化形式，在存储上非常紧凑，而 AOF 日志记录的是内存数据修改的指令记录文本。AOF 日志在长期的运行过程中会变的无比庞大，数据库重启时需要加载 AOF 日志进行指令重放，这个时间就会无比漫长。所以需要定期进行 AOF 重写，给 AOF 日志进行瘦身。

RDB是通过**Redis主进程fork子进程**，让子进程**执行磁盘 IO 操作来进行 RDB 持久化**，AOF 日志存储的是 Redis 服务器的**顺序指令序列**，AOF 日志**只记录对内存进行修改的指令记录**。即RDB记录的是数据，AOF记录的是指令.

##### RDB和AOF到底该如何选择

-   不要仅仅使用 RDB，因为那样会导致你丢失很多数据，因为RDB是隔一段时间来备份数据
-   也不要仅仅使用 AOF，因为那样有两个问题，第一，通过 AOF 做冷备没有RDB恢复速度快; 第二，RDB 每次简单粗暴生成数据快照，更加健壮，可以避免 AOF 这种复杂的备份和恢复机制的 bug

​        用RDB恢复内存状态会丢失很多数据，重放AOP日志又很慢。Redis4.0退出了混合持久化来解决这个问题。将 rdb 文件的内容和增量的 AOF 日志文件存在一起。这里的 AOF 日志不再是全量的日志，而是自持久化开始到持久化结束的这段时间发生的增量 AOF 日志，通常这部分 AOF 日志很小。于是在 Redis 重启的时候，可以先加载 rdb 的内容，然后再重放增量 AOF 日志就可以完全替代之前的 AOF 全量文件重放，重启效率因此大幅得到提升。

#### 缓存雪崩和缓存穿透

##### 1.缓存雪崩是什么？

​		假设有如下一个系统，高峰期请求为5000次/秒，4000次走了缓存，只有1000次落到了数据库上，数据库每秒1000的并发是一个正常的指标，完全可以正常工作，但如果缓存宕机了，每秒5000次的请求会全部落到数据库上，数据库立马就死掉了，因为数据库一秒最多抗2000个请求，如果DBA重启数据库，立马又会被新的请求打死了，这就是缓存雪崩。

###### 如何解决缓存雪崩

-   事前：redis高可用，主从+哨兵，redis cluster，避免全盘崩溃
-   事中：本地ehcache缓存 + hystrix限流&降级，避免MySQL被打死
-   事后：redis持久化，快速恢复缓存数据

##### 2.缓存穿透是什么？

​		假如客户端每秒发送5000个请求，其中4000个为黑客的恶意攻击，即在数据库中也查不到。举个例子，用户id为正数，黑客构造的用户id为负数，如果黑客每秒一直发送这4000个请求，缓存就不起作用，数据库也很快被打死。

###### 如何解决缓存穿透

-   查询不到的数据也放到缓存，value为空，如set -999 “”


##### 3.避免缓存穿透的利器BloomFilter

###### Bloom Filter 概念

布隆过滤器（英语：Bloom Filter）是1970年由一个叫布隆的小伙子提出的。它实际上是一个很长的二进制向量和一系列随机映射函数。布隆过滤器可以用于检索一个元素是否在一个集合中。它的优点是空间效率和查询时间都远远超过一般的算法，缺点是有一定的误识别率和删除困难。

###### Bloom Filter 原理

布隆过滤器的原理是，当一个元素被加入集合时，通过K个散列函数将这个元素映射成一个位数组中的K个点，把它们置为1。检索时，我们只要看看这些点是不是都是1就（大约）知道集合中有没有它了：如果这些点有任何一个0，则被检元素一定不在；如果都是1，则被检元素很可能在。这就是布隆过滤器的基本思想。

Bloom Filter跟单哈希函数Bit-Map不同之处在于：Bloom Filter使用了k个哈希函数，每个字符串跟k个bit对应。从而降低了冲突的概率。

<img src="https://user-gold-cdn.xitu.io/2019/10/28/16e112fbd031fe71?imageView2/0/w/1280/h/960/ignore-error/1" alt="img" style="zoom:50%;" />

###### 缓存穿透

<img src="https://user-gold-cdn.xitu.io/2019/10/30/16e1b6d07e758c4a?imageView2/0/w/1280/h/960/ignore-error/1" alt="img" style="zoom:70%;" />

​                                                                    每次查询都会直接打到DB

​		言而简之就是我们先把我们数据库的数据都加载到我们的过滤器中，比如数据库的id现在有：1、2、3

​		那就用id：1 为例子他在上图中经过三次hash之后，把三次原本值0的地方改为1

​		下次数据进来查询的时候如果id的值是1，那么我就把1拿去三次hash 发现三次hash的值，跟上面的三个位置完全一样，那就能证明过滤器中有1的,反之如果不一样就说明不存在了

​		那应用的场景在哪里呢？一般我们都会用来防止缓存击穿

​		简单来说就是你数据库的id都是1开始然后自增的，那我知道你接口是通过id查询的，我就拿负数去查询，这个时候，会发现缓存里面没这个数据，我又去数据库查也没有，一个请求这样，100个，1000个，10000个呢？你的DB基本上就扛不住了，如果在缓存里面加上这个，是不是就不存在了，你判断没这个数据就不去查了，直接return一个数据为空不就好了嘛。

​		这玩意这么好使那有啥缺点么？有的，我们接着往下看

###### Bloom Filter的缺点

bloom filter之所以能做到在时间和空间上的效率比较高，是因为牺牲了判断的准确率、删除的便利性

- 存在误判，可能要查到的元素并没有在容器中，但是hash之后得到的k个位置上值都是1。如果bloom filter中存储的是黑名单，那么可以通过建立一个白名单来存储可能会误判的元素。
- 删除困难。一个放入容器的元素映射到bit数组的k个位置上是1，删除的时候不能简单的直接置为0，可能会影响其他元素的判断。可以采用[Counting Bloom Filter](http://wiki.corp.qunar.com/confluence/download/attachments/199003276/US9740797.pdf?version=1&modificationDate=1526538500000&api=v2)

###### Bloom Filter 实现

布隆过滤器有许多实现与优化，Guava中就提供了一种Bloom Filter的实现。

在使用bloom filter时，绕不过的两点是预估数据量n以及期望的误判率fpp，

在实现bloom filter时，绕不过的两点就是hash函数的选取以及bit数组的大小。

对于一个确定的场景，我们预估要存的数据量为n，期望的误判率为fpp，然后需要计算我们需要的Bit数组的大小m，以及hash函数的个数k，并选择hash函数

**(1)Bit数组大小选择**

  根据预估数据量n以及误判率fpp，bit数组大小的m的计算方式：

![img](https://user-gold-cdn.xitu.io/2019/10/28/16e112fbd079c208?imageView2/0/w/1280/h/960/ignore-error/1)

**(2)哈希函数选择**

 由预估数据量n以及bit数组长度m，可以得到一个hash函数的个数k：

![img](https://user-gold-cdn.xitu.io/2019/10/28/16e112fbd09afadb?imageView2/0/w/1280/h/960/ignore-error/1)

 哈希函数的选择对性能的影响应该是很大的，一个好的哈希函数要能近似等概率的将字符串映射到各个Bit。选择k个不同的哈希函数比较麻烦，一种简单的方法是选择一个哈希函数，然后送入k个不同的参数。

哈希函数个数k、位数组大小m、加入的字符串数量n的关系可以参考[Bloom Filters - the math](http://pages.cs.wisc.edu/~cao/papers/summary-cache/node8.html)，[Bloom_filter-wikipedia](https://en.wikipedia.org/wiki/Bloom_filter)

要使用BloomFilter，需要引入guava包：

```java
 <dependency>
            <groupId>com.google.guava</groupId>
            <artifactId>guava</artifactId>
            <version>23.0</version>
 </dependency>    
```

测试分两步：

1、往过滤器中放一百万个数，然后去验证这一百万个数是否能通过过滤器

2、另外找一万个数，去检验漏网之鱼的数量

```java
/**
 * 测试布隆过滤器(可用于redis缓存穿透)
 * 
 */
public class TestBloomFilter {

    private static int total = 1000000;
    private static BloomFilter<Integer> bf = BloomFilter.create(Funnels.integerFunnel(), total);
//    private static BloomFilter<Integer> bf = BloomFilter.create(Funnels.integerFunnel(), total, 0.001);

    public static void main(String[] args) {
        // 初始化1000000条数据到过滤器中
        for (int i = 0; i < total; i++) {
            bf.put(i);
        }

        // 匹配已在过滤器中的值，是否有匹配不上的
        for (int i = 0; i < total; i++) {
            if (!bf.mightContain(i)) {
                System.out.println("有坏人逃脱了~~~");
            }
        }

        // 匹配不在过滤器中的10000个值，有多少匹配出来
        int count = 0;
        for (int i = total; i < total + 10000; i++) {
            if (bf.mightContain(i)) {
                count++;
            }
        }
        System.out.println("误伤的数量：" + count);
    }

}
```

运行结果：



![img](https://user-gold-cdn.xitu.io/2019/10/28/16e112fbd21d26d6?imageView2/0/w/1280/h/960/ignore-error/1)



运行结果表示，遍历这一百万个在过滤器中的数时，都被识别出来了。一万个不在过滤器中的数，误伤了320个，错误率是0.03左右。

看下BloomFilter的源码：

```java
public static <T> BloomFilter<T> create(Funnel<? super T> funnel, int expectedInsertions) {
        return create(funnel, (long) expectedInsertions);
    }  

    public static <T> BloomFilter<T> create(Funnel<? super T> funnel, long expectedInsertions) {
        return create(funnel, expectedInsertions, 0.03); // FYI, for 3%, we always get 5 hash functions
    }

    public static <T> BloomFilter<T> create(
          Funnel<? super T> funnel, long expectedInsertions, double fpp) {
        return create(funnel, expectedInsertions, fpp, BloomFilterStrategies.MURMUR128_MITZ_64);
    }

    static <T> BloomFilter<T> create(
      Funnel<? super T> funnel, long expectedInsertions, double fpp, Strategy strategy) {
     ......
    }
```

BloomFilter一共四个create方法，不过最终都是走向第四个。看一下每个参数的含义：

funnel：数据类型(一般是调用Funnels工具类中的)

expectedInsertions：期望插入的值的个数

fpp 错误率(默认值为0.03)

strategy 哈希算法(我也不懂啥意思)Bloom Filter的应用

在最后一个create方法中，设置一个断点：



![img](https://user-gold-cdn.xitu.io/2019/10/28/16e112fbd6486cb5?imageView2/0/w/1280/h/960/ignore-error/1)





![img](https://user-gold-cdn.xitu.io/2019/10/28/16e112fbd78368a7?imageView2/0/w/1280/h/960/ignore-error/1)



上面的numBits，表示存一百万个int类型数字，需要的位数为7298440，700多万位。理论上存一百万个数，一个int是4字节32位，需要4*8*1000000=3200万位。如果使用HashMap去存，按HashMap50%的存储效率，需要6400万位。可以看出BloomFilter的存储空间很小，只有HashMap的1/10左右

上面的numHashFunctions，表示需要5个函数去存这些数字

使用第三个create方法，我们设置下错误率：

```
private static BloomFilter<Integer> bf = BloomFilter.create(Funnels.integerFunnel(), total, 0.0003);
```

再运行看看：



![img](https://user-gold-cdn.xitu.io/2019/10/28/16e112fc0062916c?imageView2/0/w/1280/h/960/ignore-error/1)



此时误伤的数量为4，错误率为0.04%左右。



![img](https://user-gold-cdn.xitu.io/2019/10/28/16e112fc02001fd5?imageView2/0/w/1280/h/960/ignore-error/1)



当错误率设为0.0003时，所需要的位数为16883499，1600万位，需要12个函数

和上面对比可以看出，错误率越大，所需空间和时间越小，错误率越小，所需空间和时间约大

常见的几个应用场景：

- cerberus在收集监控数据的时候, 有的系统的监控项量会很大, 需要检查一个监控项的名字是否已经被记录到db过了, 如果没有的话就需要写入db.
- 爬虫过滤已抓到的url就不再抓，可用bloom filter过滤
- 垃圾邮件过滤。如果用哈希表，每存储一亿个 email地址，就需要 1.6GB的内存（用哈希表实现的具体办法是将每一个 email地址对应成一个八字节的信息指纹，然后将这些信息指纹存入哈希表，由于哈希表的存储效率一般只有 50%，因此一个 email地址需要占用十六个字节。一亿个地址大约要 1.6GB，即十六亿字节的内存）。因此存贮几十亿个邮件地址可能需要上百 GB的内存。而Bloom Filter只需要哈希表 1/8到 1/4 的大小就能解决同样的问题。

###### 总结

​		布隆过滤器主要是在回答道缓存穿透的时候引出来的，文章里面还是写的比较复杂了，很多都是网上我看到就复制下来了，大家只要知道他的原理，还有就是知道他的场景能在面试中回答出他的作用就好了。



#### Redis 主从架构

​		单机的 redis，能够承载的 QPS 大概就在上万到几万不等。对于缓存来说，一般都是用来支撑读高并发的。因此架构做成**主从(master-slave)架构**，**一主多从，主负责写，并且将数据复制到其它的 slave 节点，从节点负责读**。所有的读请求全部走从节点。这样也可以很轻松实现水平扩容，支撑读高并发。

<img src="参考图片/image-20200414000920224.png" alt="image-20200414000920224" style="zoom:40%;" />

​		**redis replication -> 主从架构 -> 读写分离 -> 水平扩容支撑读高并发**

##### **1.redis replication 的核心机制**

-   redis 采用**异步方式**复制数据到 slave 节点，redis2.8 开始，slave node 会周期性地确认自己每次复制的数据量；
-   一个 master node 是可以**配置多个 slave node 的**；
-   slave node 也可以**连接其他的 slave node**；
-   slave node 做复制的时候，**不会 block master node 的正常工作**；
-   slave node 在做复制的时候，也**不会 block 对自己的查询操作**，它会用旧的数据集来提供服务；但是复制完成的时候，**需要删除旧数据集，加载新数据集，这个时候就会暂停对外服务了**；
-   slave node 主要用来**进行横向扩容，做读写分离**，扩容的 slave node 可以提高读的吞吐量。

​       注意，如果采用了主从架构，那么建议**必须开启 master node 的持久化**，不建议用 slave node 作为 master node 的数据热备，因为那样的话，如果你关掉 master 的持久化，可能在 **master 宕机重启的时候数据是空的**，然后可能一经过复制， slave node 的数据也丢了。

​		另外，master 的各种备份方案，**也需要做**。万一本地的所有文件丢失了，从备份中**挑选一份 rdb 去恢复 master**，这样才能确保启动的时候，是有数据的，即使采用了后续讲解的**高可用机制**，slave node 可以自动接管 master node，但也可能 **sentinel 还没检测到 master failure**，master node 就自动重启了，还是可能导致上面所有的 **slave node 数据被清空**。



##### **2.redis 主从复制的核心原理**

​		当启动一个 slave node 的时候，它会发送一个 **PSYNC 命令给 master node**。

​		如果这是 slave node 初次连接到 master node，那么会触发一次 **full resynchronization 全量复制**。此时 master 会**启动一个后台线程**，开始**生成一份 RDB 快照文件**，

​		同时还会**将从客户端 client 新收到的所有写命令缓存在内存中**。RDB 文件生成完毕后， master 会将这个 RDB 发送给 slave，**slave 会先写入本地磁盘，然后再从本地磁盘加载到内存中**，

​		接着 master 会将内存中**缓存的写命令发送到 slave**，slave 也会同步这些数据。

​		slave node 如果跟 master node 有网络故障，**断开了连接，会自动重连**，连接之后 master node **仅会复制给 slave 部分缺少的数据**。

<img src="参考图片/image-20200414001426795.png" alt="image-20200414001426795" style="zoom:50%;" />

###### **过程原理**

-   当从库和主库建立MS关系后，会向**主数据库发送SYNC命令**
-   主库接收到SYNC命令后会开始在**后台保存快照(RDB持久化过程)，并将期间接收到的写命令缓存起来**
-   当快照完成后，主Redis会将快照文件和所有缓存的写命令发送给从Redis
-   从Redis接收到后，**会载入快照文件并且执行收到的缓存的命令**
-   之后，主Redis每当接收到写命令时就会将命令发送从Redis，从而保证数据的一致

###### 缺点

​		所有的slave节点数据的复制和同步**都由master节点来处理**，会造成master节点压力太大，使用从从结构来解决。




##### 3.生产环境中的 redis 是怎么部署的？

​		redis cluster，10 台机器，**5 台机器部署了 redis 主实例**，另外 5 台机器**部署了 redis 的从实例**，每个主实例挂了一个从实例，**5 个节点对外提供读写服务**，每个节点的读写**高峰qps可能可以达到每秒 5 万**，5 台机器最多是 25 万读写请求/s。

​		机器是什么配置？32G 内存+ 8 核 CPU + 1T 磁盘，但是**分配给 redis 进程的是10g内存**，一般线上生产环境，redis 的内存**尽量不要超过 10g**，超过 10g 可能会有问题。5 台机器对外提供读写，一共有 50g 内存。

​		因为每个主实例都挂了一个从实例，**所以是高可用的**，任何一个主实例宕机，**都会自动故障迁移**，redis 从实例会**自动变成主实例继续提供读写服务**。

​		你往内存里写的是什么数据？每条数据的大小是多少？商品数据，每条数据是 10kb。100 条数据是 1mb，10 万条数据是 1g。常驻内存的是 **200 万条商品数据**，占用内存是 20g，仅仅不到总内存的 50%。目前**高峰期每秒就是 3500 左右的请求量**。其实大型的公司，会有基础架构的 team 负责缓存集群的运维。




#### Redis分区与分布式问题

##### 1.Redis分区

###### **1.Redis是单线程的，如何提高多核CPU的利用率？**

​		可以在同一个服务器部署多个Redis的实例，并**把他们当作不同的服务器来使用**，在某些时候，无论如何一个服务器是不够的， 所以，如果你想使用多个CPU，你可以**考虑一下分片（shard）**。

###### **2.为什么要做Redis分区？**

​		分区可以让Redis**管理更大的内存**，Redis将可以**使用所有机器的内存**。如果没有分区，你最多只能使用一台机器的内存。分区使Redis的计算能力**通过简单地增加计算机得到成倍提升**，Redis的网络带宽也会随着计算机和网卡的增加而成倍增长。

###### **3.你知道有哪些Redis分区实现方案？**

-   **客户端分区**就是在客户端就已经决定**数据会被存储到哪个redis节点或者从哪个redis节点读取**。大多数客户端已经实现了客户端分区。
-   **代理分区** 意味着客户端**将请求发送给代理，然后代理决定去哪个节点写数据或者读数据**。代理根据分区规则决定请求哪些Redis实例，然后根据Redis的响应结果返回给客户端。redis和memcached的一种代理实现就是Twemproxy
-   **查询路由(Query routing)** 的意思是**客户端随机地请求任意一个redis实例**，然后由Redis将请求转发给正确的Redis节点。Redis Cluster实现了**一种混合形式的查询路由**，但并不是直接将请求从一个redis节点转发到另一个redis节点，而是在客户端的帮助下直接redirected到正确的redis节点。

###### **4.Redis分区有什么缺点？**

-   **涉及多个key的操作通常不会被支持**。例如你不能对两个集合求交集，因为他们可能被**存储到不同的Redis实例**（实际上这种情况也有办法，但是不能直接使用交集指令）。
-   **同时操作多个key,则不能使用Redis事务**.
-   **分区使用的粒度是key**，不能使用一个非常长的排序key存储一个数据集（The partitioning granularity is the key, so it is not possible to shard a dataset with a single huge key like a very big sorted set）
-   当使用分区的时候，数据处理会非常复杂，例如为了备份你必须从不同的Redis实例和主机同时收集RDB / AOF文件。
-   **分区时动态扩容或缩容可能非常复杂**。Redis集群在运行时增加或者删除Redis节点，能做到最大程度对用户透明地数据再平衡，但其他一些客户端分区或者代理分区方法则不支持这种特性。然而，有一种预分片的技术也可以较好的解决这个问题。

##### 2.Redis分布式问题

###### 1.Redis实现分布式锁

​		Redis为单进程单线程模式，采用**队列模式**将并发访问变成**串行访问**，且多客户端对Redis的连接并不存在竞争关系。

**Redis中可以使用SETNX命令实现分布式锁。**

-   当且仅当 key 不存在，将 key 的值设为 value。 若给定的 key 已经存在，则 SETNX 不做任何动作

-   SETNX 是『SET if Not eXists』(如果不存在，则 SET)的简写。

-   返回值：设置成功，返回 1 。设置失败，返回 0 。


**使用SETNX完成同步锁的流程及事项如下：**

-   使用SETNX**命令获取锁**，若返回0（key已存在，锁已存在）则获取失败，反之获取成功

-   为了防止获取锁后程序出现异常，导致其他线程/进程调用SETNX命令总是返回0而进入死锁状态，需要为该key**设置一个“合理”的过期时间**

-   释放锁，使用DEL命令**将锁数据删除**



###### **2.如何解决 Redis 的并发竞争 Key 问题**

​		所谓 Redis 的并发竞争 Key 的问题也就是**多个系统同时对一个 key 进行操作**，但是最后执行的顺序和我们期望的顺序不同，这样也就导致了结果的不同！

​		推荐一种方案：**分布式锁**（zookeeper 和 redis 都可以实现分布式锁）。（如果不存在 Redis 的并发竞争 Key 问题，不要使用分布式锁，这样会影响性能）

​		基于zookeeper**临时有序节点**可以实现的分布式锁。大致思想为：每个客户端对某个方法加锁时，在zookeeper上的与该方法对应的指定节点的目录下，**生成一个唯一的瞬时有序节点**。 判断是否获取锁的方式很简单，只需要判断有序节点中序号最小的一个。 当释放锁的时候，只需将这个瞬时节点删除即可。同时，其可以避免服务宕机导致的锁无法释放，而产生的死锁问题。完成业务流程后，删除对应的子节点释放锁。在实践中，当然是从以可靠性为主。所以首推Zookeeper。

###### 3.分布式Redis是前期做还是后期规模上来了再做好？为什么？

​		既然Redis是如此的轻量（**单实例只使用1M内存**），为防止以后的扩容，最好的办法就是**一开始就启动较多实例**。即便你只有一台服务器，你也可以**一开始就让Redis以分布式的方式运行**，使用分区，在同一台服务器上启动多个实例。

​		一开始就多设置几个Redis实例，例如32或者64个实例，对大多数用户来说这操作起来可能比较麻烦，但是从长久来看做这点牺牲是值得的。

​		这样的话，当你的数据不断增长，需要更多的Redis服务器时，你需要做的就是仅仅将Redis实例从一台服务迁移到另外一台服务器而已（而不用考虑重新分区的问题）。一旦你添加了另一台服务器，你需要将你一半的Redis实例从第一台机器迁移到第二台机器。




### Redis面试技巧

小伙子您好，看你简历上写了你项目里面用到了Redis，你们为啥用Redis？

​		心里忍不住暗骂，这叫啥问题，大家不都是用的这个嘛，但是你不能说出来。

​		认真回答道：帅气迷人的面试官您好，因为传统的关系型数据库如Mysql已经不能适用所有的场景了，**比如秒杀的库存扣减，APP首页的访问流量高峰等等，都很容易把数据库打崩，所以引入了缓存中间件**，目前市面上比较常用的**缓存中间件有Redis 和 Memcached 不过中和考虑了他们的优缺点，最后选择了Redis**。

​		至于更细节的对比朋友们记得查阅Redis 和 Memcached 的区别，比如两者的优缺点对比和各自的场景，后续我有时间也会写出来。

​		那小伙子，我再问你，Redis有哪些数据结构呀？

**字符串String、字典Hash、列表List、集合Set、有序集合SortedSet。**

​		这里我相信99%的读者都能回答上来Redis的5个基本数据类型。如果回答不出来的小伙伴我们就要加油补课哟，大家知道五种类型最适合的场景更好。

​		但是，如果你是Redis中高级用户，而且你要在这次面试中突出你和其他候选人的不同，还需要加上下面几种数据结构**HyperLogLog、Geo、Pub/Sub**。

​		如果你还想加分，那你说还玩过Redis Module，像BloomFilter，RedisSearch，Redis-ML，这个时候面试官得眼睛就开始发亮了，心想这个小伙子有点东西啊。

​		注：本人在面试回答到Redis相关的问题的时候，经常提到**BloomFilter**（布隆过滤器）这玩意的使用场景是真的多，而且用起来是真的香，原理也好理解，看一下文章就可以在面试官面前侃侃而谈了，不香么？下方传送门 ↓

**避免缓存击穿的利器之BloomFilter**

​		如果有大量的key需要设置同一时间过期，一般需要注意什么？

​		如果大量的key过期时间设置的过于集中，到过期的那个时间点，redis可能会出现短暂的卡顿现象。严重的话会出现缓存雪崩，我们一般需要在时间上**加一个随机值**，使得过期时间分散一些。

​		电商首页经常会使用定时任务刷新缓存，可能大量的数据失效时间都十分集中，如果失效时间一样，又刚好在失效的时间点大量用户涌入，就有可能造成缓存雪崩

​		那你使用过Redis分布式锁么，它是什么回事？

​		先拿setnx来争抢锁，抢到之后，再用expire给锁加一个过期时间防止锁忘记了释放。

​		这时候对方会告诉你说你回答得不错，然后接着问如果在setnx之后执行expire之前进程意外crash或者要重启维护了，那会怎么样？

​		这时候你要给予惊讶的反馈：唉，是喔，这个锁就永远得不到释放了。紧接着你需要抓一抓自己得脑袋，故作思考片刻，好像接下来的结果是你主动思考出来的，然后回答：我记得set指令有非常复杂的参数，这个应该是可以同时把setnx和expire合成一条指令来用的！

​		对方这时会显露笑容，心里开始默念：嗯，这小子还不错，开始有点意思了。

​		假如Redis里面有1亿个key，其中有10w个key是以某个固定的已知的前缀开头的，如何将它们全部找出来？

**使用keys指令可以扫出指定模式的key列表。**

​		对方接着追问：如果这个redis正在给线上的业务提供服务，那使用keys指令会有什么问题？

​		这个时候你要回答redis关键的一个特性：redis的单线程的。keys指令会导致线程阻塞一段时间，线上服务会停顿，直到指令执行完毕，服务才能恢复。这个时候可以**使用scan指令**，scan指令可以无阻塞的提取出指定模式的key列表，但是会有一定的重复概率，在客户端做一次去重就可以了，但是整体所花费的时间会比直接用keys指令长。

​		不过，增量式迭代命令也不是没有缺点的： 举个例子， 使用 SMEMBERS 命令可以返回集合键当前包含的所有元素， 但是对于 SCAN 这类增量式迭代命令来说， 因为在对键进行增量式迭代的过程中， 键可能会被修改， 所以增量式迭代命令只能对被返回的元素提供有限的保证 。

**使用过Redis做异步队列么，你是怎么用的？**

​		一般使用list结构作为队列，rpush生产消息，lpop消费消息。当lpop没有消息的时候，要适当sleep一会再重试。

**如果对方追问可不可以不用sleep呢**

​		list还有个指令叫blpop，在没有消息的时候，它会阻塞住直到消息到来。

**如果对方接着追问能不能生产一次消费多次呢？**

​		使用pub/sub主题订阅者模式，可以实现 1:N 的消息队列。

​		如果对方继续追问 pub/su b有什么缺点？

​		在消费者下线的情况下，**生产的消息会丢失，得使用专业的消息队列如RocketMQ等**。

**如果对方究极TM追问Redis如何实现延时队列？**

​		这一套连招下来，我估计现在你很想把面试官一棒打死（面试官自己都想打死自己了怎么问了这么多自己都不知道的），如果你手上有一根棒球棍的话，但是你很克制。平复一下激动的内心，然后神态自若的回答道：使用sortedset，拿时间戳作为score，消息内容作为key调用zadd来生产消息，消费者用zrangebyscore指令获取N秒之前的数据轮询进行处理。

​		到这里，面试官暗地里已经对你竖起了大拇指。并且已经默默给了你A+，但是他不知道的是此刻你却竖起了中指，在椅子背后。

**Redis是怎么持久化的？服务主从数据怎么交互的？**

​		RDB做镜像**全量持久化**，AOF做**增量持久化**。因为RDB会耗费较长时间，不够实时，在停机的时候会导致大量丢失数据，所以需要AOF来配合使用。在redis实例重启时，会使用RDB持久化文件重新构建内存，再使用AOF重放近期的操作指令来实现完整恢复重启之前的状态。

​		这里很好理解，把RDB理解为一整个表全量的数据，AOF理解为每次操作的日志就好了，服务器重启的时候先把表的数据全部搞进去，但是他可能不完整，你再回放一下日志，数据不就完整了嘛。不过Redis本身的机制是 AOF持久化开启且存在AOF文件时，优先加载AOF文件；AOF关闭或者AOF文件不存在时，加载RDB文件；加载AOF/RDB文件城后，Redis启动成功； AOF/RDB文件存在错误时，Redis启动失败并打印错误信息

**对方追问那如果突然机器掉电会怎样？**

​		取决于AOF日志sync属性的配置，如果不要求性能，在每条写指令时都sync一下磁盘，就不会丢失数据。但是在高性能的要求下每次都sync是不现实的，一般都使用定时sync，比如1s1次，这个时候最多就会丢失1s的数据。

**对方追问RDB的原理是什么？**

​		你给出两个词汇就可以了，fork和cow。fork是指redis通过创建子进程来进行**RDB操作**，cow指的是copy on write，子进程创建后，父子进程共享数据段，父进程继续提供读写服务，写脏的页面数据会逐渐和子进程分离开来。

​		注：回答这个问题的时候，如果你还能说出AOF和RDB的优缺点，我觉得我是面试官在这个问题上我会给你点赞，两者其实区别还是很大的，而且涉及到Redis集群的数据同步问题等等。想了解的伙伴也可以留言，我会专门写一篇来介绍的。

**Pipeline有什么好处，为什么要用pipeline？**

​		可以将多次IO往返的时间缩减为一次，前提是pipeline执行的指令之间没有**因果相关性**。使用redis-benchmark进行压测的时候可以发现影响redis的QPS峰值的一个重要因素是pipeline批次指令的数目。

**Redis的同步机制了解么？**

​		Redis可以使用主从同步，从从同步。第一次同步时，主节点做一次bgsave，并同时将后续修改操作记录到内存buffer，待完成后将RDB文件全量同步到复制节点，复制节点接受完成后将RDB镜像加载到内存。加载完成后，再通知主节点将期间修改的操作记录同步到复制节点进行重放就完成了同步过程。后续的增量数据通过AOF日志同步即可，有点类似数据库的binlog。

**是否使用过Redis集群，集群的高可用怎么保证，集群的原理是什么？**

​		Redis Sentinal着眼于高可用，在master宕机时会自动将slave提升为master，继续提供服务。

​		Redis Cluster着眼于扩展性，在单个redis内存不足时，使用Cluster进行分片存储。

面试结束

小伙子你可以的，什么时候有时间来上班啊，要不明天就来吧？

你强装镇定，这么急啊我还需要租房，要不下礼拜一吧。

好的 心想这小子这么NB是不是很多Offer在手上，不行我得叫hr给他加钱。

能撑到最后，你自己都忍不住自己给自己点个赞了（暗示点赞，每次都看了不点赞，你们想白嫖我么？你们好坏喲，不过我喜欢⁄(⁄ ⁄•⁄ω⁄•⁄ ⁄)⁄）。

**总结**

​		在技术面试的时候，不管是Redis还是什么问题，如果你能举出实际的例子，或者是直接说自己开发过程的问题和收获会给面试官的印象分会加很多，回答逻辑性也要强一点，不要东一点西一点，容易把自己都绕晕的。

​		还有一点就是我问你为啥用Redis你不要一上来就直接回答问题了，你可以这样回答：

​		帅气的面试官您好，首先我们的项目DB遇到了瓶颈，特别是秒杀和热点数据这样的场景DB基本上就扛不住了，那就需要缓存中间件的加入了，目前市面上有的缓存中间件有 Redis 和 Memcached ，他们的优缺点……，综合这些然后再结合我们项目特点，最后我们在技术选型的时候选了谁。

​		如果你这样有条不紊，有理有据的回答了我的问题而且还说出这么多我问题外的知识点，我会觉得你不只是一个会写代码的人，你逻辑清晰，你对技术选型，对中间件对项目都有自己的理解和思考，说白了就是你的offer有戏了。



