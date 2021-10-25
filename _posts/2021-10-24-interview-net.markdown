---
layout: post
title:  "面试总结-网络"
date:   2021-10-24 18:01:50 +0800
categories: 面试
---

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

          ![token](../_source/token.png)

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

      ![SSL过程](../_source/SSL过程.gif)

    * websocket

      通过单个`TCP`连接提供全双工通信通道

      借用HTTP协议进入初始握手，建立连接后，以TCP协议进行读取/写入数据

      客户端请求头：

      ```
      GET /chat HTTP/1.1
          Host: server.example.com
          Upgrade: websocket
          Connection: Upgrade
          Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
          Sec-WebSocket-Protocol: chat, superchat
          Sec-WebSocket-Version: 13
          Origin: http://example.com
      ```

      服务器响应头

      ```
      HTTP/1.1 101 Switching Protocols
          Upgrade: websocket
          Connection: Upgrade
          Sec-WebSocket-Accept: HSmrc0sMlYUkAGmm5OPpG2HaGWk=
          Sec-WebSocket-Protocol: chat
      ```

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

      ![TCP报文](../_source/TCP报文.jpg)

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

        ![TCP连接三次握手](../_source/TCP连接三次握手.jpg)

        ![TCP连接四次挥手](../_source/TCP连接四次挥手.jpg)

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

      ![UDP数据格式](../_source/UDP数据格式.png)

    * 是如何工作的

      UDP数据有可能在网络层被分片发送，在接收端根据分片标志组装数据，如果组装后的数据长度与UDP首部的长度字段的值对不起来，则丢弃整个UDP数据报

* 网络层

  * 负责分组交换网中不同主机间的通信，IP协议；
    作用：发送数据时，将传输层中的报文段或用户数据报封装成IP数据报，并选择合适路由

  * 传输单位：**数据报 datagram**

  * 设备：路由器

  * 数据格式

    ![IP协议数据格式](../_source/IP协议数据格式.jpeg)

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

  ![TCP:IP数据包](../_source/TCP:IP数据包.jpg)

* and：

  * DNS（Domain Name System）

  * CDN (Content Delivery Network)

    通过在网络各处放置节点服务器，实时地根据网络流量和各节点的连接、负载状况以及到用户的距离和响应时间等综合信息将用户的请求重新导向离用户最近的服务节点上

    ![CDN原理](../_source/CDN原理.jpg)

  * ddos攻击原理

    利用网络分层各个协议的特点(分布式)使服务器拒绝服务的目的

    SYN-Flood: 发送大量syn连接，但并不完成握手连接的第三步，服务器会不停的重试并等待一定的时间后（sync timeout）放弃这个未完成的连接，最终服务器的网络资源和带宽被这种大量的半连接消耗殆尽

    ACK-Flood：建立连接传输数据时，数据包四元组故意设计错误，服务器发送RST，导致重新建立连接

    UDP-Flood：UDP协议是无连接性的，服务端只要开了一个UDP的端口提供相关服务的话，那么就可用大量UDP小包冲击（DNS服务器或Radius认证服务器、流媒体视频服务器）
    Connection-Flood、HTTP-GET、UDP DNS Query Flood

  * 网络安全

    ![网络安全](../_source/网络安全.png)

    * CSRF（Cross Site Request Forgery）

      - 登录受信任网站A，并在本地生成Cookie。

      - 在不登出A的情况下，访问危险网站B

      - 解决：服务端在set-cookie中设置随机的anti-csrf-token

        ![csrf](../_source/csrf.jpg)

    * XSS（cross-site scripting attacks）

      * 攻击者通过代码注入（code injection）的方式，恶意篡改受害者所访问的网站内容；把受害者引导到恶意网站；窃取受害者cookie

        ![stored xss](../_source/stored xss.png)

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