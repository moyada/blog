---
title: 方法句柄
categories: java
date: 2017-12-25 13:09:16
tags:
---

jdk7提供了一种方法句柄(MethodHandle)的api，使得程序能够更加高效的以动态的方式调用方法，下面展示下该api的基本语法。


1. 首先使用MethodHandles的静态方法lookup()获取到MethodHandles.Lookup，这是用于获取方法句柄的对象。

2. 确定方法的返回值类型和参数类型，通过MethodType.methodType(Class<?> rtype, Class<?>[] ptypes)获取方法句柄的签名类型MethodType

3. 接着通过选择调用此方法的类来获取方法句柄，lookup.findVirtual(Class<?> refc, String name, MethodType type)

4. 句柄也获得了，就可以使用实例对象进行方法调用了，利用MethodHandle的invoke(Object... args)或invokeExact(Object... args)，第一个参数为实例对象，后面的参数为调用方法的传递参数。