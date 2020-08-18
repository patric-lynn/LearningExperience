### MySQL相关知识

#### MySQL索引结构

##### 1.索引基础概念

​		索引，是数据库中专门用于帮助用户快速查询数据的一种**数据结构**。索引是表的目录，在查找内容之前可以先在目录中查找索引位置，以此**快速定位查询数据**。索引会保存在**额外的文件中**。类似于字典中的**目录**，查找字典内容时可以**根据目录查找到数据的存放位置**，然后直接获取即可。

###### 1.索引分类

-   **普通索引**：

    仅加速查询 最基本的索引，没有任何限制，是我们大多数情况下使用到的索引。

    ```mysql
    CREATE INDEX  index_name  on user_info(name)  ;
    
    1.1 创建普通索引
    
    1.1.1、创建表的时候同事创建索引
    
    create table healerjean (
      id   bigint(20)  NOT NULL AUTO_INCREMENT PRIMARY KEY COMMENT  '主键',
      name  VARCHAR(32) NOT NULL COMMENT '姓名',
      email VARCHAR(64) NOT NULL COMMENT  '邮箱',
      message text DEFAULT  NULL COMMENT '个人信息',
      INDEX index_name (name) COMMENT '索引name'
    ) COMMENT  = '索引测试表';
    
    1.1.2、在存在的表上创建索引
    
    create index index_name on healerjean(name)
    
    1.1.3、注意：
    
    对于创建索引时如果是blob 和 text 类型，必须指定length。
    
    create index ix_extra on in1(message(200));
    
    alter table employee add index emp_name (name);
    
    1.2、删除索引
    
    drop index_name on healerjean;
    
    alter TABLE users  drop index  name_index ;
    
    1.3、查看索引
    
    这个时候，我们会发现其实主键id也是一个索引
    show index from healerjean;
    
    ```

-   **唯一索引：**

    与普通索引类型，不同的是：**加速查询 + 列值唯一**（可以有**null**）

    ```
    CREATE UNIQUE INDEX  mail  on user_info(name)  ;
    ```

-   **全文索引：**

    全文索引（FULLTEXT）仅可以适用于**MyISAM引擎的数据表**；作用于CHAR、VARCHAR、TEXT数据类型列。

-   **组合索引：**

    将几个列作为一条索引进行检索，使用最左匹配原则。

###### **2.索引选取原则与场景**

-   1、**越小的**数据类型通常更好：越小的数据类型通常在磁盘、内存和CPU缓存中都需要更少的空间，处理起来更快。
-   2、**简单的**数据类型更好：整型数据比起字符，处理开销更小，因为字符串的比较更复杂。
-   3、尽量**避免NULL**：应该指定列为NOT nuLL,在MySQL中， 含有空值的列很难进行查询优化，因为它们使得索引、索引的统计信息以及比较运算更加复杂

**什么样的字段适合创建索引**

-   1、表的**主键、外键必须有索引**；外键是唯一的，而且经常会用来查询 
-   2、数据量**超过300的表**应该有索引；
-   3、经常与其他表**进行连接的表**，在**连接字段上应该建立索引**；经常连接查询，需要有索引 
-   4、经常**出现在Where子句中的字段**，加快判断速度，特别是大表的字段，应该建立索引，建立索引，一般用在select ……where f1 and f2 ，我们在f1或者f2上建立索引是没用的。只有两个使用联合索引才能有用
-   5、经常用到**排序的列上**，因为索引已经排序。
-   6、经常用在范围内搜索的列上创建索引，因为索引已经排序了，其指定的范围是连续的

**什么场景不适合创建索引**

-   第一，对于那些在查询中**很少使用或者参考的列不应该创建索引**。这是因 为，既然这些列很少使用到，因此有索引或者无索引，并不能提高查询速度。相反，由于增加了索引，反而降低了系统的维护速度和增大了空间需求。
-   第二，对于那些**只有很少数据值的列**也不应该增加索引。因为本来结果集合就是相当于全表查询了，所以没有必要。这是因为，由于这些列的取值很少，例如人事表的性别列，在查询的结果中，结果集的数据行占了表中数据行的很大比例，即需要在表中搜索的数据行的比例很大。增加索引，**并不能明显加快检索速度**。
-   第三，对于那些定义为**text, image和bit数据类型的列不应该增加索引**。这是因为，这些列的数据量要么相当大，要么取值很少。
-   第四，当**修改性能远远大于检索性能时，不应该创建索引**。这是因为，修改性能和检索性能是互相矛盾的。当增加索引时，会提高检索性能，但是会降低修改性能。当减少索引时，会提高修改性能，降低检索性能。因 此，当修改性能远远大于检索性能时，不应该创建索引。
-   第五，不会出现在where条件中的**字段不该建立索引**。



###### 3.索引优缺点

**索引优点**

-   索引由数据库中一列或多列组合而成，其作用是提高对表中数据的查询速度
-   索引的优点是可以提高检索数据的速度

**索引缺点**

-   索引的缺点是**创建和维护索引需要耗费时间**，索引可以提高查询速度，会减慢写入速度
-   索引并不是越多越好，索引固然可以提高**相应的 select 的效率**，但同时也**降低了 insert 及 update 的效率**，因为 insert 或 update 时有可能会**重建索引**，所以怎样建索引需要慎重考虑，视具体情况而定。一个表的索引数**最好不要超过6个**，若太多则应考虑一些不常使用到的列上建的索引是否有必要。



###### 4.索引使用与不使用的情况

​	**索引正确使用的情况**

```mysql
一.对于创建的多列索引,只要查询的条件中用到了最左边的列,索引一般就会被使用:
1、首先按 company_id,moneys 的顺序创建一个复合索引,具体如下:

mysql> create index ind_sales2_companyid_moneys on sales2(company_id,moneys);
Query OK, 1000 rows affected (0.03 sec)
Records: 1000 Duplicates: 0 Warnings: 0

2、然后按 company_id 进行表查询,具体如下:

mysql> explain select * from sales2 where company_id = 2006\G;
*************************** 1. row ***************************
id: 1
select_type: SIMPLE
table: sales2
type: ref
possible_keys: ind_sales2_companyid_moneys
208key: ind_sales2_companyid_moneys
key_len: 5
ref: const
rows: 1
Extra: Using where
1 row in set (0.00 sec)

3、可以发现即便 where 条件中不是用的 company_id 与 moneys 的组合条件,索引仍然能用到,这就是索引的前缀特性。

4、但是如果只按 moneys 条件查询表,那么索引就不会被用到,具体如下:
mysql> explain select * from sales2 where moneys = 1\G;
*************************** 1. row ***************************
id: 1
select_type: SIMPLE
table: sales2
type: ALL
possible_keys: NULL
key: NULL
key_len: NULL
ref: NULL
rows: 1000
Extra: Using where
1 row in set (0.00 sec)



二.对于使用 like 的查询,后面如果是常量并且只有%号不在第一个字符,索引才可能会被使用：
1、 可以发现第一个例子没有使用索引,而第二例子就能够使用索引,
2、 区别就在于“%”的位置不同,前者把“%”放到第一位就不能用到索引,而后者没有放到第一位就使用了索引。
mysql> explain select * from company2 where name like '%3'\G;
*************************** 1. row ***************************
id: 1
select_type: SIMPLE
table: company2
type: ALL
possible_keys: NULL
key: NULL
key_len: NULL
ref: NULL
rows: 1000
Extra: Using where
1 row in set (0.00 sec)

mysql> explain select * from company2 where name like '3%'\G;
*************************** 1. row ***************************
id: 1
select_type: SIMPLE
table: company2
type: range
209possible_keys: ind_company2_name
key: ind_company2_name
key_len: 11
ref: NULL
rows: 103
Extra: Using where
1 row in set (0.00 sec)

三.如果列名，记得是列的名字，是索引,使用 column_name is null 将使用索引。
mysql> explain select * from company2 where name is null\G;
*************************** 1. row ***************************
id: 1
select_type: SIMPLE
table: company2
type: ref
possible_keys: ind_company2_name
key: ind_company2_name
key_len: 11
ref: const
rows: 1
Extra: Using where
1 row in set (0.00 sec)

四.如果对大的文本进行搜索,使用全文索引而不用使用 like ‘%…%’。
```

​	**有索引但不使用的情况**

```mysql
一、如果 MySQL 估计使用索引比全表扫描更慢,则不使用索引。例如，如果列key_part1 均匀分布在 1 和 100 之间,下列查询中使用索引就不是很好:

SELECT * FROM table_name where key_part1 > 1 and key_part1 < 90;

二、如果使用 MEMORY/HEAP 表并且 where 条件中不使用“=”进行索引列,那么不会用到索引。heap 表只有在“=”的条件下才会使用索引。

三、用 or 分割开的条件,如果 or 前的条件中的列有索引,而后面的列中没有索引,那么涉及到的索引都不会被用到,例如:，必须or前后都有索引才能被使用，而且必须是单独索引。

mysql> show index from sales\G;
*************************** 1. row ***************************
Table: sales
Non_unique: 1
Key_name: ind_sales_year
Seq_in_index: 1
Column_name: year
210Collation: A
Cardinality: NULL
Sub_part: NULL
Packed: NULL
Null:
Index_type: BTREE
Comment:
1 row in set (0.00 sec)

四、如果列是字符型，传入的是数字，则不上‘’不会使用索引

mysql> explain select * from company2 where name = 294\G;
*************************** 1. row ***************************
id: 1
select_type: SIMPLE
table: company2
type: ALL
possible_keys: ind_company2_name
key: NULL
key_len: NULL
ref: NULL
rows: 1000
Extra: Using where
1 row in set (0.00 sec)
 
 
mysql> explain select * from company2 where name = '294'\G;
*************************** 1. row ***************************
id: 1
select_type: SIMPLE
table: company2
type: ref
possible_keys: ind_company2_name
key: ind_company2_name
key_len: 23
ref: const
rows: 1
Extra: Using where
1 row in set (0.00 sec)
```



##### 2.索引数据结构

###### 1.平衡二叉树:

​		平衡二叉树在数据结构中属于比较好理解的部分，按照键值的大小将索引存储在二叉树合适的位置，具体的存储方法就和平衡二叉树的存储是一样的。需要注意左右子树的高度差，进行二叉树的旋转保证二叉树的平衡。

###### 2.B树

​		B树的每个结点内部都要存储一个指向关键字代表的具体数据的指针，因为每个盘块的大小是固定的，这样使得每个结点的度相对应较小，如果每个磁盘只存储对应的主键值或者只存储指向具体数据的指针这样可以解决存储空间。B树的存储方式很接近二叉树，这样会导致有些数据的查找较慢，比如有些数据的深度较大。采用B+树的方式使得查找更加稳定。

###### 3.B+树

​		B+树很好的弥补了B树的一些缺点提高了的查找效率和存储效率。

###### 4.Hash索引

​		和数据结构中的hash查找类似，通过计算关键字的hash值查找该数据对应的地址，可以理解为通过hash值并对hash值进行相应的处理确定数据的存储位置，如果发生数据冲突，解决的办法为链地址法。





#### MySQL存储引擎

##### 1.MySQL两种存储引擎

###### 1.InnoDB(聚簇索引)

​		InnoDB的一个数据表中，它的**索引文件和数据文件是同一个**，也就是说，在查询时只要找到了索引就找到了数据文件。在InnoDB中既存储**主键索引值**，又存储**行数据**，叫做**聚簇索引** 。聚簇索引**会将磁盘数据按照一列或者多列的值进行排序**，通常按照**主键一列的值**进行排序的。聚簇索引的特点是**存储的顺序和索引的顺序一致**，一张表中只存在一个聚簇索引。

​		**InnoDB索引的实现**

​	    与 MyISAM相同的一点是，InnoDB **也采用 B+Tree**这种数据结构来**实现 BTree索引**。而很大的区别在于，InnoDB 存储引擎采用“**聚集索引**”的数据存储方式实现B-Tree索引，所谓“聚集”，就是指**数据行和相邻的键值紧凑地存储在一起**，注意 InnoDB **只能聚集一个叶子页（16K）的记录**（即聚集索引**满足一定的范围的记录**），因此包含相邻键值的记录可能会相距甚远。注意: 对innodb来说：

-   1: **主键索引** 既存储索引值,又在叶子中存储行的数据 
-   2: 如果没有主键, 则会用**Unique key**做主键 
-   3: 如果没有unique,则系统**生成一个内部的rowid做主键**. 
-   4: 像innodb中,主键的索引结构中,既存储了主键值,又存储了行数据,这种结构称为”**聚簇索引**”

 	 下图说明了 InnoDB聚集索引的实现方式，同时也体现了一张 innoDB表的结构，InnoDB 中，主键索引和数据是一体的，没有分开： 

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200401014725328.png" alt="image-20200401014725328" style="zoom:40%;" />

​		这种实现方式，给予了 InnoDB **按主键检索的超高性能**。可以**有目的性地选择聚集索引**，比如一个邮件表，可以选择用户ID来聚集数据，这样只需要从磁盘**读取较少并且连续的数据页**就能获得某个id的用户全部的邮件，**避免了读取分散页时所耗费的随机I/O**。

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200401014751271.png" alt="image-20200401014751271" style="zoom:42%;" />

​		 而对于辅助索引，InnoDB采用的方式是**在叶子页中保存主键值**，通过这个**主键值来回表**（上图）查询到一条完整记录，因此按辅助索引检索**实际上进行了二次查询**，效率肯定是没有按照主键检索高的。由于每个辅助索引都**包含主键索引**，因此，为了减小辅助索引所占空间，我们通常希望 InnoDB 表中的**主键索引尽量定义得小一些**（值得一提的是，MyISAM会使用**前缀压缩技术**使得索引变小，而InnoDB按照**原数据格式**进行存储），并且希望InnoDB的主键是**自增长的**，因为如果主键并非自增长，插入时，由于写入是乱序的，会使得插入效率变低。



###### 2.MyIsam（非聚簇索引）

​		**非聚簇索引**也就是数据和索引**分开存储**的。

​		**MyISAM索引的实现**

   	MyISAM**索引文件和数据文件**是**分离的**，索引文件**仅保存记录所在页的指针**（物理位置），通过这些地址来读取页，进而读取被索引的行。下图是MyISAM的索引原理图：（为了简化，一个页内只存放了两条记录。）

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200401012745388.png" alt="image-20200401012745388" style="zoom:40%;" />

​		上图所提供的示例表字段有**Col1（ID）、Col2(age)、Col3（name）**三个，其中Col1为**Primary Key**（主键），上图很好地说明了树中叶子保存的是对应行的物理位置。通过该值，存储引擎能顺利地进行**回表查询**，得到一行完整记录。同时，每个叶子页也**保存了指向下一个叶子页的指针**。从而方便叶子节点的范围遍历。 而对于二级索引，在 MyISAM存储引擎中以与上图同样的方式实现，可以看出MyISAM的索引文件**仅仅保存数据记录的地址**。



###### 3.两种引擎总结

​		**1、关于innoDB中索引的使用**

 	   了解不同**存储引擎**的索引实现方式对于正确使用和**优化索引**都非常有帮助，例如知道了InnoDB的索引实现后，就很容易明白为什么**不建议使用过长的字段作为主键**，因为所有辅助索引都**引用主索引**，过长的主索引会令辅助索引变得过大。再例如，用非单调的字段作为主键在InnoDB中不是个好主意，因为InnoDB数据文件本身是一棵B+Tree，**非单调的主键会造成在插入新记录时数据文件为了维持B+Tree的特性而频繁的分裂调整，十分低效**，而使用**自增字段作为主键**则是一个很好的选择。

​		**2、什么时候选用myisam**

 	   myisam的主键索引的叶子节点**只存放数据在物理磁盘上的指针**，其他次索引也是一样的； 

-   innodb的主键索引的**叶子节点下面直接存放数据**，其他次索引的叶子节点**指向主键id**； 
-   由此可以挖掘出一个问题，就是如果Innodb有大数据列，比如 varchar(300)，这种比较多的话，那么排序的时候用主键id排序会比较慢，因为id主键下面放着所有数据列，而Myisam就**不需要扫描数据列**，要解决这个问题的话可以再**建一个和主键id一起的联合索引**；

 	  MyISAM表索引**在处理文本索引时更具优势**，而INNODB表索引在**其它类型上更具效率优势**。比如全文索引一般在CHAR、VARCHAR或TEXT列上创建，MyISAM表支持而INNODB表不支持，常见**主要针对文本进行索引**。同时MySQL**高并发需要事务场景时，只能使用INNODB表**。

​		**3、该如何选用两个存储引擎呢**

​	    因为MyISAM相对简单所以在效率上要优于InnoDB。如果系统**读多，写少，对原子性要求低**，那么MyISAM最好的选择。且MyISAM恢复速度快。可**直接用备份覆盖恢复**。 如果系统**读少，写多的时候**，尤其是**并发写入高**的时候。InnoDB就是首选了。 两种类型都有自己优缺点，选择那个完全要看自己的实际类弄。



###### 3.联合索引 

​		**最左匹配原则**，按照联合索引进行查找的原则是，查找字段的关键字中必须**按照索引设定顺序出现**才能**按照索引进行查找**。也就是说查找的关键字序列中出现了某一个关键字，但是该关键字在索引中的前一个关键字并未出现，则不能按照索引查找。比如我为一个字段设置了ABC三个关键字为联合索引。那么当我的查询条件出现了BC而没有写出A，那么**将无法使用的索引的方式进行查询**。




##### 2.InnoDB和MyISAM的区别

​		mysql支持很多表类型的表（即存储引擎），如myisam、innodb、memory、archive、example等。每种存储引擎都有自己的优点和缺点，充分的理解每种存储引擎，有助于合理的使用它们。有人认为在同一个数据库中使用多种存储引擎很影响性能，其实这是一种十分错误的想法。实际上，除非是非常简单的数据库，否则的话，只使用一种存储引擎，对应用程序的性能来说是一个十分糟糕的行为。对数据库了解的人会**根据每张表的作用不同来选择适当的存储引擎，这才是正确的做法**。mysql的存储引擎很多，但是在工作中运用最多的存储引擎有两个，一个是InnoDB,一个是MyISAM。这里就聊聊这两个存储引擎，并比较下两者之间的区别。

###### 1.MyISAM

​		MyISAM现在为mysql的**默认存储引擎**，如果在建表的时候，没有指定表类型，mysql就会默认使用MyISAM。MyISAM有一个很多人认为很重要，然而我却觉得没什么卵用的特性，那就是MyISAM表示**独立于操作系统之外**的。通俗点说就是你**可以很轻松的将MyISAM表从windows移植到linux或者从linux移植到windows**。这确实是个优点，可是哪个技术主管会有在决定用了一种操作系统一段时间后又要换服务器的操作系统的脑残行为，所有我认为这个优点没有什么实际的意义。

​		MyISAM存储引擎是mysql组件中非常重要的一部分，在mysql中可以创建3种MyISAM格式的表——**静态、动态和压缩**。格式不需要单独指定，mysql会根据表结构自动选择最合适的格式。

1、**MyISAM静态**

​		如果表的每个字段的数据类型的定义都是使用静态的（如char），mysql就会**自动使用静态MyISAM格式**，这种类型格式的表的**性能是很高的**，也就是查询更新用的时间很少，但要知道这是**在牺牲空间为代价**。因为每一列都要分配最大的空间，即使有部分空间没有用到，这就使得静态的表所占的空间会比较大。

2、**MyISAM动态**

​		如果表的每个字段的数据类型的定义**都是使用动态的**（如varchar），mysql就会自动使用动态MyISAM格式，这种类型格式的表的**性能会有所下降**，但是它的空间占有要比静态的少很多。

3、**MyISAM压缩**

​		如果有一张表在设计之初只赋予了它读的使命，就可以用MyISAM压缩表，在相同的配置下，它的性能是最快的。



###### **2.InnoDB**

​		InnoDB是一个**事务型存储引擎**，它**默认支持事务**。因此，在对数据完整度有较高要求的项目中，都是采用InnoDB作为存储引擎，比如跟银行相关的项目。InnoDB**还引入行级锁定**。其他表类型都是全表锁定，意思就是说，在一个用户要修改一张表中，自己那一行的某个信息的时候，数据库会先锁定整张表，其他用户对这张表无法进行操作，这就是**全表锁定**。行级锁定就不一样了，**只锁定自己要修改的那一行的**，也就是说其他人对表还可以进行操作，只是不能操作被锁定的那一行的数据了。好处显而易见，在速度上更快了，**特别适合处理多重并发的更新请求。InnoDB还引入了外键约束**。



###### 3.两种引擎的主要区别

**1.MyISAM：**

​		每个MyISAM在磁盘上存储成三个文件。第一个文件的名字以表的名字开始，扩展名指出文件类型。**.frm文件存储表定义**。数据文件的**扩展名为.MYD (MYData)**。MyISAM表格可以被压缩，而且它们支持全文搜索。**不支持事务**，而**且也不支持外键**。如果**事务回滚将造成不完全回滚**，**不具有原子性**。在进行updata时进行**表锁**，**并发量相对较小**。如果执行大量的SELECT，**MyISAM是更好的选择**。

​		MyISAM的索引和数据是分开的，并且**索引是有压缩的**，内存使用率就对应提高了不少。能加载更多索引，而Innodb是**索引和数据是紧密捆绑的**，没有使用压缩从而会造成**Innodb比MyISAM体积庞大不小**。MyISAM**缓存在内存的是索引**，不是数据。而InnoDB**缓存在内存的是数据**，相对来说，服务器内存越大，InnoDB发挥的优势越大。

**优点：**查询数据相对较快，适合**大量的select**，可以**全文索引**。

**缺点：**不支持事务，不支持外键，**并发量较小**，不适合大量update



**2.InnoDB：**

​		这种类型是**事务安全的**。它与BDB类型具有相同的特性,它们还**支持外键**。具有比BDB还丰富的特性,因此如果需要一个事务安全的存储引擎，建议使用它。**在update时表进行行锁，并发量相对较大**。如果你的数据**执行大量的INSERT或UPDATE，出于性能方面的考虑，应该使用InnoDB表**。

**优点：**支持事务，**支持外键**，并发量较大，**适合大量update**

**缺点：**查询数据相对较快，**不适合大量的select**

​		对于支持事务的InnoDB类型的表，影响速度的主要原因是AUTOCOMMIT默认设置是打开的，而且程序没有显式调用BEGIN 开始事务，导致每插入一条都自动Commit，严重影响了速度。**可以在执行sql前调用begin，多条sql形成一个事务（即使autocommit打开也可以），将大大提高性能。**

**基本的差别为**：MyISAM类型**不支持事务处理等高级处理**，而InnoDB类型支持。MyISAM类型的表强调的是**性能**，其执行数度比InnoDB类型**更快**，而InnoDB提供**事务支持**以及**外键**等高级数据库功能。



###### 4.具体实现细节的差别

- 1.InnoDB不支持**FULLTEXT类型的索引**。
- 2.InnoDB 中不保存表的具体行数，也就是说，执行select count(\*) from table时，InnoDB要扫描一遍整个表来计算有多少行，但是MyISAM只要**简单的读出保存好的行数** 即可。注意的是，当count(*)语句包含 where条件时，两种表的操作是一样的。
- 3.对于AUTO_INCREMENT类型的字段，InnoDB中必须**包含只有该字段的索引**，但是在MyISAM表中，可以和其他字段一起建立联合索引。
- 4.DELETE FROM table时，InnoDB不会重新建立表，而是**一行一行的删除**。
- 5.LOAD TABLE FROM MASTER操作对InnoDB是不起作用的，解决方法是首先把InnoDB表改成MyISAM表，导入数据后再改成InnoDB表，但是对于使用的额外的 InnoDB特性（例如外键）的表不适用。

​		另外，InnoDB表的**行锁也不是绝对的**，如果在执行一个SQL语句时**MySQL不能确定要扫描的范围**，**InnoDB表同样会锁全表**，例如update table set num=1 where name like "%aaa%"。任何一种表都不是万能的，只用恰当的针对业务类型来选择合适的表类型，才能最大的发挥MySQL的性能优势。



###### **5.MyISAM与InnoDB如何选择**

​		MYISAM和INNODB是Mysql数据库提供的**两种存储引擎**。两者的优劣可谓是**各有千秋**。INNODB会支持一些关系数据库的高级功能，如事务功能和行级锁，MYISAM不支持。MYISAM的性能更优，占用的存储空间少。所以，选择何种存储引擎，视具体应用而定。

​		如果你的应用程序**一定要使用事务，毫无疑问你要选择INNODB引擎**。但要注意，INNODB的行级锁是有条件的。在**where条件没有使用主键时，照样会锁全表**。比如DELETE FROM mytable这样的删除语句。

​		如果你的应用程序对查询性能要求较高，就要使用MYISAM了。**MYISAM索引和数据是分开的**，而且其索引是压缩的，可以更好地利用内存。所以它的**查询性能明显优于INNODB**。压缩后的索引也能节约一些磁盘空间。MYISAM拥有**全文索引的功能**，这可以极大地优化LIKE查询的效率。

​		有人说MYISAM只能用于小型应用，其实这只是一种偏见。如果数据量比较大，这是需要通过升级架构来解决，比如**分表分库**，而不是**单纯地依赖存储引擎**。

​		**其他一些说法：**现在一般都是选用innodb了，主要是myisam的全表锁，读写串行问题，并发效率锁表，效率低myisam对于读写密集型应用一般是不会去选用的。

​		关于Mysql数据库**默认的存储引擎**：

​		MyISAM和InnoDB是MySQL的两种存储引擎。如果是默认安装，那就应该是InnoDB，你可以在my.ini文件中找到default-storage-engine=INNODB；当然你可以在建表时指定相应的存储引擎。通过show create table xx 可以看见相应信息。





##### .数据库事务原子性、一致性是怎样实现的？

​		在数据库实现的场景中，一致性可以分为数据库**外部的一致性**和**数据库内部的一致性**。前者由外部应用的编码来保证，即某个应用在执行转帐的数据库操作时，必须在**同一个事务内部调用对帐户A和帐户B的操作**。如果在这个层次出现错误，这**不是数据库本身能够解决**的，也不属于我们需要讨论的范围。后者由数据库来保证，即在同一个事务内部的一组操作**必须全部执行成功（或者全部失败）**。这就是**事务处理的原子性**。

​		**利用提交点和日志来保证**：日志为了实现原子性，需要**通过日志**：将所有对数据的**更新操作都写入日志**，如果一个事务中的一部分操作已经成功，但以后的操作，由于断电/系统崩溃/其它的软硬件错误而无法继续，则通过**回溯日志**，将已经**执行成功的操作撤销**，从而达到“**全部操作失败**”的目的。

​		最常见的场景是，数据库系统**崩溃后重启**，此时数据库**处于不一致的状态**，必须先执行一个crash recovery的过程：读取日志**进行REDO**（重演**将所有已经执行成功但尚未写入到磁盘的操作**，保证持久性），再**对所有到崩溃时尚未成功提交的事务进行UNDO**（撤销**所有执行了一部分但尚未提交的操作**，保证原子性）。crash recovery结束后，数据库**恢复到一致性状态**，可以继续被使用。

 		日志的**管理和重演**是数据库实现中最复杂的部分之一。如果涉及到**并行处理和分布式系统**（日志的复制和重演是数据库高可用性的基础），会比上述场景还要复杂得多。

 		但是，原子性并不能完全保证一致性。**在多个事务并行进行的情况下，即使保证了每一个事务的原子性，仍然可能导致数据不一致的结果。**例如，事务1需要将100元转入帐号A：先读取帐号A的值，然后在这个值上加上100。但是，在这两个操作之间，另一个事务2修改了帐号A的值，为它增加了100元。那么最后的结果应该是A增加了200元。但事实上， 事务1最终完成后，帐号A只增加了100元，因为事务2的修改结果被事务1覆盖掉了。

 		为了保证并发情况下的一致性，引入了**隔离性**，即保证每一个事务能够看到的数据总是一致的，就好象其它并发事务并不存在一样。用术语来说，就是多个事务并发执行后的状态，和它们串行执行后的状态是等价的。怎样实现隔离性，已经有很多人回答过了，原则上**无非是两种类型的锁**：

 		一种是**悲观锁**，即当前事务将**所有涉及操作的对象加锁**，操作完成后释放给其它对象使用。为了尽可能提高性能，发明了**各种粒度**（数据库级/表级/行级……）/**各种性质**（共享锁/排他锁/共享意向锁/排他意向锁/共享排他意向锁……）的锁。为了解决死锁问题，又发明了**两阶段锁协议/死锁检测**等一系列的技术。

​		一种是**乐观锁**，即不同的事务可以同时看到**同一对象（一般是数据行）的不同历史版本**。如果有两个事务同时修改了同一数据行，那么在较晚的事务**提交时进行冲突检测**。实现也有两种，一种是通过**日志UNDO的方式**来获取数据行的历史版本，一种是**简单地在内存中保存同一数据行的多个历史版本**，通过时间戳来区分。

 		锁也是数据库实现中最复杂的部分之一。同样，如果涉及到分布式系统（分布式锁和两阶段提交是分布式事务的基础），会比上述场景还要复杂得多。数据库有自己的并发控制和锁问题，虽然在原理上和操作系统中的概念非常类似，但是并不是同一个层次上的东西。数据库中的锁，在粒度/类型/实现方式上和操作系统中的锁都完全不同。操作系统中的锁，在数据库实现中称为latch（一般译为闩）。其他回答者回答的其实是“在并行事务处理的情况下怎样保证数据的一致性”。

 		最后回到原来的问题（“原子性、一致性的实现机制是什么”）。我手头有本Database System Concepts（4ed，有点老了），在第15章的开头简明地介绍了ACID的概念及其关系。如果你想从**概念上**了解其实现，把这本书的相关章节读完应该能大概明白。如果你想从**实践上**了解其实现，可以找innodb这样的开源引擎的源代码来读。不过，即使是一个非常粗糙的开源实现（不考虑太复杂的并行处理，不考虑分布式系统，不考虑针对操作系统和硬件的优化之类），要基本搞明白恐怕也不是一两年的事。





#### MySQL基础框架-面试

##### 1.基本架构

> 你知道MySQL的基本架构么？你能在纸上给我大致画出这个示意图么？

<img src="/Users/xiaoxiangyuzhu/Pictures/Typora%20Images/image-20200511232427016.png" alt="image-20200511232427016" style="zoom:40%;" />

##### 2.连接器是什么

​		我们要进行查询，第一步就是先去链接数据库，那这个时候就是**连接器**跟我们对接。他负责跟客户端建立链接、获取权限、维持和管理连接。链接的时候会经过TCP握手，然后身份验证，然后我们输入用户名密码就好了。验证ok后，我们就连上了这个MySQL服务了，但是这个时候我们处于空闲状态。

​		**怎么查看空闲连接列表？**

​		**show processlist**，下图就是我在自己的数据库表执行命令的结果，其中的Command列显示为Sleep的这一行，就表示现在系统里面有一个空闲连接。

![img](https://tva1.sinaimg.cn/large/006tNbRwly1gbmizyzijbj30vq04utb4.jpg)


​		这里需要注意的是，我们数据库的客户端太久没响应，连接器就会自动断开了，这个时间参数是wait_timeout控制住的，默认时长为8小时。断开后重连的时候会报错，如果你想再继续操作，你就需要重连了。

​		这个有个我看过的书本的案例：

```
一个在政府里的朋友说，他们的系统很奇怪，每天早上都得重启一下应用程序，否则就提示连接数据库失败，他们都不知道该怎么办。
我分析说，按照这个错误提示，应该就是连接时间过长了，断开了连接。数据库默认的超时时间是8小时，而你们平时六点下班，下班之后系统就没有人用了，等到第二天早上九点甚至十点才上班，这中间的时间已经超过10个小时了，数据库的连接肯定就会断开了。是的，就是超出了超时时间，然后写代码的人也没注意到这个细节，所以才会出现这个问题。把超时时间改得长一点，问题就解决了。
```

​		这种参数其实我们平时不一定能接触到，但是真的遇到问题的时候，知道每个参数的大概用法，不至于让你变成无头苍蝇。那除了重新链接，还有别的方式么？因为建立链接还是比较麻烦的。**使用长连接。**

​		但是这里有个缺点，使用长连接之后，内存会飙得很快，我们知道MySQL在执行过程中临时使用的内存是管理在连接对象里面的。

​		只有在链接断开的时候才能得到释放，那如果一直使用长连接，那就会导致OOM（Out Of Memory），会导致MySQL重启，在JVM里面就会导致**频繁的Full GC**。

​		那你会怎么解决？
​		我一般会定期断开长连接，使用一段时间后，或者程序里面判断执行过一个占用内存比较大的查询后就断开连接，需要的时候重连就好了。

​		还有别的方法么？你这种感觉不优雅呀小老弟。
​		执行比较大的一个查询后，执行mysql_reset_connection可以重新**初始化连接资源**。这个过程相比上面一种会好点，不需要重连，但是会初始化连接的状态。

##### 3.查询缓存是什么

​		MySQL拿到一个查询请求后，会先到查询缓存看看，之前是不是执行过这条语句。大家是不是好奇同一条语句在MySQL执行两次，第一次和后面的时间是不一样的，后者明显快一些，这就是因为缓存的存在。

​		MySQL跟Redis一样，只要是你之前执行过的语句，**都会在内存里面用key-value形式存储着**。查询的时候就会拿着语句先去缓存中查询，如果能够命中就返回缓存的value，如果不命中就执行后面的阶段。但是我还是不喜欢用缓存，**因为缓存弊大于利**。哦？此话怎讲？**缓存的失效很容易**，只要对表有任何的更新，这个表的所有查询缓存就会全部被清空，就会出现缓存还没使用，就直接被清空了，或者积累了很多缓存准备用来着，但是一个更新打回原形。这就导致查询的命中率低的可怕，只有那种**只查询不更新的表适用缓存**，但是这样的表往往很少存在，一般都是什么**配置表**之类的。

​		**那我们查询的时候不想用缓存一般都是怎么操作的，或者是用缓存又怎么操作？**可以显示调用，把query_cache_type设置成为DEMAND，这样SQL默认不适用缓存，想用缓存就用**SQL_CACHE**。有个小技巧就是，我们之前开发的时候，都会去库里看看sql执行时间，但是可能是有缓存的，一般我们就在sql前面使用SQL_NO_CACHE就可以知道真正的查询时间了。

```mysql
 select SQL_NO_CACHE * from B
```


​		缓存在MySQL8.0之后就取消了，所以大家现在应该不需要太关注这个问题，主要是我之前用的版本都不高，所以缓存一直有，在《高性能MySQL》书中也看到了一些关于缓存的介绍，就想起来给大家也提一下了。

##### **4.分析器是什么**

​		在缓存没有命中的情况下，就开始执行语句了，你写的语句有没有语法错误，这是接下来MySQL比较关心的点。

​		那他会怎么做呢？会先做**词法分析**，你的语句有这么多单词、空格，MySQL就需要识别每个字符串所代表的是什么，是关键字，还是表名，还是列名等等。然后就开始**语法分析**，根据词法分析的结果，语法分析会判断你sql的对错，错了会提醒你的，并且会提示你哪里错了。

##### 5.优化器和执行器是什么

​		**主要是优化什么呢？**
​		优化就比较简单了，因为我们建立表可能会建立很多索引，优化有一步就是要**确认使用哪个索引**，比如使用你的主键索引，联合索引还是什么索引更好。还有就是**对执行顺序进行优化**，条件那么多，先查哪个表，还是先关联，会出现很多方案，最后由优化器决定选用哪种方案。最后就是执行了.

​		**执行就交给执行器去做**。

​		第一步可能就是**权限的判断**，其实这里我不确定的一个点是，很多公司都是**自研的线上查询系统**，我们是不能用Navicat直连线上库，只能去网页操作，那表的权限是在MySQL层做的，还是系统做的，我猜应该是系统层做的，**MySQL可能默认就全开放了**，只是我们 不知道ip。执行的时候，就**一行一行的去判断是否满足条件**，有索引的执行起来可能就好点，一行行的判断就像是接口都提前在引擎定义好了，所以他比较快。数据库的慢日志有个**rows_examined字段**，扫描多少行可以看到，还有explain也可以看到执行计划，我们扫描了多少行。可以小伙子，基础大致框架还是了解得很清楚的，我们下次深入了解下，**索引和部分机制**。



##### 6.如何防止SQL注入

**一、SQL注入简介**

​		SQL注入是比较常见的**网络攻击方式之一**，它不是利用操作系统的BUG来实现攻击，而是针对程序员编程时的疏忽，通过SQL语句，实现**无帐号登录，甚至篡改数据库**。

**二、SQL注入攻击的总体思路**

1.寻找到SQL注入的位置

2.判断服务器类型和后台数据库类型

3.针对不通的服务器和数据库特点进行SQL注入攻击

**三、SQL注入攻击实例**

比如在一个登录界面，要求输入用户名和密码：

可以这样输入实现免帐号登录：

用户名： ‘or 1 = 1 –

密 码：

​		点登陆,如若没有做特殊处理,那么这个非法用户就很得意的登陆进去了.(当然现在的有些语言的数据库API已经处理了这些问题)

这是为什么呢? 下面我们分析一下：

从理论上说，后台认证程序中会有如下的SQL语句：

String sql = "select * from user_table where username=

' "+userName+" ' and password=' "+password+" '";

当输入了上面的用户名和密码，上面的SQL语句变成：

SELECT * FROM user_table WHERE username=

'’or 1 = 1 -- and password='’

分析SQL语句：

条件后面username=”or 1=1 用户名等于 ” 或1=1 那么这个条件一定会成功；

然后后面加两个-，这意味着注释，它将后面的语句注释，让他们不起作用，这样语句永远都能正确执行，用户轻易骗过系统，获取合法身份。

这还是比较温柔的，如果是执行

SELECT * FROM user_table WHERE

username='' ;DROP DATABASE (DB Name) --' and password=''

….其后果可想而知… 

**四、应对方法**

下面我针对JSP，说一下应对方法：

**1.（简单又有效的方法）PreparedStatement**

采用预编译语句集，它内置了处理SQL注入的能力，只要使用它的setXXX方法传值即可。

使用好处：

(1).代码的可读性和可维护性.

(2).PreparedStatement尽最大可能提高性能.

(3).最重要的一点是极大地提高了安全性.

原理：

sql注入只对sql语句的准备(编译)过程有破坏作用

而PreparedStatement已经准备好了,执行阶段只是把输入串作为数据处理,

而不再对sql语句进行解析,准备,因此也就避免了sql注入问题. 

**2.使用正则表达式过滤传入的参数**

要引入的包：

import java.util.regex.*;

正则表达式：

private String CHECKSQL = “^(.+)\\sand\\s(.+)|(.+)\\sor(.+)\\s$”;

判断是否匹配：

Pattern.matches(CHECKSQL,targerStr);

下面是具体的正则表达式：

检测SQL meta-characters的正则表达式 ：

/(\%27)|(\’)|(\-\-)|(\%23)|(#)/ix

修正检测SQL meta-characters的正则表达式 ：/((\%3D)|(=))[^\n]*((\%27)|(\’)|(\-\-)|(\%3B)|(:))/i

典型的SQL 注入攻击的正则表达式 ：/\w*((\%27)|(\’))((\%6F)|o|(\%4F))((\%72)|r|(\%52))/ix

检测SQL注入，UNION查询关键字的正则表达式 ：/((\%27)|(\’))union/ix(\%27)|(\’)

检测MS SQL Server SQL注入攻击的正则表达式：

/exec(\s|\+)+(s|x)p\w+/ix

等等…..

 

**3.字符串过滤**

比较通用的一个方法：

（||之间的参数可以根据自己程序的需要添加）

public static boolean sql_inj(String str){

String inj_str = "'|and|exec|insert|select|delete|update|

count|*|%|chr|mid|master|truncate|char|declare|;|or|-|+|,";

String inj_stra[] = split(inj_str,"|");

for (int i=0 ; i &lt; inj_stra.length ; i++ ){

if (str.indexOf(inj_stra[i])&gt;=0){

return true;

}

}

return false;

}

 

**4.jsp中调用该函数检查是否包函非法字符**

 

防止SQL从URL注入：

sql_inj.java代码：

package sql_inj;

import java.net.*;

import java.io.*;

import java.sql.*;

import java.text.*;

import java.lang.String;

public class sql_inj{

public static boolean sql_inj(String str){

String inj_str = "'|and|exec|insert|select|delete|update|

count|*|%|chr|mid|master|truncate|char|declare|;|or|-|+|,";

//这里的东西还可以自己添加

String[] inj_stra=inj_str.split("\\|");

for (int i=0 ; i &lt; inj_stra.length ; i++ ){

if (str.indexOf(inj_stra[i])&gt;=0){

return true;

}

}

return false;

}

}

 

**5.JSP页面判断代码：**

 

使用javascript在客户端进行不安全字符屏蔽

功能介绍：检查是否含有”‘”,”\\”,”/”

参数说明：要检查的字符串

返回值：0：是1：不是

函数名是

function check(a){

return 1;

fibdn = new Array (”‘” ,”\\”,”/”);

i=fibdn.length;

j=a.length;

for (ii=0; ii＜i; ii++)

{ for (jj=0; jj＜j; jj++)

{ temp1=a.charAt(jj);

temp2=fibdn[ii];

if (tem’; p1==temp2)

{ return 0; }

}

}

return 1;

}

总的说来，防范一般的SQL注入只要在代码规范上下点功夫就可以了。

凡涉及到执行的SQL中有变量时，用JDBC（或者其他数据持久层）提供的如：PreparedStatement就可以 ，切记不要用拼接字符串的方法就可以了。



#### MySQL语法学习-面试

##### 1.ON 与 WHERE 的区别

**前言**

很多同学在学习 Mysql 表关联的时候弄不清 `ON` 与 `WHERE` 的区别，不知道条件应该写在 `ON` 里面还是 `WHERE` 里面，作者在工作的时候也入过坑，总觉得条件写在哪里查询结果都是一样的，最后出错坏了事，差点惹了大祸。所以今天简单易懂的总结一下他们的区别，大家共同学习。

**准备工作**

我们先准备两个表，并造一些数据：

```sql
-- t1
create table t1
(
    id bigint default '0' not null comment '主键id',
    name char(100) default '' not null comment '姓名',
    age int default '0' not null
);
-- t2
create table t2
(
    id bigint default '0' not null comment '主键id',
    goals int default '0' not null comment '进球数',
    matches int default '0' not null comment '比赛编号'
);
```

**探究**

**口诀：先执行 `ON`，后执行 `WHERE`；`ON` 是建立关联关系，`WHERE` 是对关联关系的筛选**。记住这句话就可以准确地判断查询结果了，我们通过两个 sql 来进行分析：

```sql
SELECT t1.id, t1.name, t1.age FROM t1 LEFT JOIN t2 ON t1.id = t2.id WHERE matches = 2; -- 条件放在 WHERE
SELECT t1.id, t1.name, t1.age FROM t1 LEFT JOIN t2 ON t1.id = t2.id AND matches = 2; -- 条件放在 ON
```

执行第一个 sql 时，前提是 `LEFT JOIN`，所以左边的数据在创建关联关系时会保留，根据口诀，先执行 `ON` 建立关联关系，然后通过 `WHERE` 筛选。

**多表查询语法结构：**
table_reference {[INNER] JOIN | {LEFT|RIGHT} [OUTER] JOIN} table_reference ON conditional_expr

在多表查询时，ON和where都表示筛选条件，**on先执行**，**where后执行**。
**区别：**外连接时，on条件是在**生成临时表时使用的条件**，它不管on中的条件是否为真，**都会返回左边表中的记录**。而where条件是在临时表生成好后，**再对临时表进行过滤的条件**。





#### MySQL常见查询-leetCode面试笔试

##### [1. 第二高的薪水](https://leetcode-cn.com/problems/second-highest-salary/)

编写一个 SQL 查询，获取 Employee 表中第二高的薪水（Salary） 。

+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
例如上述 Employee 表，SQL查询应该返回 200 作为第二高的薪水。如果不存在第二高的薪水，那么查询应返回 null。

+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+

**思路：**

首先先将数据去重：SELECT **DISTINCT Salary** FROM Employee
再将是数据按薪水降序排除：SELECT DISTINCT Salary FROM Employee **ORDER BY Salary DESC**
分页的思想是一页一条数据，第二高的薪水则在第二页：SELECT DISTINCT Salary FROM Employee ORDER BY Salary DESC **LIMIT 1, 1**
考虑到极端情况：没有第二薪水则为空，**使用ifnull判断**：
SELECT IFNULL( (SELECT DISTINCT Salary FROM Employee ORDER BY Salary DESC LIMIT 1, 1),null) AS SecondHighestSalary
分页的其他使用：offset
SQL查询语句中的 limit 与 offset 的区别：
limit y 分句表示: 读取 y 条数据
limit x, y 分句表示: **跳过 x 条数据，读取 y 条数据**
limit y offset x 分句表示: 跳过 x 条数据，读取 y 条数据

**解法：**

①	select Salary as SecondHighestSalary from Employee order by Salary desc limit 1 offset 1

②	select ifNull((select distinct salary from Employee order by Salary Desc limit 1,1),null ) as  SecondHighestSalary;

③	**SELECT DISTINCT  Salary AS SecondHighestSalary FROM Employee ORDER BY Salary DESC LIMIT 1 OFFSET 1**



##### 2.[第N高的薪水](https://leetcode-cn.com/problems/nth-highest-salary/)

编写一个 SQL 查询，获取 Employee 表中第 n 高的薪水（Salary）。

+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
例如上述 Employee 表，n = 2 时，应返回第二高的薪水 200。如果不存在第 n 高的薪水，那么查询应返回 null。

+------------------------+
| getNthHighestSalary(2) |
+------------------------+
| 200                    |
+------------------------+

**解答：**

CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT

BEGIN

  set N = N-1;

 RETURN (

   *# Write your MySQL query statement below.*

   select ifNull((select distinct Salary from Employee 

   order by Salary desc

   limit 1 offset N),null) as SecondHighestSalary 

 );

END



##### 3.[分数排名](https://leetcode-cn.com/problems/rank-scores/)

编写一个 SQL 查询来实现分数排名。如果两个分数相同，则两个分数排名（Rank）相同。请注意，平分后的下一个名次应该是下一个连续的整数值。换句话说，名次之间不应该有“间隔”。

+----+-------+
| Id | Score |
+----+-------+
| 1  | 3.50  |
| 2  | 3.65  |
| 3  | 4.00  |
| 4  | 3.85  |
| 5  | 4.00  |
| 6  | 3.65  |
+----+-------+
例如，根据上述给定的 Scores 表，你的查询应该返回（按分数从高到低排列）：

+-------+------+
| Score | Rank |
+-------+------+
| 4.00  | 1    |
| 4.00  | 1    |
| 3.85  | 2    |
| 3.65  | 3    |
| 3.65  | 3    |
| 3.50  | 4    |
+-------+------+

**解答**：

select a.Score as Score,

(select count(distinct b.Score) from Scores as b where b.Score>=a.Score) as Rank

from Scores as a

order by a.Score desc



##### 4.编写一个 SQL 查询，查找所有至少连续出现三次的数字。

编写一个 SQL 查询，查找所有至少连续出现三次的数字。

+----+-----+
| Id | Num |
+----+-----+
| 1  |  1  |
| 2  |  1  |
| 3  |  1  |
| 4  |  2  |
| 5  |  1  |
| 6  |  2  |
| 7  |  2  |
+----+-----+
例如，给定上面的 Logs 表， 1 是唯一连续出现至少三次的数字。

+-----------------+
| ConsecutiveNums |
+-----------------+
| 1               |
+-----------------+



##### 5.[组合两个表](https://leetcode-cn.com/problems/combine-two-tables/)

表1: `Person`

```
+-------------+---------+
| 列名         | 类型     |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
PersonId 是上表主键
```

表2: `Address`

```
+-------------+---------+
| 列名         | 类型    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
AddressId 是上表主键
```

编写一个 SQL 查询，满足条件：无论 person **是否有地址信息**，都需要基于上述两表提供 person 的以下信息：

```
FirstName, LastName, City, State
```

**解答：**

考虑到可能不是每个人都有地址信息，我们应该使用 outer join 而不是默认的 inner join。

select FirstName, LastName, City, State
from Person **left join** Address
on Person.PersonId = Address.PersonId;

    注意：如果没有某个人的地址信息，使用 where 子句过滤记录将失败，因为它不会显示姓名信息。








