---
title: ConcurrentHashMap分析
categories: java
date: 2018-05-26 14:32:42
tags:
---

ConcurrentHashMap是jdk提供的针对并发环境下的集合类容器，功能与HashMap类似。
在JDK8的时候进行了大规模的改进，这里真对JDK7与JDK8做下简单的对比。

在JDK7版本中是利用分段锁(Segment)和ReentrantLock类实现并发控制的:

内部维护了一组Segment，通过对key进行hash获取相应的Segment，Segment内部维护了一组HashEntry，类似HashMap结构，并且在并发修改阶段对管程进行加锁控制。

而JDK8中ConcurrentHashMap具体的变化有：

* 不在使用分段锁，而是恢复成与HashMap类似的结构。

* 对hash算法进行改进，使用高位移到低位异或，避免哈希碰撞。原因是jdk的哈希寻址是使用低位，而有些数据的哈希值差异主要在高位。

* 使用链表加红黑树的数据结构进行存储，哈希碰撞时使用链表，当链表长度大于8时，将当前链表的数据结构变形为红黑树，这样做的目的是为了解决当链表长度过大而造成的查询开销。

* 对数据的修改使用了`CAS`和`sychronized`进行并发控制，原因是sychronized在近代JVM中已经经过大量优化，性能与ReentrantLock差距不大，并且放弃使用ReentrantLock能够节省更多的内存开销。

* 在JDK7中size()方法使用的是遍历Segment加锁获取，在获取大小的时候会影响数据的修改。JDK8中使用了类似LongAdder底层的存储结构，分段存储大小，并且进行了缓存行填充。

* 存储数据使用`volatile`来保证可见性。

* 避免初始开销，延迟加载数据结构。