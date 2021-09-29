---
layout: post
title:  "面试总结"
date:   2021-04-04 18:01:50 +0800
categories: 其他
---

* 语言

  * 编程范式

    | 编程范式..    | 程序员的编程世界观                                           | 特点                                                         |
    | ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
    | 面向过程      | a 操作 b                                                     |                                                              |
    | 面向对象      | 定义a，a 可以操作 b                                          | 封装 继承 <br />多态（程序运行时根据具体的类/对象表现不同的行为） |
    | 面向函数      | Functional programming is about writing pure functions.<br />no side-effect 、referential-transparency |                                                              |
    | 事件驱动编程  | GUI for example                                              |                                                              |
    | 同步/异步编程 |                                                              |                                                              |

  * 设计模式

    | 设计模式..    | 解决问题的经验                                       |
    | ------------- | ---------------------------------------------------- |
    | 创建型↓       |                                                      |
    | 工厂/抽象工厂 |                                                      |
    | 单例          |                                                      |
    | 对象池        |                                                      |
    | 结构型↓       |                                                      |
    | 适配器        |                                                      |
    | 桥接          | 将抽象部分与它的实现部分分离，使它们都可以独立地变化 |
    | 装饰器        |                                                      |
    | 代理          |                                                      |
    | 行为型 ↓      |                                                      |
    | 观察者        |                                                      |

  * 语言

    * 语言编译原理
    * 动态语言 静态语言

  * C/C++

    * 一个由 C 编译的程序占用的内存分区

      栈：函数参数值、局部变量值（了解栈溢出，memcpy strcpy）

      堆：手动开辟的内存，需要手动管理

      全局区、静态区：全局变量、静态变量

      常量区：常量字符串等

      程序代码区

  * python

    * 运行时

      1. python 编译器（cpython 比如）将 python 模块编译成编译成一个字节码对象 PyCodeObject，保存在 pyc 文件中；
      2. PyCodeObject 对象包含了 Python 代码汇总的字符串、常量值、以及通过语法解析后编译生成的字节码指令；同时 PyCodeObject 还会存放这些字节码指令与原始代码行号之间的对应关系；
      3. 当模块已经被 python 编译器编译后，Python 虚拟机会从编译得到的 PyCodeObject 对象中依次读入每一条字节码指令，在当前的上下文环境中执行这条字节码

    * python 2/3 区别

      * 编码
      * 内置语句/statement --> 函数

    * 语法、函数、类

      * 描述符

        * 一个类只要实现了\_\_get__ _\_set__ _\_del__(未必全部实现)，并且该类的实例是另一个类的类属性，那么该类就称为描述符

        * 实例属性的访问顺序：数据描述符 > 实例属性 > 非数据描述符 > \__getattri__

      * 装饰器

      * 生成器、生成器表达式、迭代器、可迭代对象、容器...

        ![迭代器生成器](/Users/cyan/Desktop/frame diagram/迭代器生成器.png)

        * 生成器

          yield  

          yield from

          ​    将一个生成器部分操作委托给另一个生成器。
          ​	此外，允许子生成器（即yield from后的“参数”）返回一个值，
          ​	该值可供委派生成器（即包含yield from的生成器）使用

        * 生成器表达式

          列表推导式的生成器版本
          (x**2 for x in range(10))

      * 封装、继承（多重|钻石继承）、多态

      * 闭包

        * 闭包是定义在某个作用域中的函数

        * 变量作用域

          引用时顺序：当前函数作用域 > 任何外围作用域 > 全局作用域 > 内置作用域 

          修改外层变量：可以使用 nonlocal 语句修饰

        * nonlocal vs global

      * list tuple dictionary set 底层实现

        * list tuple

          长度可变的数组（非链表），指向这个数组的指针及其长度被保存在一个列表头结构中

        * dictionary set : hash

      * 单元测试（ < 服务测试 < 端到端测试）

        unitest + 断言

      * 序列化

        * json.loads/load json.dumps/dump
        * pickle

      * 动态语言特征  

        * 元类 
          * 类的类型是为 type，也即元类，使用 type(name, bases, dict) 可以动态的创建类
          * 也可以指定类 A 继承于 type，创建新类时，使 metaclass = A, 这样就可以定义一个类的行为
        * compile eval exec

      * 文件操作

        * 路径查找
        * 读写

      * with

        类实现了 \_\_enter__ \_\_exit__，with 类的实例

        ```
        with instance as i:
        	
        ```

      * 其他语法

        * _ __ 下划线用法
        * copy vs deepopy
        * == vs is 
        * for else
        * try except finally 的执行顺序，存在return时的执行顺序
        * 字典 key-value，value 是数组时的简化写法 collections.defaultdict(list)

    * 内存管理与回收

      * 创建

        内存池机制，频繁调用new/malloc会导致大量的内存碎片

      * 回收

        引用计数：当引用数量为0时，立即释放内存

        标记-清除：标记不可达对象，并释放

        分代回收：将对象按照存活时间分成3代，不同代执行标记清除时的时间差是分级的

        GC：人为干预垃圾内存回收

    * python 异步编程

      * 多线程 / 线程池 / 多进程/ 进程池

      * 协程

        - yield vs yield from

        - asyncio

          - python 原生支持的协程库
          - asyncio.coroutine yield from
          - async/wait

        - 框架支持

          | name              | 支持的功能                   |
          | ----------------- | ---------------------------- |
          | asyncio           | python 原生协程支持          |
          | gevent            | 基于 greenlet 实现的协程支持 |
          | tornado           | web 框架 + 协程支持          |
          | yield、yield from | 生成器实现协程               |

      * prefork

        利用了 linux 多个进程/线程可以绑定同一个端口的特性，开启多个子进程 accept 数据

      * 队列

        作为一种数据结构，在应用场景上可以提高并发（+多线程）、流量削峰（速度控制）、 程序解耦并且是原子操作（线程安全的）。

  * 其他

    * iOS
    * js css html
    * node.js
    * 小程序

    * shell 常用命令

    * 正则
      * 语法
      * python re（match search sub）

  * 工具

    * jupyter，在线代码编辑
    * vim tmux
    * vscode

* 框架或服务

  * web server

    * 组成

      install app、settings

      路由、静态文件、模板、ORM

      部署

    * nginx

      * 如何配置的（详情见 nginx 配置目录：/usr/local/etc/nginx/nginx.conf）

        ```
        # 全局块
        ...              
        # events块
        events {         
           ...
        }
        # http块
        http      
        {
            # http全局块
            ...   
            # 虚拟主机server块
            server        
            { 
                # server全局块
                ...       
                # location块
                location [PATTERN]   
                {
                    ...
                }
                location [PATTERN] 
                {
                    ...
                }
          }
          server
          {
            ...
          }
          # http全局块
          ...     
        }
        ```

      * 负载均衡策略

        轮询、权重轮询、ip_hash、第三方支持（fair/url_hash）

      * cmd, use homebrew
        brew services start/stop/restart nginx 
        ps -ef | grep nginx
        ln -s tar.nginx.conf source.nginx.conf

    * uWSGI

      * 配置详情见 /Users/cyan/Desktop/mysite/mysite/wsgi.ini
      * cmd
        uwsgi --ini uwsgin.ini
        uwsgi uwsgi.ini
        uwsgi --stop uwsgi.pid
        uwsgi --reload uwsgi.pid

    * django

      * 中间件

        * CsrfViewMiddleware

          ![django_csrfmiddleware](/Users/cyan/Desktop/frame diagram/django_csrfmiddleware.png)

    * flask

      * 插件式的“微” python webserver 框架

      * 上下文

        * 请求上下文

          每个请求是一个线程/协程，将当前请求的环境变量封装成一个上下文对象存储在以线程/协程ID为 key 的字典里，并 push 到栈式数据结构中，可以很好的隔离请求间的干扰
          比如：flask.session flask.request 的使用

        * 应用上下文

    * tornado

      * web 框架
      * httpserver httpclient
      * 异步网络
      * 协程

  * pyspider + dataservice

    * architecture

      db mq

      processor schduler fetcher result webui 

      数据中心：etl、cal、CRUD

      ws、distributed-task-worker[分布式任务队列]

    * <分布式>部署

      * 各个模块分别部署在不同的docker上，支持多主机，暂不支持集群
      * docker_compose编排上述docker容器，包括容器的连接、宿主机与容器的数据卷/目录挂载和端口映射等

    * 实现细节

      * 单机内 docker 间通过 docker0 通信，主机间端口映射方式通信
      * ws（web server）
        * resful graphql
        * ~~远程[应用间]调用~~，暂未实现
      * pyspider
        * 每个模块可以启动多个，可以运行在线程或者进程上
        * inqueue outqueue runloop
        * schdeler: project_queue with token_bucket
        * fetcher: tornado_loop
        * 数据提取（正则 pyquery xpath beautifulsoup）
        * 爬虫辅助工具及反爬措施应对
          * 抓包工具：charles

  * scrapy

  * gs_package

    * gscache

      1、实现多级缓存

      ​	  进程缓存分担主缓存访问压力

      ​	  较高性能

      2、定义本地缓存数据结构和方法
      	  双向链表：保证LRU
      	  字典：获取数据

      3、基于memcached实现分布式锁

    * gsasync，分布式的任务队列

      1. 函数描述序列化后通过 api 传递到 queue（aws的sqs），其他服务器/进程消费队列消息
      2. 消息重复、失败处理

  * gsdb

  * gsweb

  * pauli

    * 内部用户管理模块，包括用户管理、登录流程、权限管理
      * user
        * model: user_id...
      * auth_login
        * 登录凭证，包括密码登录、token登录、cert登录..
          * 可以在不同的服务上配置不同的方式
          * token 用于内网登录，uuid.uuid1()
          * cert 用于外网登录，openssl 自签名证书
          * model：user_id password/token/cert_serial
        * 验证登录，flask-session
          * 配置 session 存储
          * 处理 sesssion 信息、加密、过期等
      * perm
        * model
          * permdes
            * action 权限的行为，business: sub_business: verb
            * effect 权限是否打开
            * resource 权限的作用范围
          * roledes： name perms<list>
          * userperm：user_id roles<list> perms<list>

  * aws 服务

    * ec2

    * sqs

    * s3(boto3) + CloudFront CDN service

    * lamda

      可使您无需预配置或管理服务器即可运行代码

    * redshift

      * 按行存储的数据select时即使只涉及到几列，也要读取所有数据。这对海量

        据查询时的I/O操作是承受不了的，对于数据分析不友好。

      * 按列存储，每列的数据放在一起，且同类型数据更利于压缩存储，查询时获取

        到不同的列再组装；但增删改时麻烦。

  * logging haproxy...

  * 潮流技术

    * 消息队列 RPC 微服务 大数据 机器学习 分布式/集群...

    * 业务点：存储、计算、管理、通信、监测...

    * 框架：hadoop tensorflow spark...

    * 容器-docker

      * 概念

        * 进程上的封装，操作系统层面的虚拟
        * 镜像、容器

      * docker build/Dockerfile、docker run、docker compose

      * 数据管理（数据卷/目录挂载）

        可以指定数据卷（docker volume create xx）或者主机目录挂载到一个或多个容器中，实现数据的共享与持久化

        ![types-of-mounts](/Users/cyan/Desktop/frame diagram/types-of-mounts.png)

      * 网络

        * 单机

          * 通信方式

            Bridge、Host、Container、None

            ![docker_network_bridge](/Users/cyan/Desktop/frame diagram/docker_network_bridge.png)

          * 外部访问容器，端口映射 -p | expose

          * 容器间通信

            * 通过 docker0 网桥（Bridge）直接通信

            * create network
              docker network create -d bridge my-net
              docker run -it -rm --name box1 --network my-net box sh
              docker run -it -rm --name box2 --network my-net box sh

            * ~~extern_links link~~

              链接容器，使得源容器和接收容器可以互相通信；
              接收容器可以获取源容器的一些数据（源容器的环境变量可以在Dockerfile设置env或者docker run时指定）

        * 集群 / 多主机

          * 通信方式

            直接路由、隧道方式（tunnel）、桥接方式（Pipework）

      * 集群：docker swarm、 Kubernetes(k8s)

        * 解决什么问题

          管理跨多个主机的容器，提供基本的部署，调度以及运用伸缩

        * 基本组成（k8s） 

          ![ks8架构](/Users/cyan/Desktop/frame diagram/ks8架构.jpg)

    * 消息队列

      * rabbitmq（amqp advanced message queue protocol 的实现）

        * 几个概念

          broker：简单来说就是消息队列服务器实体。

          vhost：虚拟主机，一个broker里可以开设多个vhost，用作不同用户的权限分离。
          channel：消息通道，在客户端的每个连接里，可建立多个channel，每个channel代表一个会话任务

          exchange：消息交换机，它指定消息按什么规则，路由到哪个队列。
          queue：消息队列载体，每个消息都会被投入到一个或多个队列。

          routing Key：路由关键字，exchange根据这个关键字进行消息投递。

          binding：绑定，它的作用就是把exchange和queue按照路由规则绑定起来。

        * 消息发布模式

          消费者生产者模式

          广播：fanout direct topic

        * 持久化

      * ~~kafka~~

      * redis.list

    * 分布式任务分发系统，配置消息队列（gsasync/celery）

      ![celery](/Users/cyan/Desktop/frame diagram/celery.png)

    * RPC

      * 概念

        * 解决分布式系统中，服务之间的调用问题。
        * 远程调用时，要能够像本地调用一样方便，让调用者感知不到远程调用的逻辑
        * 总结：RPC = 动态代理模式 + socket

      * 实现

        * Call ID映射
        * 序列化和反序列化
        * 网络传输（socket or http）

      * python 框架

        gRPC vs thrift

    * 微服务

      * 设计原则

        AKF（按业务）拆分原则

        前后端分离

        无状态服务

        通信风格（Restful 优先）

      * 平台设计

        ![微服务平台的内容](/Users/cyan/Desktop/frame diagram/微服务平台的内容.jpg)

    * 分布式存储

      * elasticsearch，倒排索引

        索引--》库

        类型--》表

        文档--》行

    * 分布式锁：

      * 特征

        互斥性： 同一时刻只能有一个线程持有锁

      ​	  可重入性： 同一节点上的同一个线程如果获取了锁之后能够再次获取锁

        	锁超时：和J.U.C中的锁一样支持锁超时，防止死锁
        	
        	高性能和高可用： 加锁和解锁需要高效，同时也需要保证高可用，防止分布式锁失效
        	
        	具备阻塞和非阻塞性：能够及时从阻塞状态中被唤醒

      * 实现

        数据库、redis

* 数据库

  * 通用知识点

    * 关系型数据库、no-sql 数据库、分析型数据库

      | 关系型数据库                             | no-sql 数据库                                     |
      | ---------------------------------------- | ------------------------------------------------- |
      | 固定的表结构                             | 基于文档的，基于K-V键值对的<br />没有固定的表结构 |
      | 水平扩展难，不好对数据进行分片           | 原生支持数据的水平扩展                            |
      | ACID，强调数据的强一致性                 | 数据最终一致性                                    |
      | 支持复杂的表查询，高并发时读写性能比较差 | 反之~                                             |

    * 数据类型、基本语法

    * 事务

      * 事务特性ACID 

        Atomicity 原子性：各个操作看作一个逻辑单位
        Consistency一致性：数据要么是提交前的数据，要么是失败全部回滚的数据
        Isolation 隔离性：并发时的隔离级别
        Durability 永久性：一旦提交成功，对数据的改变是永久的

      * 并发问题

        脏读 不可重复读 幻读

      * 隔离级别

        读未提交 读已提交 可重复读 串行化

    * 键

      * 唯一键： unique key

      * 主键：primary key，主键一定是唯一键

      * 外键：foreign key

        * 另一个表的主键是当前表的外键，比如用户表ID字段是订单表ID的外键

        * 可以设置约束，但会影响性能

          1. 触发：on delete、on update

          2. 可设参数：

             | 参数        |                      |
             | ----------- | -------------------- |
             | cascade     | 跟随外键改动         |
             | restrict    | 限制外表中的外键改动 |
             | set Null    | 设空值               |
             | set Default | 设默认值             |
             | 默认        | no action            |

    * 索引

      * 唯一索引

      * 主键索引，是唯一索引的特殊情况，不允许空值

      * 普通索引

      * 联合索引

      * 全文索引（倒排索引实现）

      * 索引实现 B+ R hash FullText

        * B +

          使用B+树建立的保存关键字及关键字信息在磁盘中的存储位置的数据结构

          有序值索引节点空间利用率低，无序值索引插入导致裂表的可能性更大

        * hash

          Hash方式不像Btree那样需要多次查询才能定位到记录，因此Hash索引的效率高于B-tree，但是不支持范围查找和排序等功能

    * msyql 引擎

      * ~~TokuDB BDB MEMORY~~ MyISAM InnoDB

      * 引擎主要区别

        | 引   擎    | myisam                                                       | innodb                                                       |
        | ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
        | 存储       | 每个表在磁盘上存储成三个文件：表定义文件、数据文件、索引文件 | 所有的表都保存在同一个数据文件中                             |
        | 事务       | 强调的是性能，每次查询具有原子性，不提供事务支持             | 支持事务，外部键等高级数据库功能                             |
        | 锁         | 只支持表级锁，CRUD给表自动加锁                               | 支持事务和行级锁，大幅度提高了多用户并发操作的新能           |
        | 主键与索引 | 允许没有任何索引和主键的表存在，索引都是保存行的地址         | 没有设定主键或者非空唯一索引时自动生成一个6字节的主键(不可见)，**数据是主索引的一部分**，附加索引保存的是主索引的值 |
        | 行数       | 保存有表的总行数                                             | select count(*) from table 扫描全表                          |

    * 锁相关机制

      * 粒度锁

        * 表级锁
        * 页级锁
        * 行级锁

      * 锁机制

        * 读锁（共享锁）：其他事务可以读，但不能写
        * 写锁（排他锁）：其他事务不能读取，也不能写

      * 另一种定义

        悲观锁：假定会发生并发冲突，屏蔽一切可能违反数据完整性的操作，在做操作之前先上锁
        乐观锁：假设不会发生并发冲突，只在提交操作时检查是否违反数据完整性。 乐观锁不能解决脏读的问题

      * myisam

        * select 自动给表加读锁
        * update delete insert 自动给表加写锁
        * 在自动加锁的情况下，MyISAM 总是一次获得 SQL 语句所需要的全部锁，这也正是 MyISAM 表不会出现死锁（Deadlock Free）的原因
        * 支持并发插入，即在数据文件中间没有空闲块，则可以在查询同时插入行到数据文件的末尾（减少给定表的读和写操作之间的争用）

      * InnoDB

        * 行锁是通过给索引上的索引项加锁来实现的，意味着只有通过索引条件检索数据，InnoDB 才使用行级锁，否则表锁

        * 意向锁是表锁， InnoDB 自动加的， 不需用户干预

          * 意向共享锁（IS）：事务打算给数据行加行共享锁，事务在给一个数据行加共享锁前必须先取得该表的 IS 锁。
          * 意向排他锁（IX）：事务打算给数据行加行排他锁，事务在给一个数据行加排他锁前必须先取得该表的 IX 锁

        * 锁兼容

          ![InnoDB锁兼容](/Users/cyan/Desktop/frame diagram/InnoDB锁兼容.jpg)

        * 表锁：

          未命中索引

          表较小，引擎判断使用表扫描更优化

        * 间隙锁

          1、范围条件而不是相等条件检索数据，并请求共享或排他锁时，InnoDB会给符合条件的已有数据记录的索引项加锁；对于键值在条件范围内但并不存在的记录，叫做“间隙（GAP)”，InnoDB也会对这个“间隙”加锁；

          2、这种加锁机制会阻塞符合条件范围内键值的并发插入

          3、防止幻读

          4、对于数据的恢复和复制（with BINLOG）是必须的，隔离级别要求是串行化

        * 锁和多版本数据（MVCC）是 InnoDB 实现可重复读和隔离级别的手段

          ![Screen Shot 2020-03-01 at 9.55.27 PM](/Users/cyan/Desktop/frame diagram/innodb.png)

        * 隐式加锁过程

          1、随时都可以执行锁定，InnoDB会根据隔离级别在需要的时候自动加锁

          2、锁只有在执行commit或者rollback的时候才会释放，并且所有的锁都是在同一时刻被释放（这里可以理解死锁产生的原因）

        * 显示加锁

          1、共享锁：SELECT * FROM table_name WHERE ... LOCK IN SHARE MODE

          2、排它锁：SELECT * FROM table_name WHERE ... FOR UPDATE

      * 死锁

        * 概念

          多个事务在同一资源上互相占用，并请求锁定对方占用的资源

        * 原因

          1、当事务试图以不同的顺序锁定资源时，就可能产生死锁

          2、多个事务同时锁定同一个资源时也可能会产生死锁

          3、锁的行为和顺序和存储引擎相关。以同样的顺序执行语句，有些存储引擎会产生死锁有些不会

        * 解决

          1、死锁检测

          2、锁超时

          3、死锁恢复：将持有最少行级排他锁的事务进行回滚

        * 避免死锁

          1、如有必要，直接申请足够级别的锁，即排他锁

          2、如果事务需要修改或锁定多个表，则应在每个事务中以相同的顺序使用加锁语句

          3、对一个表而言，尽可能以固定的顺序存取表中的行

          4、修改隔离级别

    * 存储过程与函数

      存储过程和函数都可以避免开发人员重复编写相同的SQL语句，并且存储过程和函数都是在MySQL服务器中执行的，可以减少客户端和服务器端的数据传输。

    * 数据库查询优化

      * 表设计- 范式、反范式

        * 第一范式要求最低，只要求表中字段不可用在拆分。
        * 第二范式在第一范式的基础上要求每条记录由主键唯一区分，记录中所有属性都依赖于主键。
        * 第三范式在第二范式的基础上，要求所有属性必须直接依赖主键，不允许间接依赖。
        * 一般满足第三范式即可

      * 表结构优化+索引

        * 合适的数据类型，适当反范式（冗余数据）
        * 读写分离（主从库）
          * 读多写少，提升性能
          * 数据一致性
          * Master 高可用
        * 分库、分表、增加中间表
          * 分库
            * 分担负载、单库（单机）容量
            * 垂直分库
              * 按业务拆分
              * 跨库事务复杂、无法 join、单库瓶颈
            * 水平分库
              * 按照某个维度/规则/唯一ID拆分
              * 单个库的容量可控、单条记录保证了数据完整性、数据关系可以通过 `join` 维持 、避免了跨库事务 
              * 拆分规则对编码有一定的影响（分页排序）、不同业务的分区交互需要统筹设计
          * 分表
            * 单表性能瓶颈
            * 垂直分表
              * 热点非热点字典，关系紧密字段等拆分
            * 水平分表
              * 同水平分库
        * 合适的索引，避免滥用
        * 列字段尽量设置为Not Null，空列难以查询优化

      * sql 语句优化

        * 慢查询日志/mysqldumpy、show processlist 查看执行效率低的语句，kill $ID 可以终止会话

        * explain（索引使用情况）、profile（查询分步耗时）

        * 避免使用 select *

        * 在索引列排序

        * example

          * like 会使用索引吗，经测试 "like_str" "like_str%"会使用，"%like_str(%)"不会使用；但可能因 mysql 版本而异

          * 联合索引的数据结构：多个字段分别从左到右按序排列

          * 多个索引符合条件下，mysql 只会选择一个索引

          * explain 结果解释

            重要列解释 type:
            	const：常量，使用了主键/唯一索引
            	eq_ref：范围，使用了主键/唯一索引
            	ref：基于索引
            	range：扫描索引范围
            	index：基于索引扫描全表
            	all：扫描全表

            explain select * from testindex where id >0;

            +----+-------------+-----------+------------+-------+---------------+---------+---------+------+------+----------+-------------+

            | id | select_type | table     | partitions | type  | possible_keys | key     | key_len | ref  | rows | filtered | Extra       |

            +----+-------------+-----------+------------+-------+---------------+---------+---------+------+------+----------+-------------+

            |  1 | SIMPLE      | testindex | NULL       | range | PRIMARY       | PRIMARY | 4       | NULL |    6 |   100.00 | Using where |

            +----+-------------+-----------+------------+-------+---------------+---------+---------+------+------+----------+-------------+

            ![msyql](/Users/cyan/Desktop/frame diagram/msyql.png)

  * mysql

    * mac 启动

      server: mysql.server start/stop mysql.server status

      client: mysql -uroot -pk0cyan007

    * 数据类型

      数值 INT DOUBLE .. int(11) 数字代表显示宽度，与存储字节数无关

      日期/时间 DATE TIME ..

      字符串 CHAR VARCHAR BLOG TEXT ..

    * 基本语法

      * 创建表

      * CRUD

      * 联表查询

        | join         | des                                                          |
        | ------------ | ------------------------------------------------------------ |
        | left join    | 行数：左表+两表符合 on 条件的行的排列组合<br />结果：左表的字段 + 右表字段，没匹配到是 NULL |
        | right join   |                                                              |
        | <inner>join  | 行数：交集的排列组合                                         |
        | full join    | 貌似不支持<br />(select * from a order by id) union (select * from b order by id); |
        | union<all>   | 结果：两表查询的列字段的相加组合,相同的值合并<br />两表查询的列含义最好相同 |
        | 自己连接自己 | select a.name, b.name from team a, team b where a.name < b.name |
        |              |                                                              |
        |              |                                                              |


        ![mysql_join](/Users/cyan/Desktop/frame diagram/mysql_join.jpg)

      * 分组 group by
        数据分到一个分组，group by key 常与sum() avg() group_concat()等聚合函数一起使用
        可以多个字段

      * where 子句在所选列上设置过滤条件

      * HAVING 子句则在由 GROUP BY 子句创建的分组上设置过滤条件
        	select order_id from table 
          	group by order_id
          	Having count(*) > 2;

      * 子查询

        * from型子查询：把内层的查询结果当成临时表，供外层sql再次查询。查询结果集可以当成表看待，临时表需要一个别名。
         * where型子查询：把内层查询的结果作为外层查询的比较条件。运算符有= > < >= <= <> ANY IN SOME ALL EXISTS

      * 相关子查询：是指引用了外部查询列的子查询，即子查询会对外部查询的每行进行一次计算

      * sql example

        * 下单前10的产品

          select chanpin_id, nums from 
          	(select chanpin_id, sum(num) as nums from order group by chanpin_id) as tmp 
          order by nums desc; 

        * 所有用户中下单前10的产品

          select chanpin_id, user_id, nums from 
          	(select chanpin_id, user_id, sum(num) as nums from order group by chanpin_id, user_id) as tmp 
          order by nums desc; 

      * 事务

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

      | 数据类型 | 语法                                                     | 特征                                                    | 实现             |
      | -------- | -------------------------------------------------------- | ------------------------------------------------------- | ---------------- |
      | string   | set key value nx[px]<br />get <br />setnx<br />incr      |                                                         |                  |
      | list     | l(r)push l(r)pop <br />llen lrange                       | 可重复，头尾增删                                        | 双向链表         |
      | set      | sadd <br />smembers<br />spop key [count]<br />scard key | 不重复                                                  | hashmap          |
      | sort_set | zadd key 0.1 "xx"<br />zrange key 0 -1<br />zcard key    | 有序不重复<br />每一个元素有一个关联分数                | hashmap skiplist |
      | hash     | h(m)set<br />h(m)get<br />hgetall<br />hdel              | 多个 kv 以 hash 形式存储；<br />hgetall 时间复杂度 O(n) | hashmap          |

    * 应用场景

      * 缓存

      * 消息队列, list

      * 分布式锁，redis setnx

        获取锁：

        ​	setnx (set if not exist) + expire(key, timeout)

        ​	lua_script，保证原子性

        释放锁：del key

  * memcached

  * 列数据库

* 网络

  * 应用层

    * 确定进程之间通信的性质以满足用户需求。
      应用层协议有很多，如支持万维网应用的HTTP协议，支持电子邮件的SMTP协议，支持文件传送的FTP协议等

    * 传输单位：**报文 message**（程序级数据）

    * 协议及实现

      * http

        * 报文由四部分组成：起始行、首部、空行、主体
          起始行和首部都是由行分隔的ASCII文本，每行以行终止符作为结束；主体是一个可选的数据块，可以是文本或者任意的二进制数据

          请求报文：

          ```
          <method> <request-URL> <version>
          <headers>
          
          <entigy-body>
          ```

          响应报文：

          ```
          <version> <status> <response-phrase>
          <headers>
          
          <entity-body>
          ```

          post 请求

          ```
          POST / HTTP1.1
          Host:www.wrox.com
          User-Agent:Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 2.0.50727; .NET CLR 3.0.04506.648; .NET CLR 3.5.21022)
          Content-Type:application/x-www-form-urlencoded
          Content-Length:40
          Connection: Keep-Alive
          
          name=Professional%20Ajax&publisher=Wiley
          ```

        * restful api 

          REST: Representational State Transfer
          URL定位资源，HTTP动词（GET,POST,DELETE,PUT,HEAD,OPTION..）描述操作

        * graphql

        * 如何保存无状态的http协议

          * cookie

          * session

          * token/jwt (token的标准实现)

            ![token](/Users/cyan/Desktop/frame diagram/token.png)

            1. 用户通过用户名和密码发送请求
            2. 程序验证
            3. 程序返回一个签名的token给客户端
            4. 客户端储存token, 并且每次用每次发送请求
            5. 服务端验证Token并返回数据
            6. JWT = Base64(Header) + "." + Base64(Payload) + "." + $Signature
            7. 每次服务器验证token是否可信+是否过期等，即可判断该用户的登录状态

      * https

        http + ssl/tsl，secure socket layer 的作用在于每次建立会话时先验证下对方是否可信任，确认后再使用普通的HTTP协议通信，具体通信过程：
        1） 客户端向服务器端索要并验证公钥。
        2） 双方协商生成"对话密钥"。
        3） 双方采用"对话密钥"进行加密通信。

        ![SSL过程](/Users/cyan/Desktop/frame diagram/SSL过程.gif)

      * websocket

        客户端、服务端的双向通信

  * 表示层

    应用层数据编码和转化,以确保以一个系统应用层发送的信息 可以被另一个系统应用层识别

  * 会话层

    * socket 编程

      socket是进程通讯的一种方式，即调用这个网络库的一些API函数实现分布在不同主机的相关进程之间的数据交换；

      五元组（协议，本地地址，本地端口号，远地地址，远地端口号），其中协议就是TCP或者UDP协议。

  * 传输层

    * 负责主机间不同进程的通信。
      这一层中的协议有面向连接的TCP（传输控制协议）、无连接的UDP（用户数据报协议）；

    * 传输单元：**报文段 segment**（组成报文的分组）

    * TCP

      * 特点：
        流式连接（建立连接后可以不断发送数据报）
        可靠性（数据不丢失 不失序） 
        有效流控 
        全双工操作 
        多路复用

      * 数据格式

        ![TCP报文](/Users/cyan/Desktop/frame diagram/TCP报文.jpg)

        TCP首部最小为20字节，这20字节分为5行，每行4个字节也就是32个位，各字段占用的字节数如图，各字段表示意义如下：

        源端口号：

        目标端口号：

        序列号：

        ​	TCP用序列号对数据包进行标记，以便在到达目的地后重新重装，假设当前的序列号为 s，发送数据长度为 l（认为是**普通报文**），则下次发送数据时的序列号为 s + l。在建立连接时通常由计算机生成一个随机数作为序列号的初始值。

        确认号：【累积确认】

        ​	表示期望收到对方下一个报文段的序号值，通讯的任何一方在收到对方的一个报文之后，都要发送一个相对应的**「确认报文」**，来表达确认收到。

        部首长度：

        URG：

        ACK：
        	当 ACK = 1 的时候，确认号（Acknowledgemt Number）有效。 一般称携带 ACK 标志的 TCP 报文段为「确认报文段」。为0表示数据段不包含确认信息，确认号被忽略。
        	TCP 规定，在连接建立后所有传送的报文段都必须把 ACK 设置为 1。

        PSH：

        ​	当 PSH = 1 的时候，表示该报文段高优先级，接收方 TCP 应该尽快推送给接收应用程序，而不用等到整个 TCP 缓存都填满了后再交付。

        RST：

        ​	当 RST = 1 的时候，表示 TCP 连接中出现严重错误，需要释放并重新建立连接。 一般称携带 RST 标志的 TCP 报文段为**「复位报文段」**。

        SYN：

        ​	当 SYN = 1 的时候，表明这是一个请求连接报文段。 一般称携带 SYN 标志的 TCP 报文段为**「同步报文段」**。 在 TCP 三次握手中的第一个报文就是同步报文段，在连接建立时用来同步序号。

        FIN：

        ​	当 FIN = 1 时，表示此报文段的发送方的数据已经发送完毕，并要求释放 TCP 连接。

        一般称携带 FIN 的报文段为**「结束报文段」**。

        窗口大小：

        ​	该字段明确指出了现在允许对方发送的数据量，它告诉对方本端的 TCP 接收缓冲区还能容纳多少字节的数据，这样对方就可以控制发送数据的速度。 窗口大小的值是指，从本报文段首部中的确认号算起，接收方目前允许对方发送的数据量。

        校验和：

        ​	由发送端填充，接收端对 TCP 报文段执行 CRC 算法，以检验 TCP 报文段在传输过程中是否损坏，如果损坏这丢弃。

        ​	检验范围包括首部和数据两部分，这也是 TCP 可靠传输的一个重要保障。

        紧急指针：

        ​	仅在 URG = 1 时才有意义，它指出本报文段中的紧急数据的字节数。 当 URG = 1 时，发送方 TCP 就把紧急数据插入到本报文段数据的最前面，而在紧急数据后面的数据仍是普通数据。

      * 是如何工作的

        * TCP 建立连接、断开连接三次握手四次挥手
          注意，MSL：Maxinum Segment Lifetime

          ![TCP连接三次握手](/Users/cyan/Desktop/frame diagram/TCP连接三次握手.jpg)

          ![TCP连接四次挥手](/Users/cyan/Desktop/frame diagram/TCP连接四次挥手.jpg)

      * 建立连接后发送数据时
        发送端可以发送多个序列号的报文段，
        接收端需要发送确认收到的序列号的报文段，确认号=序列号+报文长度+1（这里确认包没有重传机制），这里不一定每个报都要确认，可以累计确认，
          发送端从未收到确认的序列号起，重传

        * TCP 过程异常分析

          * 建立连接

          * 断开连接

          * 数据传输中

          * 异常有哪些

            一方服务器崩溃、一方服务器崩溃并重启、网络问题、长时间无数据

          * 如何保证处理异常重传机制、探测报文、保活机制 

        * 如何确定已经接受完毕数据

          1、报文中消息首部字段 Conent-Length

          2、报文中消息首部字段 Transfer-Encoding，数据发送结束后打上结束标志

        * keep alive：应用层实现该协议，使得客户端和服务端支持发送完数据后socket连接并不关闭，再下一层是保持TCP连接不关闭

    * UDP

      * 数据格式

        ![UDP数据格式](/Users/cyan/Desktop/frame diagram/UDP数据格式.png)

      * 是如何工作的

        UDP数据有可能在网络层被分片发送，在接收端根据分片标志组装数据，如果组装后的数据长度与UDP首部的长度字段的值对不起来，则丢弃整个UDP数据报

  * 网络层

    * 负责分组交换网中不同主机间的通信，IP协议；
      作用：发送数据时，将传输层中的报文段或用户数据报封装成IP数据报，并选择合适路由

    * 传输单位：**数据报 datagram**

    * 设备：路由器

    * 数据格式

      ![IP协议数据格式](/Users/cyan/Desktop/frame diagram/IP协议数据格式.jpeg)

      如果传输层部分字节数过大或者因为数据链路层帧数据段长度限制，IP数据报需要进行分片发送，这在首部的第二行表示。

  * 数据链路层

    * 负责将网络层的IP数据报组装成帧
    * 传输单位：**帧 frame**
    * 设备：交换机

  * 物理层

    * 透明地传输比特流
    * 传输单位：**比特 bit**

  * TCP/IP协议族

    数据包（datapacket）是TCP/IP通信协议传输中的数据单位

    ![TCP:IP数据包](/Users/cyan/Desktop/frame diagram/TCP:IP数据包.jpg)

  * and：

    * DNS（Domain Name System）

    * CDN (Content Delivery Network)

      通过在网络各处放置节点服务器，实时地根据网络流量和各节点的连接、负载状况以及到用户的距离和响应时间等综合信息将用户的请求重新导向离用户最近的服务节点上

      ![CDN原理](/Users/cyan/Desktop/frame diagram/CDN原理.jpg)

    * ddos攻击原理

      利用网络分层各个协议的特点(分布式)使服务器拒绝服务的目的

      SYN-Flood: 发送大量syn连接，但并不完成握手连接的第三步，服务器会不停的重试并等待一定的时间后（sync timeout）放弃这个未完成的连接，最终服务器的网络资源和带宽被这种大量的半连接消耗殆尽

      ACK-Flood：建立连接传输数据时，数据包四元组故意设计错误，服务器发送RST，导致重新建立连接

      UDP-Flood：UDP协议是无连接性的，服务端只要开了一个UDP的端口提供相关服务的话，那么就可用大量UDP小包冲击（DNS服务器或Radius认证服务器、流媒体视频服务器）
      Connection-Flood、HTTP-GET、UDP DNS Query Flood

    * 网络安全

      ![网络安全](/Users/cyan/Desktop/frame diagram/网络安全.png)

      * CSRF（Cross Site Request Forgery）

        - 登录受信任网站A，并在本地生成Cookie。

        - 在不登出A的情况下，访问危险网站B

        - 解决：服务端在set-cookie中设置随机的anti-csrf-token

          ![csrf](/Users/cyan/Desktop/frame diagram/csrf.jpg)

      * XSS（cross-site scripting attacks）

        * 攻击者通过代码注入（code injection）的方式，恶意篡改受害者所访问的网站内容；把受害者引导到恶意网站；窃取受害者cookie

          ![stored xss](/Users/cyan/Desktop/frame diagram/stored xss.png)

    * 跨域

      * 什么是跨域

        * 同源策略

          * 同源策略是一种约定，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，浏览器很容易受到XSS、CSRF等攻击

          * 同源是指"协议+域名+端口"三者全部相同

        * 同源策略限制内容

          - Cookie、LocalStorage、IndexedDB 等存储性内容
          - DOM 节点
          - AJAX 请求发送后，结果被浏览器拦截了

        * 同源策略未限制内容

          - `<img src=XXX>`
          - `<link href=XXX>`
          - `<script src=XXX>`

        * 所以什么是跨域

          当不同源之间请求资源时，就被称为“跨域”，而是否同源是通过“URL首部”来判断的。

          注意：跨域请求时，请求已经发出去了，只是被浏览器拦截了。

      * CDN会遇到跨域问题吗

        配置CDN的资源域名一般与网站、api等域名不一致，会产生跨域问题；

        但是如果仅是图片资源等，<img src> 是未被同源策略限制的；

        猜测：配置CDN域名时，应该在服务端做过处理

      * 怎么解决跨域问题

        * jsonp

          <scripts callback>

        * cors

          配置服务器 webserver 支持 Access-Control-Allow-Origin: 源域名

        * nginx 反向代理

          服务器之间请求不存在跨域问题，可以配置nginx作为一个和源域名相同的反向代理，同时传递cookie参数给目的域名

* 计算机公共基础

  * 计算机原理

    * 计算机原理--程序执行过程（计算机是怎么跑起来的）

  * 操作系统

    * 操作系统--linux

      - 架构

        <img src="/Users/cyan/Desktop/frame diagram/linux 架构.jpg" alt="linux 架构" style="zoom:40%;" />

        ![linux 内核架构](/Users/cyan/Desktop/frame diagram/linux 内核架构.png)

      - ~~文件系统~~

    * 程序执行调度

      - **进程**

        - 是一个具有一定功能的程序在一个数据集上的一次动态执行过程。

        * 在内存中的数据结构

          进程由**程序，数据集合和进程控制块**三部分组成。
          程序用于描述进程要完成的功能，是控制进程执行的指令集；
          数据集合是程序在执行时需要的数据和工作区；
          程序控制块（PCB）包含程序的描述信息和控制信息，是进程存在的唯一标志。

        * 进程的几种状态

          | 标识 | 说明                                                         |
          | ---- | ------------------------------------------------------------ |
          | R    | 正在运行或者在运行队列中等待                                 |
          | S    | 中断（休眠中，受阻，在等待某个条件的形成或接收到信号）       |
          | D    | 不可中断（收到信号不唤醒和不可运行，进程必须等待直到有中断发生） |
          | Z    | 僵死（进程已终止，但是进程描述符仍存在，直到父进程调用wait4() 后释放） |
          | T    | 停止（进程收到SIGSTOP, SIGSTP, SIGTIN, SIGOUT 信号后停止运行） |

      * 线程

        * 是能拥有资源和独立运行的最小单位，也是程序执行的最小单位。

        * 在内存中的数据结构

          复用进程的地址空间，数据空间，资源描述符表（~~ps：未必所有的数据空间都是复用的，线程应该有自己独立的栈~~）

        * 线程的几种状态

          * 新建

          * 可运行的，被调度算法执行

          * 正在运行

          * 阻塞 synchronized  I/O_block

          * 等待 wait()  join()  sleep()

          * 终止 

            ![thread_status](/Users/cyan/Desktop/frame diagram/thread_status.png)

        * 守护线程 vs 工作线程

          当主线程只剩下守护线程时，主线程退出 

      * **进程线程的另一种描述：**进程和线程都是一个时间段（或者指令条数）的描述，是CPU工作时间段的描述，不过是颗粒大小不同。
        进程就是包换上下文切换的程序，执行时间总和 = CPU加载上下文+CPU执行+CPU保存
        线程是CPU执行的时间片时间

      * 进程线程调度实现

        在linux采用相同的调度技术，即存在优先级的红黑树数据结构；

        处于阻塞状态或者等待状态的线程，只有解除状态后才进入调度队列

      * 例程：

        - 过程、函数、方法、子程序被定义为操作的序列。

          例程的执行形成了父子关系，且孩子例程总是在父例程前结束。
          也即压栈弹栈式函数调用

      * 协程：

        - 是例程的范化概念。协程通过保持执行状态可以明确的挂起和恢复，每个协程拥有自己的栈和控制块(control-block)。
          也即将协程状态可以在挂起时在独立的栈中保留，在恢复时从栈中恢复到主栈

        - 特征区分

          控制传递（Control-transfer）机制

          协程是否为栈式（Stackful）构造

          协程是否作为语言的第一类（First-class）对象提供

      * 锁（即线程同步机制）

        多线程下，为了线程同步，避免资源竞争问题，可以使用原子操作或

        者锁机制。

        锁是一种非强制机制，每一个线程访问数据或资源之前，首先试图获

        取(Acquireuytreewq)锁,并在访问结束之后释放(release)。在锁已经被

        占用时获取锁，线程会等待，直到该锁被释放。

      * 通信

        - 进程间通信，在内核态交换数据

          共享内存

          消息队列

          信号（signal）、 信号量、

          管道（pipe），流管道(s_pipe)和有名管道（FIFO）

          套接字（socket)，可用于不同计算机间进程的通信

        * 线程间通信：主要要解决线程同步（数据竞争）问题和线程间控制问题

          * 互斥锁提供了以排他方式防止数据结构被并发修改的方法。

            * 属于sleep-waiting类型的锁，包括普通锁、检错锁、递归锁

            * 即当锁处于占用状态时，其他线程会挂起，当锁被释放时，

              所有等待的线程都将被唤醒，再次对锁进行竞争。

              在挂起与释放过程中，涉及用户态与内核态之间的context切

              换，而这种切换是比较消耗性能的。

          * 自旋锁

            自旋锁被某线程占用时，其他线程不会进入睡眠(挂起)状态，而是一直运行（自旋/空转）直到锁被释放。由于不涉及用户态与内核态之间的切换，它的效率远远高于互斥锁。

          * 读写锁允许多个线程同时读共享数据，而对写操作是互斥的。

          * 条件变量可以以原子的方式阻塞进线程，直到某个特定条件为真为止。对条件的测试是在互斥锁的保护下进行的。条件变量始终与互斥锁一起使用。

          * 信号量机制(Semaphore)

          * 信号机制(Signal)

          * 线程间控制

            主要就是线程状态切换通知，如  interrupt() join() notify() yield()

      * 线程池/进程池（线程的复用机制）

        ![线程池](/Users/cyan/Desktop/frame diagram/线程池.png)

      * 异步编程

        - 异步编程需要解决的问题，具体实现见 python 异步编程

          - 事件循环、回调
          - 多个回调之间的状态管理困难
          - 同步的代码结构

        - 同步 vs 异步（**用户态、任务**）

          - 指的是在用户态，任务之间是否等待，其中异步的实现有多种方式，如：多线程、消息队列、reactor模式、甚至定期询问 fd 是否准备就绪等

        - 堵塞 vs 非阻塞（**线程**）

          -  指的是当前线程是否被阻塞，比如常见的I/O操作、lock等

        - 并发 vs 并行：并行是真正的多核推进

        - 5种网络 IO

          - 阻塞 blocking IO

            ![2阶段网络IO](/Users/cyan/Desktop/frame diagram/2阶段网络IO.jpg)

          - 非阻塞nonblocking IO

          - 信号驱动signal driven IO

          - 就绪事件通知

            - 维护一个状态，由用户读取，在数据就绪时就生效

            - reactor 模式就是一种利用了**就绪事件通知**原则实现的I/O多路复用模型

              系统调用（like recv()）之前先调用epoll（select poll ），内核准

              备好数据后（这个过程不占用CPU的），通知进程哪几个socket

              有数据，将数据从内核空间copy到进程空间

            - select poll epoll 对比

          - 异步IO

            * 异步IO由系统调用用户的回调函数，直到数据IO完成才发生回调

  * 软件设计

    * 分布式系统 uniqueID

    * 设计一个目录-题目、用户-题目模块

      实现：

      * 章节页三个字段的联合索引（B+ 树结构）
      * 预热用户做题的正确率，写入 redis 缓存

    * 点赞系统

    * 订单系统

    * 秒杀系统

    * 缓存机制

      缓存的访问速度高于数据库的访问速度，一般都是基于内存缓存（redis memcache）

      设计原则：高可用性、一致性、实时性

      具体设计：

      * 多级缓存
        * 进程内缓存 --> 缓存集群 --> 数据库
        * 分布式缓存算法（扩展节点）：hash、一致性hash
      * 缓存更新
        * 懒加载式，加载数据时，从缓存读取，缓存没有向上一级缓存或数据库读取，并补充缓存；懒加载删除
        * 异步更新，启动线程更新
        * 任务队列，刷新节点缓存
      * 缓存淘汰策略: FIFO、LRU、LFU
      * 缓存雪崩（缓存同时失效）
        * 随机缓存时长
        * 增加互斥锁，对缓存的更新操作进行加锁保护，保证只有一个线程进行缓存更新
      * 缓存穿透（不存在的数据直接查询数据库）
        * 缓存空结果
        * bloom filter
      * 缓存击穿（在数据请求的时候，某一个缓存刚好失效或者正在写入缓存）
        * mutex
      * 持久化方案、数据预热

  * 密码学

    - 对称加密

      - DES AES

    - 哈希加密

      - 验证文件完整性
      - 数字签名

    - 非对称加密

      - RSA等

    - 数字证书

      是数字认证机构下发的证书，包含了真实服务器的公钥和网站的其他信息，机构用自己的私钥加密后发给客户端（浏览器），客户端通过机构的公钥解密得到服务器的公钥等信息，这样客户端就能够与从服务器索要的公钥对比，判断服务器是否是可信的

    - 数字签名

      为验证报文的正确性（未被中途篡改）而设计。

      算法是S(H(M))，所以服务器发送的信息是M+数字签名

    - 如何保证通信的可靠性

      - 传输通道是可信的 <数字证书>，通道是可信的是保证数字签名和报文正确的基础
      - 内容未被篡改 <数字签名>
      - 内容是加密的 <非明文>

    - SSH secure shell，openssh

      - 解决计算机之间明文登录的问题
      - 密码登录过程一
        - a请求登录b
        - b将自己签发的公钥发给a
        - a使用公钥将登录密码加密后发给b
        - b使用私钥解密后是正确的登录密码，允许登录
      - 免密登录过程二
        - a将公钥放在需要登录的机器上
        - a请求b时，b发送一段随机字符串
        - a使用私钥加密后发送给b
        - b使用公钥解密解密后与随机字符串相等，允许登录
      - 注意：非对称秘钥对是自己生成的，没有CA公正，登录时需要手动确认主机公钥指纹的正确性

* 算法与数据结构

  - 算法设计、算法分析

    - 树

      递归、迭代、深度优先、广度优先、栈

    - 动态规划

      一般是设计一个表，后边的值依赖前边的值

  - 排序

  - 字符串

  - 数组

  - 链表

  - 哈希表

    例：大量IP地址的查找

    python 中 set dict 就是很好的 hash 表

  - 位运算

    - 优先级 ~、 >><<、&、  ^  |、
    - a | b & a = (a | b) & a = a, a & b | a = a, a ^ a = 0, 0 ^ a = a

  - 树

    - 通用树的概念：

      - 深度 高度 节点 根节点 叶子节点 

      * 度数：在树中，每个节点的子节点（子树）的个数就称为该节点的度（degree）。

      * 阶数：（Order）阶定义为一个节点的子节点数目的最大值。（自带最大值属性）
      * 先序遍历/深度优先/根-左-右、中序遍历、后序遍历、按层遍历/广度优先

    * 二叉树（最多两个子节点）、~~二叉堆~~、 Treap、二叉查找树(BST)、 平衡二叉树（AVL)、伸展树、  红黑树 ...

      * 二叉堆：将数组以二叉树的形式表示出来，满足根节点是数组的最大/小值

      * Treap：每一个节点随机对应一个优先级，通过旋转使满足父节点的优先级大/小于子节点的优先级

      * 二叉查找树：左子树的值小于节点值，右子树的值大于节点值

      * 平衡二叉树：是一个二叉查找树，满足任意子树到叶子节点的高度之差小于等于1

      * 红黑树：是一个接近的平衡二叉树

        （1）每个节点或者是黑色，或者是红色。
        （2）根节点是黑色。

        （3）每个叶子节点（NIL）是黑色。 [注意：这里叶子节点，是指为空(NIL或NULL)的叶子节点！]

        （4）如果一个节点是红色的，则它的子节点必须是黑色的。

        （5）从一个节点到该节点的子孙节点的所有路径上包含相同数目的黑节点。 

    * 增删节点，如何维持树规则

      * Treap
        * 增加：左旋右旋
        * 删除：只有一个子节点，直接删除，接上子节点；两个子节点，将要删除的节点旋转到只有一个子节点后，删除该节点
        * 二叉查找树
          * 增加：递归判断大小，添加节点
          * 删除：只有一个子节点，直接删除，接上子节点；两个子节点，找到最节点子节点后交换，删除被交换的节点
        * 平衡二叉树
          * 增加：同二叉查找树，添加节点后，找到最近失衡父节点，旋转以平衡子树高度
          * 删除：同二叉查找树，删除节点后，找到最近失衡父节点，旋转以平衡子树高度
        * 红黑树：
          * 增加：增加节点设置为红色，加后续不同case下的旋转操作，核心思路是将红色的节点移到根节点，然后将根节点设为黑色
          * 删除：将红黑树当作一颗二叉查找树，将节点删除，通过"旋转和重新着色"等一系列来修正该树，使之重新成为一棵红黑树。

    * 查询：O(lgn)

  * 多个子节点：B树 --> B+ --> B*

    * B 树，是一个多路平衡树，降低树的高度

      * 用阶定义

        1. 树中每个结点最多含有m个孩子（m>=2）；
        2. 除根结点和叶子结点外，其它每个结点至少有[ceil(m / 2)]个孩子（其中ceil(x)是一个取上限的函数）；
        3. 若根结点不是叶子结点，则至少有2个孩子（特殊情况：没有孩子的根结点，即根结点为叶子结点，整棵树只有一个根节点）；
        4. 所有叶子结点都出现在同一层，叶子结点不包含任何关键字信息(可以看做是外部接点或查询失败的接点，实际上这些结点不存在，指向这些结点的指针都为null)；
        5. 每个非终端结点中包含有n个关键字信息： (n，P0，K1，P1，K2，P2，......，Kn，Pn)。其中：
                a)   Ki (i=1...n)为关键字，且关键字按顺序升序排序K(i-1)< Ki。
                b)   Pi为指向子树根的接点，且指针P(i-1)指向子树种所有结点的关键字均小于Ki，但都大于K(i-1)。 
                c)   关键字的个数n必须满足： [ceil(m / 2)-1]<= n <= m-1。

      * 用度定义

      * 节点结构

        ```
        #define m 1024
        struct BTNode;
        typedef struct BTNode *PBTNode;
        struct BTNode{
        		int keynum;				 # 关键字数量
          	PBTNode parent;		# 父节点
        		PBTNode *childnode; # 子树指针向量
          	KeyType *key;			 # 关键字指针向量
        }
        ```

    - B +，与B树区别

      1. 所有的叶子结点中包含了全部关键字的信息，及指向含有这些关键字记录的指针，且叶子结点本身依关键字的大小自小而大的顺序链接；
      2. 所有的非终端结点可以看成是索引部分，结点中仅含有其子树根结点中最大（或最小）关键字
      3. 索引节点更小，磁盘I/O的读取效率、方便了范围查找和遍历

      - B \* 
        1. 在 B+ 树的基础上(所有的叶子结点中包含了全部关键字的信息，及指向含有这些关键字记录的指针)，B*树中非根和非叶子结点再增加指向兄弟的指针；*
        2. B \* 树定义了非叶子结点关键字个数至少为(2/3)*M，即块的最低使用率为2/3（代替B+树的1/2）

  * 堆栈队列

    - 队列

      队列是只允许在一端进行插入操作、而在另一端进行删除操作的线性表，是一种可以实现【先进先出】的存储结构。

      实现：数组 链表

    - 栈

      栈是限定仅在表尾进行插入和删除操作的线性表，是一种可以实现【先进后出】的存储结构。

      栈的应用：递归

      实现：数组 链表

    - 堆

      堆是一种经过排序的树形数据结构，每个节点都有一个值，通常我们所说的堆的数据结构是指二叉树。所以堆在数据结构中通常可以被看做是一棵树的数组对象。而且堆需要满足一下两个性质：
      （1）堆中某个节点的值总是不大于或不小于其父节点的值；
      （2）堆总是一棵完全二叉树。

      堆的存取是【任意】的

      实现：二叉树

