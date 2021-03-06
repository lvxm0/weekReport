# week6 周报
16340164 吕雪萌

第六周主要讲解了关于移动应用开发--服务端开发的知识。

服务器端开发是开发者中技术性偏强，对逻辑思维要求更高的一个细分方向，处理的只有逻辑和业务。


## 服务端开发解决问题

  - 数据：存储用户资料，信息，商品，文章，视频
  - 传输：消息投递，转账，上传，下载

## 服务端开发技术栈
  ![](https://pic4.zhimg.com/80/v2-69a21823782827c0905fa83ef606caa4_hd.jpg)

## 服务端构架
  ![](https://github.com/lvxm0/weekReport/blob/master/week6_2.PNG)
  
### 基础设施

  - 操作系统
  - 网络设施
  - 机房机架

### 接入层

  - [反向代理请求nginx](https://www.jianshu.com/p/bed000e1830b)
    - 概念：指以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个服务器。
    - 过程：客户端向反向代理的命名空间中的内容发送普通请求，接着反向代理服务器将判断向何处(原始服务器)转交请求，并将获得的内容返回给客户端，就像这些内容原本就是它自己的一样。
    - 与正向代理（代理服务器）对比：反向代理服务器和原始服务器在一个网络中，而正向代理中代理服务器和原始服务器在不同网络。
    
    ![](https://upload-images.jianshu.io/upload_images/2062729-a25b69ba0dd3e6cf.jpg?imageMogr2/autoorient/strip%7CimageView2/2/w/550/format/webp)
    
  - [负载均衡](https://www.jianshu.com/p/4af1940bd251)
    - 概念：负载均衡可以将请求前端的请求分担到后端多个节点上，提升系统的响应和处理能力。
    - [负载均衡策略](https://www.jianshu.com/p/d7e173d212a8)
      - 轮询：轮询是一种很简单的实现，依次将请求分配给后端服务器。优点就是实现简单，请求均匀分配。缺点也恰恰在于请求均匀分配，因为后端服务器通常性能会有差异，所以希望性能好的服务器能够多承担一部分。也不适合对长连接和命中率有要求的场景。
      
      - 随机: 随机把请求分配给后端服务器。请求分配的均匀程度依赖于随机算法了，因为实现简单，常常用于配合处理一些极端的情况，如出现热点请求，这个时候就可以random到任意一台后端，以分散热点。当然缺点也不言而喻。
      
      - 权重: 加权轮询就是一种改进的轮询算法，轮询算法是权值相同的加权轮询。需要给后端每个服务器设置不同的权值，决定分配的请求数比例。这个算法应用就相当广泛了，对于无状态的负载场景，非常适合。优点解决了服务器性能不一的情况，缺点是权值需要静态配置，无法自动调节。也不适合对长连接和命中率有要求的场景。

      - hash: 根据Source IP、 Destination IP、URL、或者其它，算hash值或者md5，再采用取模。
      - 一致性hash: 具体可见[链接](https://www.jianshu.com/p/d7e173d212a8)
   
### 逻辑层

  - 作用： 根据业务逻辑，处理用户请求，得到结果
  - 设计模式
    - 单体式服务：所有的业务逻辑集中在一个代码仓库里，一个进程里。
      - 优点: 简单，适合协作人数少，功能较少的项目; 更新维护方便
      - 缺点: 难以支撑高复杂度系统
      
    - 微服务：业务逻辑按功能/类型划分，分别部署多个服务，使用网络通信交换数据
      - 优点：多人团队协作成本更低; 功能复用; 系统解耦; 故障隔离
      - 缺点：系统复杂度大增, 对业务流程设计提出更高的要求; 对网络，基础框架要求更高。
      
      
### 存储层

- 作用：数据的存储，查找，转发，支持模糊查询
- [常用的存储](https://www.jianshu.com/p/23e00d5c705a)
  - SQL类关系型存储
    - 典型： MYSQL/ORACLE
    - 核心知识点：索引; 事务与事务隔离级别;存储方法;触发器;不同关系型数据库的SQL差异
    
  - NoSQL类Key-Value型存储
    - 典型：redis/memcache
    - 适用场景
      - 分布式session存储，数据缓存，分布式锁，简单的消息队列，计数器（累加器）
      
  - 分布式文件系统：文件存储本身，做平台用
    - 典型：网易nos/阿里云oss
    - 适用场景
      - 存储需要在分布式架构下共享的文件对象：如前端静态文件，图片资源，大文件资源等
      
  - 队列存储：核心任务存储
    - 典型：rabbitmq/kafka
    - 适用场景
      - 消息中间件一般适用于：系统之间需要高度解耦；异步传输数据（非阻塞）；保证数据传输过程中数据可以持久化，解决生产端和消费端能力不一致的问题，避免峰值数据对下游服务节点的冲击等
      - 对比rabbitmq和kafka: rabbitmq主要适用数据可靠性，一致性要求比较高的场景；kafka主要适用数据吞吐量比较的场景，如日志数据的实时传输等
  
### 支撑系统

  - 日志系统
  - 监控报警
  - 运维系统


## 基础知识
常用技术名词

### HTTP/HTTPS

- HTTP: 超文本传输协议，数据交换协议的一种。HTTP基于TCP，是万维网数据通信的基础。
- HTTPS: TLS加持的HTTP。提供网站服务器身份认证; 保护数据交换的隐私以及完整性校验

### 数据封装与交换

- 数据格式
  - JSON
  - XML
  - Protobuf

### RPC远程过程调用

- [概念](https://www.jianshu.com/p/2176b75f44d4)：封装网络收发/传输协议，专注业务逻辑开发
  - RPC从一台机器（客户端）上通过参数传递的方式调用另一台机器（服务器）上的一个函数或方法（可以统称为服务）并得到返回的结果。
  - RPC 会隐藏底层的通讯细节（不需要直接处理Socket通讯或Http通讯）
  - RPC是一个的请求响应模型，客户端发起请求，服务器返回响应（类似于Http的工作方式）。
  - RPC 在使用形式上像调用本地函数（或方法）一样去调用远程的函数（或方法）。
  
- Thrift, GRPC
  - 初代 RPC 技术的跨语言面向对象的回归
  - 仍然需要通过中间语言来编写类型和接口定义
  - 需要用代码生成器将中间语言翻译成占位程序（stub）
  - 需要基于生成的服务器代码来单独编写服务
  - 修改更新不方便

## 服务端开发入门

- 学习链接
  - [服务端开发](https://www.w3cschool.cn/windwant/)
  - [Swift构建App服务器端](http://www.cocoachina.com/ios/20180108/21767.html)
  - [快速搭建模拟服务器](http://www.cocoachina.com/ios/20151105/14074.html)


## 心得体会
对服务端有了概念上的理解，但是如何搭建服务器，如何选择语言，还是一个需要自己去探索的问题



计算
数据计算：算法层面：Feeds推荐
云端服务器对客户端计算力的补充

技术栈：（图片）
业务垂直：结构逻辑数据层

技术设施：支持系统：

垂直看”
接入层：反向代理
负载均衡：轮询，负载，
逻辑层：
存储层：数据存储转发查找 支持模糊查询
分布式文件系统：文件存储本身，做平台用
队列存储：核心任务存储

服务端技术介绍
常用技术名词：
-	HTTP/HTTPS
HTTP: 数据传输协议，浏览器服务器传输纽带 1.1 2.0 规划3.0 明文传输 不安全
HTTPS：提供网站服务器身份认证（网站是对的不是仿造），保护数据交换的隐私以及完整性校验（加了一个传输层：TLS层）

数据封装交换
JSON,XML, Protobuf

Rpc 远程过程调用

信息队列：解耦业务逻辑，消峰限流

业务系统设计：



链接：
https://www.jianshu.com/p/929efb69b363
http://code.cocoachina.com/view/135384

https://www.jianshu.com/p/f43b5964f582

https://juejin.im/entry/5c067eb56fb9a04a0a5ef583

[UI源码](http://code.cocoachina.com/view/135384)

sudo gem sources --remove https://rubygems.org/

sudo gem sources --a https://gems.ruby-china.com/

gem sources -l

sudo gem install cocoapods

sudo pod setup
