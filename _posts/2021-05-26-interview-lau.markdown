---
layout: post
title:  "语言"
date:   2021-10-24 18:01:50 +0800
categories: 面试
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

        ![迭代器生成器](https://github.com/cyanzhao/cyanzhao.github.io/raw/master/_source/迭代器生成器.png)

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

          ![django_csrfmiddleware](https://github.com/cyanzhao/cyanzhao.github.io/raw/master/_source/django_csrfmiddleware.png)

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

        ![types-of-mounts](https://github.com/cyanzhao/cyanzhao.github.io/raw/master/_source/types-of-mounts.png)

      * 网络

        * 单机

          * 通信方式

            Bridge、Host、Container、None

            ![docker_network_bridge](https://github.com/cyanzhao/cyanzhao.github.io/raw/master/_source/docker_network_bridge.png)

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

          ![ks8架构](https://github.com/cyanzhao/cyanzhao.github.io/raw/master/_source/ks8架构.jpg)

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

      ![celery](https://github.com/cyanzhao/cyanzhao.github.io/raw/master/_source/celery.png)

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

        ![微服务平台的内容](https://github.com/cyanzhao/cyanzhao.github.io/raw/master/_source/微服务平台的内容.jpg)

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