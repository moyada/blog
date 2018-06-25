---
title: 使用 Eclipse Memory Analyzer (MAT) 分析内存泄漏
categories: java
date: 2018-05-23 10:18:19
tags:
---

使用MAT分析内存泄漏主要是检测对象是否可达，是否无用。

通过jmap -dump:format=b,file=[file_name]] [pid] 生成`hprof`文件

在Eclipse Marketplace安装 Memory Analyzer，打开dump文件

* 注意: dump 导出后的内存跟实际监控看到的内存大小不一致的时候, 有可能是使用了`堆外内存`
    
# Overview 面板
Remainder(剩余) 应用 Heap 可分配的内存, 如果可分配内存很小, 则可以考虑加大或者进行优化

# Histogram 面板 (类 的角度)
可以查看内存中实例的数量以及占用内存的大小
            
# Dominator Tree 面板 (对象实例 的角度)
按照占用内存由大到小的顺序列举了对象列表情况
    
# Top Consumers 面板 (按类和包进行分组分析大消耗对象)
    
# Leak Suspects 面板 (内存泄漏分析报表)
    
# Shallow size 是指对象本身占用内存的大小, 不包含对其他对象的引用
# Retained size 是指 Shallow size + 该对象能直接或者间接访问到的对象的 Shallow size 之和，也就是指 该对象被 GC 之后所能回收的内存的总和


# List Objects

## with incoming references
查看这个对象持有的外部对象引用

## with outcoming references
查看这个对象被哪些外部对象引用

# Path To GC Roots

## exclude weak references
排除 软引用

## exclude soft references
排除 弱引用

## exclude weak/soft references
排除 软／弱引用

## exclude all phantim/weak/soft etc. references
查看 强引用

# GC root Unreachable
没有引用标记, 会被回收, 不会产生 leak, 由于没有 GC 发生所以没有被释放

> 参考文档:
> [MAT 使用进阶](http://www.jianshu.com/p/c8e0f8748ac0)
> [美团技术  Linux 与 JVM 的内存关系](https://tech.meituan.com/linux-jvm-memory.html)
> [Java内存泄漏分析](http://www.javatang.com/archives/2017/11/08/11582145.html)
> [追踪 Netty 异常占用堆外内存的经验分享](https://zhuanlan.zhihu.com/p/21741364)