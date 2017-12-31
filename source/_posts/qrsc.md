---
title: 浅入深出
categories: java
date: 2017-12-25 12:56:19
tags:
---

# Java
## HashMap的原理及其底层实现
## ArrayList和LinkedList的区别
## Java反射相关的内容
## Hashmap和ConcurrentHahmap区别（分段锁具体实现原理）
## volatile原理，锁原理
## 锁，乐观锁，悲观锁，synchronized原理，锁方法和锁对象的区别，锁静态和锁非静态的区别
## synchronzied和可重入锁的区别
## 队列相关问题
## mybatis怎么实现乐观锁，悲观锁
## 线程池相关(ThreadPollExecutor)
## jvm相关（内存模型，GC，分代算法）
## JVM调优

# Spring
## SpringMvc和SpringBoot的区别
## spring boot 加载原理
## spring加载原理
## Spring IOC原理及其具体实现
## spring 事物相关问题

# 数据库
## 数据库范式有哪些，具体原理和概念
## mysql默认隔离级别repeated read,介绍一下这个隔离级别相关的一些知识点。
## 数据库设计相关问题：如果有两张表的是多对多的关系，怎么处理比较合适？
## mysql乐观锁，悲观锁的实现机制。
## redis为什么是单线程的？请说出底层原理
## redis缓存实时更新问题
## redis sortedSet底层实现及其原理。
## 聚集索引和非聚集索引
## 多列索引和单列索引
## 数据库ACID
即原子性(Automicity)、一致性(Consistency)、隔离性(Isolation)、持久性(Durability)
* 原子性：是指事务必须是一个单元，不可拆分，只存在成功与失败两种状态，任何一个中间环节失败将导致全部失败。
* 一致性：与原子性是相辅相承的，数据在事务的层面上完成前和完成后是两个状态，失败时已经修改的数据需要回退，不能存在数据已经被修改。
* 隔离性：事务与事务之间不会互相影响。
* 持久性：操作一旦完成将永久的保存在数据库中。

# 分布式
## RPC相关概念及其原理
## RPC HTTP,Socket区别
## 分布式锁

# 操作系统
## 典型PC系统各种操作指令的大概时间
## 执行基本指令(execute typical instruction)
## 从一级缓存中读取数据
## 分支误预测(branch misprediction)
## 从二级缓存获取数据
## 互斥加锁/解锁(Mutex lock/unlock)
## 从主内存获取数据(fetch from main memory)
## 从内存中顺序读取1MB数据
## 从新的磁盘位置获取数据（随机读取）
## 从磁盘中顺序读取1MB数据

# 网络
## HTTP三次握手
## 通过1G bps 的网络发送2K字节
## 从美国发送一个报文包到欧洲再返回

# 设计模式
## 怎么才能设计一个高QPS的接口
## 秒杀怎么设计
## 什么是工厂模式，工厂模式的具体应用场景是什么？
## 如果有一个非常长的数组，其中有一个左括号，在数组的右边有一个右括号进行匹配，如果匹配成功，那么继续。如果匹配不成功，就记录下来。求第一次匹配不成功的数组的下标，如果匹配不成功，也继续，直到整个数组遍历完毕，求：有多少对括号不匹配。
