---
title: dubbo原理学习
categories: 随笔
date: 2018-05-30 21:06:58
tags:
---

dubbo项目按功能分层，各司其职。支持ShutdownHook优雅关机。

* 接口层
给服务提供者和消费者实现

* 配置层
对dubbo服务框架进行各种配置

* 代理层
服务代理，透明生成客户端stub和服务端skeleton，将provider和consumer的请求生成代理，代理之间进行网络通信

* 注册层
负责服务的注册与发现，将provider作为服务注册到注册中心，consumer可以从注册中心查询所需调用的服务

* 集群层
封装多个服务提供者的路由以及负载均衡，将多个实例组合成一个统一的服务提供

* 监控层
对consumer调用provider的调用次数和调用时间进行监控，并且提供管理后台

* 远程调用层
封装provider和consumer之间的具体rpc调用，目前支持的方式有netty、netty4、mina、http、grizzly、p2p、zookeeper

* 信息交换层
封装请求响应模式，同步转异步

* 网络传输层
抽象tcp数据传输为统一接口，目前dubbo自身支持的协议有dubbo、hessian、http、injvm、memcached、redis、rmi、thrift、webservice

* 数据序列化层
对传递的数据进行序列话和反序列化，目前包含的序列话方式有fastjson、fst、hessian2、jdk、kryo


# 工作流程

## provider
1. 连接注册中心

2. 将提供服务的方法和配置写入注册中心

3. 生成动态代理，监听来自consumer的代理请求

4. 根据请求信息调用代理方法

5. 对收到的调用请求进行监控

## consumer
1. 连接注册中心

2. 通过注册中心获取对应的provider信息

3. 通过代理获取获取provider可用地址

4. 选择负载均衡算法访问服务机器

5. 代理对provider方法请求，根据协议序列化请求，通过netty发送数据

6. 服务方收到请求反序列化，调用请求方法代码返回数据


