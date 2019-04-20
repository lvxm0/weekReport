服务端开发解决什么问题？

数据：存储-资料，信息，商品，文章，视频
传输-消息投递，转账，上传，下载

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

gem sources --remove https://rubygems.org/

gem sources --a https://gem.ruby-china.com/

gem sources -l

sudo gem install cocoapods

pod setup
