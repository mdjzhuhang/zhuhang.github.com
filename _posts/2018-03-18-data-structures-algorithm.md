---
layout: post
title: Data Structures and Algorithm
excerpt: to be continues… <br> consistent hashing
Category: others
---

#### consistent hashing

[三分钟看懂一致性哈希算法](http://blog.csdn.net/gerryke/article/details/53939212)

一致性哈希算法 consistent hashing，作为分布式计算的数据分配参考，比传统的取模，划段都好很多。

最关键的区别就是，对节点和数据，都做一次哈希运算，然后比较节点和数据的哈希值，数据取和节点最相近的节点做为存放节点。这样就保证当节点增加或者减少的时候，影响的数据最少，减少数据的迁移规模。

当后端是缓存服务器时，经常使用一致性哈希算法来进行负载均衡。

使用一致性哈希的好处在于，增减集群的缓存服务器时，只有少量的缓存会失效，回源量较小。

[consistent hashing](http://blog.csdn.net/sparkliang/article/details/5279393)：环形 hash 空间，平衡性，虚拟节点

[分布式存储和一致性哈希](http://blog.csdn.net/xiaqunfeng123/article/details/51668409)
【对象--->服务器】……>【对象--->虚拟节点---> 服务器】

