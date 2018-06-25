---
title: dubbo学习2-构建服务jar包
categories: dubbo
date: 2017-06-25 18:37:44
tags:
---

使用dubbo服务的运行方式大致有三种：Servlet容器、Main方法(Spring容器)、Dubbo提供的Main方法(Spring容器)。
在这些方式中推荐使用的是第三种，因为这种方式提供了JDK的ShutdownHook完成优雅关机.

服务提供方：
	停止时，先标记为不接收新请求，新请求过来时直接报错，让客户端重试其它机器。
	然后，检测线程池中的线程是否正在运行，如果有，等待所有线程执行完成，除非超时，则强制关闭。
服务消费方：
	停止时，不再发起新的调用请求，所有新的调用在客户端即报错。
	然后，检测有没有请求的响应还没有返回，等待响应返回，除非超时，则强制关闭。

优雅关机超时强制关闭的配置为
```
<dubbo:application ...>
    <dubbo:parameter key="shutdown.timeout" value="60000" /> <!-- 单位毫秒,缺省时间为10秒 -->
</dubbo:application>
```