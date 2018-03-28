---
layout: post
title: 了解 memcache，mongoDB，Redis
excerpt: redis：基于内存也可持久化的Key-Value关系型数据库。MongoDB：基于分布式文件存储。memcache：分布式的高速缓存系统
category: backend
---

#### NoSQL
non-relational，Not Only SQL

如 memcache，mongoDB，Redis 等数据库

[nosql--简介](https://blog.csdn.net/guchuanyun111/article/details/52056899)

[关于NoSQL与SQL的区别](https://blog.csdn.net/xlgen157387/article/details/47908797)

- Redis

    适用于对读写效率要求都很高，数据处理业务复杂和对安全性要求较高的系统（如新浪微博的计数和微博发布部分系统，对数据安全性、读写要求都很高）。
- MongoDB

    主要解决海量数据的访问效率问题。
- Memcached

    动态系统中减轻数据库负载，提升性能；做缓存，适合多读少写，大数据量的情况（如人人网大量查询用户信息、好友信息、文章信息等）；
　　用于在动态系统中减少数据库负载，提升性能;做缓存，提高性能（适合读多写少，对于数据量比较大，可以采用sharding）。

[Redis、MongoDB及Memcached的区别](https://www.cnblogs.com/boazy/p/Redis.html)

[Redis 和 Memcached 优缺点，主要应用场景？--知乎](https://www.zhihu.com/question/19829601)
　
#### Memcached（内存Cache）

　　是一个高性能的分布式内存对象缓存系统，用于动态Web应用以减轻数据库负载。它通过在内存中缓存数据和对象来减少读取数据库的次数，从而提高动态、数据库驱动网站的速度。Memcached基于一个存储键/值对的hashmap。其守护进程（daemon）是用C写的，但是客户端可以用任何语言来编写，并通过memcached协议与守护进程通信。

#### MongoDB（NoSQL数据库）

MongoDB是一个跨平台，面向文档的数据库，提供高性能，高可用性和易于扩展。MongoDB是工作在集合和文档上一种概念。

MongoDB中有三元素：数据库，集合，文档，其中“集合”就是对应关系数据库中的“表”，“文档”对应“行”

#### redis（内存数据库）

[概述](https://blog.csdn.net/guchuanyun111/article/details/52057407) ， 
[相关问题](https://blog.csdn.net/guchuanyun111/article/details/52064870)

Redis本质上是一个Key-Value类型的内存数据库，很像memcached，整个数据库统统加载在内存当中进行操作，定期通过异步操作把数据库数据flush到硬盘上进行保存。

优点：

    因为是纯内存操作，Redis的性能非常出色，每秒可以处理超过 10万次读写操作，是已知性能最快的Key-Value DB。

    支持保存多种数据结构，单个value的最大限制是1GB

缺点：

    受到物理内存的限制，不能用作海量数据的高性能读写


#### Redis 和 mysql 区别

- mysql：中小型的网络数据库，比oracle和sqlserver小，但是并发能力远超过acess这样的桌面数据库。
- redis：支持网络、可基于内存亦可持久化的日志型、Key-Value数据库。

可以认为redis比mysql简化很多。mysql支持集群。

现在大量的软件使用redis作为mysql在本地的数据库缓存，然后再适当的时候和mysql同步


