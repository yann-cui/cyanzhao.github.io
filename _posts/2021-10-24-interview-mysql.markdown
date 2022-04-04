---
layout: post
title:  "面试总结-mysql"
date:   2021-10-24 18:01:50 +0800
categories: 面试
---

- 通用知识点

  - 关系型数据库、no-sql 数据库、分析型数据库

    | 关系型数据库                             | no-sql 数据库                                |
    | ---------------------------------------- | -------------------------------------------- |
    | 固定的表结构                             | 基于文档的，基于K-V键值对的 没有固定的表结构 |
    | 水平扩展难，不好对数据进行分片           | 原生支持数据的水平扩展                       |
    | ACID，强调数据的强一致性                 | 数据最终一致性                               |
    | 支持复杂的表查询，高并发时读写性能比较差 | 反之~                                        |

  - 数据类型、基本语法

  - 事务

    - 事务特性ACID 

      Atomicity 原子性：各个操作看作一个逻辑单位 Consistency一致性：数据要么是提交前的数据，要么是失败全部回滚的数据 Isolation 隔离性：并发时的隔离级别 Durability 永久性：一旦提交成功，对数据的改变是永久的

    - 并发问题

      脏读 不可重复读 幻读

    - 隔离级别

      读未提交 读已提交 可重复读 串行化

  - 键

    - 唯一键： unique key

    - 主键：primary key，主键一定是唯一键

    - 外键：foreign key

      - 另一个表的主键是当前表的外键，比如用户表ID字段是订单表ID的外键

      - 可以设置约束，但会影响性能

        1. 触发：on delete、on update

        2. 可设参数：

           | 参数        |                      |
           | ----------- | -------------------- |
           | cascade     | 跟随外键改动         |
           | restrict    | 限制外表中的外键改动 |
           | set Null    | 设空值               |
           | set Default | 设默认值             |
           | 默认        | no action            |

  - 索引

    - 唯一索引

    - 主键索引，是唯一索引的特殊情况，不允许空值

    - 普通索引

    - 联合索引

    - 全文索引（倒排索引实现）

    - 索引实现 B+ R hash FullText

      - B +

        使用B+树建立的保存关键字及关键字信息在磁盘中的存储位置的数据结构

        有序值索引节点空间利用率低，无序值索引插入导致裂表的可能性更大

      - hash

        Hash方式不像Btree那样需要多次查询才能定位到记录，因此Hash索引的效率高于B-tree，但是不支持范围查找和排序等功能

  - msyql 引擎

    - ~~TokuDB BDB MEMORY~~ MyISAM InnoDB

    - 引擎主要区别

      | 引   擎    | myisam                                                       | innodb                                                       |
      | ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
      | 存储       | 每个表在磁盘上存储成三个文件：表定义文件、数据文件、索引文件 | 所有的表都保存在同一个数据文件中                             |
      | 事务       | 强调的是性能，每次查询具有原子性，不提供事务支持             | 支持事务，外部键等高级数据库功能                             |
      | 锁         | 只支持表级锁，CRUD给表自动加锁                               | 支持事务和行级锁，大幅度提高了多用户并发操作的新能           |
      | 主键与索引 | 允许没有任何索引和主键的表存在，索引都是保存行的地址         | 没有设定主键或者非空唯一索引时自动生成一个6字节的主键(不可见)，**数据是主索引的一部分**，附加索引保存的是主索引的值 |
      | 行数       | 保存有表的总行数                                             | select count(*) from table 扫描全表                          |

  - 锁相关机制

    - 粒度锁

      - 表级锁
      - 页级锁
      - 行级锁

    - 锁机制

      - 读锁（共享锁）：其他事务可以读，但不能写
      - 写锁（排他锁）：其他事务不能读取，也不能写

    - 另一种定义

      悲观锁：假定会发生并发冲突，屏蔽一切可能违反数据完整性的操作，在做操作之前先上锁 乐观锁：假设不会发生并发冲突，只在提交操作时检查是否违反数据完整性。 乐观锁不能解决脏读的问题

    - myisam

      - select 自动给表加读锁
      - update delete insert 自动给表加写锁
      - 在自动加锁的情况下，MyISAM 总是一次获得 SQL 语句所需要的全部锁，这也正是 MyISAM 表不会出现死锁（Deadlock Free）的原因
      - 支持并发插入，即在数据文件中间没有空闲块，则可以在查询同时插入行到数据文件的末尾（减少给定表的读和写操作之间的争用）

    - InnoDB

      - 行锁是通过给索引上的索引项加锁来实现的，意味着只有通过索引条件检索数据，InnoDB 才使用行级锁，否则表锁

      - 意向锁是表锁， InnoDB 自动加的， 不需用户干预

        - 意向共享锁（IS）：事务打算给数据行加行共享锁，事务在给一个数据行加共享锁前必须先取得该表的 IS 锁。
        - 意向排他锁（IX）：事务打算给数据行加行排他锁，事务在给一个数据行加排他锁前必须先取得该表的 IX 锁

      - 锁兼容
      
        ![InnoDB锁兼容](https://github.com/cyanzhao/cyanzhao.github.io/raw/master/_source/InnoDB%E9%94%81%E5%85%BC%E5%AE%B9.jpg?lastModify=1634896953)

      - 表锁：

        未命中索引

        表较小，引擎判断使用表扫描更优化

      - 间隙锁

        1、范围条件而不是相等条件检索数据，并请求共享或排他锁时，InnoDB会给符合条件的已有数据记录的索引项加锁；对于键值在条件范围内但并不存在的记录，叫做“间隙（GAP)”，InnoDB也会对这个“间隙”加锁；

        2、这种加锁机制会阻塞符合条件范围内键值的并发插入

        3、防止幻读

        4、对于数据的恢复和复制（with BINLOG）是必须的，隔离级别要求是串行化

      - 锁和多版本数据（MVCC）是 InnoDB 实现可重复读和隔离级别的手段

        ![Screen Shot 2020-03-01 at 9.55.27 PM](https://github.com/cyanzhao/cyanzhao.github.io/raw/master/_source/innodb.png?lastModify=1634896953)

      - 隐式加锁过程

        1、随时都可以执行锁定，InnoDB会根据隔离级别在需要的时候自动加锁

        2、锁只有在执行commit或者rollback的时候才会释放，并且所有的锁都是在同一时刻被释放（这里可以理解死锁产生的原因）

      - 显示加锁

        1、共享锁：SELECT * FROM table_name WHERE ... LOCK IN SHARE MODE

        2、排它锁：SELECT * FROM table_name WHERE ... FOR UPDATE

    - 死锁

      - 概念

        多个事务在同一资源上互相占用，并请求锁定对方占用的资源

      - 原因

        1、当事务试图以不同的顺序锁定资源时，就可能产生死锁

        2、多个事务同时锁定同一个资源时也可能会产生死锁

        3、锁的行为和顺序和存储引擎相关。以同样的顺序执行语句，有些存储引擎会产生死锁有些不会

      - 解决

        1、死锁检测

        2、锁超时

        3、死锁恢复：将持有最少行级排他锁的事务进行回滚

      - 避免死锁

        1、如有必要，直接申请足够级别的锁，即排他锁

        2、如果事务需要修改或锁定多个表，则应在每个事务中以相同的顺序使用加锁语句

        3、对一个表而言，尽可能以固定的顺序存取表中的行

        4、修改隔离级别

  - 存储过程与函数

    存储过程和函数都可以避免开发人员重复编写相同的SQL语句，并且存储过程和函数都是在MySQL服务器中执行的，可以减少客户端和服务器端的数据传输。

  - 数据库查询优化

    - 表设计- 范式、反范式

      - 第一范式要求最低，只要求表中字段不可用在拆分。
      - 第二范式在第一范式的基础上要求每条记录由主键唯一区分，记录中所有属性都依赖于主键。
      - 第三范式在第二范式的基础上，要求所有属性必须直接依赖主键，不允许间接依赖。
      - 一般满足第三范式即可

    - 表结构优化+索引

      - 合适的数据类型，适当反范式（冗余数据）
      - 读写分离（主从库）
        - 读多写少，提升性能
        - 数据一致性
        - Master 高可用
      - 分库、分表、增加中间表
        - 分库
          - 分担负载、单库（单机）容量
          - 垂直分库
            - 按业务拆分
            - 跨库事务复杂、无法 join、单库瓶颈
          - 水平分库
            - 按照某个维度/规则/唯一ID拆分
            - 单个库的容量可控、单条记录保证了数据完整性、数据关系可以通过 `join` 维持 、避免了跨库事务 
            - 拆分规则对编码有一定的影响（分页排序）、不同业务的分区交互需要统筹设计
        - 分表
          - 单表性能瓶颈
          - 垂直分表
            - 热点非热点字典，关系紧密字段等拆分
          - 水平分表
            - 同水平分库
      - 合适的索引，避免滥用
      - 列字段尽量设置为Not Null，空列难以查询优化

    - sql 语句优化

      - 慢查询日志/mysqldumpy、show processlist 查看执行效率低的语句，kill $ID 可以终止会话

      - explain（索引使用情况）、profile（查询分步耗时）

      - 避免使用 select *

      - 在索引列排序

      - example

        - like 会使用索引吗，经测试 "like_str" "like_str%"会使用，"%like_str(%)"不会使用；但可能因 mysql 版本而异

        - 联合索引的数据结构：多个字段分别从左到右按序排列

        - 多个索引符合条件下，mysql 只会选择一个索引

        - explain 结果解释

          重要列解释 type: const：常量，使用了主键/唯一索引 eq_ref：范围，使用了主键/唯一索引 ref：基于索引 range：扫描索引范围 index：基于索引扫描全表 all：扫描全表

          ![msyql](https://github.com/cyanzhao/cyanzhao.github.io/raw/master/_source/msyql.png?lastModify=1634896953)

          ```
          explain select * from testindex where id >0;
          
          +----+-------------+-----------+------------+-------+---------------+---------+---------+------+------+----------+-------------+
          
          | id | select_type | table     | partitions | type  | possible_keys | key     | key_len | ref  | rows | filtered | Extra       |
          
          +----+-------------+-----------+------------+-------+---------------+---------+---------+------+------+----------+-------------+
          
          |  1 | SIMPLE      | testindex | NULL       | range | PRIMARY       | PRIMARY | 4       | NULL |    6 |   100.00 | Using where |
          
          +----+-------------+-----------+------------+-------+---------------+---------+---------+------+------+----------+-------------+
          ```

- mysql

  - mac 启动

    server: mysql.server start/stop mysql.server status

    client: mysql -uroot -pk0cyan007

  - 数据类型

    数值 INT DOUBLE .. int(11) 数字代表显示宽度，与存储字节数无关

    日期/时间 DATE TIME ..

    字符串 CHAR VARCHAR BLOG TEXT ..

  - 基本语法

    - 创建表

    - CRUD

    - 联表查询

      | join         | des                                                          |
      | ------------ | ------------------------------------------------------------ |
      | left join    | 行数：左表+两表符合 on 条件的行的排列组合 结果：左表的字段 + 右表字段，没匹配到是 NULL |
      | right join   |                                                              |
      | <inner>join  | 行数：交集的排列组合                                         |
      | full join    | 貌似不支持 (select * from a order by id) union (select * from b order by id); |
      | union<all>   | 结果：两表查询的列字段的相加组合,相同的值合并 两表查询的列含义最好相同 |
      | 自己连接自己 | select a.name, b.name from team a, team b where a.name < b.name |

      ![mysql_join](https://github.com/cyanzhao/cyanzhao.github.io/raw/master/_source/mysql_join.jpg?lastModify=1634896953)

    - 分组 group by 数据分到一个分组，group by key 常与sum() avg() group_concat()等聚合函数一起使用 可以多个字段

    - where 子句在所选列上设置过滤条件

    - HAVING 子句则在由 GROUP BY 子句创建的分组上设置过滤条件 select order_id from table   	group by order_id  	Having count(*) > 2;

    - 子查询

      - from型子查询：把内层的查询结果当成临时表，供外层sql再次查询。查询结果集可以当成表看待，临时表需要一个别名。
      - where型子查询：把内层查询的结果作为外层查询的比较条件。运算符有= > < >= <= <> ANY IN SOME ALL EXISTS

    - 相关子查询：是指引用了外部查询列的子查询，即子查询会对外部查询的每行进行一次计算

    - sql example

      - 下单前10的产品
      - 所有用户中下单前10的产品

    - 事务

      BEGIN COMMIT ROLLBACK

* mongo

  * mac 启动

    server: ./mongod -f /usr/local/etc/mongod.conf

    client: ./mongo

  * 数据类型、基本语法

    aggregate：有聚合和通道的性质

* redis

  * mac 启动

    server: redis-server

    client: redis-cli

  * 特征及实现

    - 支持内存、k-结构性数据、非关系型数据库
    - 每种数据类型的CRUD
    - 单线程，epoll I/O多路复用
    - api 是原子性的，伪事务保证一系列操作的原子性（需执行过程中没有错误）
    - 乐观锁（实现： watch，类似于CAS）
    - 支持持久化
      - 在指定的时间间隔能对你的数据进行快照存储。
      - 记录每次对服务器写的操作,当服务器重启的时候会重新执行这些命令来恢复原始的数据。
    - redis 集群，一致性哈希2^32

  * 数据类型、基本语法

    | 数据类型 | 语法                                        | 特征                                               | 实现             |
    | -------- | ------------------------------------------- | -------------------------------------------------- | ---------------- |
    | string   | set key value nx[px] get  setnx incr        |                                                    |                  |
    | list     | l(r)push l(r)pop  llen lrange               | 可重复，头尾增删                                   | 双向链表         |
    | set      | sadd  smembers spop key [count] scard key   | 不重复                                             | hashmap          |
    | sort_set | zadd key 0.1 "xx" zrange key 0 -1 zcard key | 有序不重复 每一个元素有一个关联分数                | hashmap skiplist |
    | hash     | h(m)set h(m)get hgetall hdel                | 多个 kv 以 hash 形式存储； hgetall 时间复杂度 O(n) | hashmap          |

  * 应用场景

    - 缓存

    - 消息队列, list

    - 分布式锁，redis setnx

      获取锁：

      ​	setnx (set if not exist) + expire(key, timeout)

      ​	lua_script，保证原子性

      释放锁：del key

* memcached

* 列数据库