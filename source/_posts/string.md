---
title: Java 8 String的变化
categories: java
date: 2017-04-04 17:52:48
tags:
---

# 不再共享char[] 数组
这个改动实际是从1.7.0_06开始发生的。String 内部是保存了一个char[]，用来记录String 中的字符串。在之前的实现中，String 有四个Field: 

char[] value;
int offset;
int count;
int hash;
由于String 的不变行，我们可以在不同的String 之间，共用一个char[] value，每个String 有自己独立的offset 和 count. 这样，对于subString, trim(), split() 等这样的操作，创建新的String对象的时候，仍然使用原来那个String 的value，从而可以做到O(1) 的时间复杂度。
这种方法的缺点是对内存回收有严重的影响。当从很长的字符串中截取一个很短的字符串时，虽然需要使用的只是很少的字符，但是却引用了一个很大的char[] 数组，不能释放。

新的String 实现，去掉了String 中的offset 和 count 这两个field。

char[] value;
int hash;
一眼看上去对于每个String 省掉了两个int 共8 个字节的开销，当然这点开销是微不足道的。最重要的影响，是String 之间无法共享char [] value 了。造成的后果是:subString, trim, split 等操作需要创建新的数组，并进行内存拷贝，时间复杂度变成O(n) ! 得到的好处则是char[] value 的生命周期变短，能够及时释放不需要的内存，改善内存占用和GC 性能 。

# 常量池的变化
String 不变性的另外一个可以优化的地方，就是常量池(String pool)，对于短小的String对象，保存到String pool中，如果遇到同样的String，则只要复用String pool中的实例就可以了 。编译器只能处理常量的String，将其放到String pool中，如果需要自己操作，则可以用String.intern()方法。

在Java6 中，String Pool 是放在PermGen 区域的。PermGen 区域的问题是一般容量比较小，以及GC 困难。

在Java7 中，String Pool 被移到了Heap 中，一方面可以使用更大的String Pool了(通过XX:StringTableSize设置)，另外一方面，也为Java8 彻底移除PermGen 做准备。由于移到了Heap 中，如果String pool 中的一个String，没有被任何实例所引用，是会被GC 回收的。

从Java7 update40 之后，String Pool 的默认大小由1009 提高到了60013。这是一个相当大的数字。

# 字符串去重
String deduplication 是Java8u20 之后引入的一个特性，这个特性完全是JVM 层面的。要使用这个特性，必须使用G1 garbage collector，通过设置参数:

-XX:+UseStringDeduplication -XX:+PrintStringDeduplicationStatistics
开启字符串去重功能。
开启了这个功能之后，JVM 在GC的时候，会顺便通过hashCode和value，来比较两个字符串是否相等。如果相等的话，就会把其中一个字符串的value 字段，指向另外一个字符串，这样同样的字符串最后只会使用一个char[] value。

参考: http://www.dongliu.net/post/5778384575528960

---

原文链接: https://mp.weixin.qq.com/s?__biz=MzIxNDQ5NzI4OA==&mid=2247484091&idx=1&sn=6b4d0d16e1ab0846b67be825285d7176&chksm=97a7e3dca0d06aca452752d2482fe5565e2292fe16888315d9473f756917839427999b19f609#rd


