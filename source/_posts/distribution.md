---
title: 分布式理论
categories: 分布式
date: 2017-12-22 18:23:45
tags:
---


分布式系统常见的问题有：

1. 消息传递异步无序(asynchronous): 现实网络不是一个可靠的信道，存在消息延时、丢失，节点间消息传递做不到同步有序(synchronous)
2. 节点宕机(fail-stop): 节点持续宕机，不会恢复
3. 节点宕机恢复(fail-recover): 节点宕机一段时间后恢复，在分布式系统中最常见
4. 网络分化(network partition): 网络链路出现延迟等问题，将N个节点隔离成多个部分，出现网络局部小集群
5. 拜占庭将军问题(byzantine failure)[2]: 节点或宕机或逻辑失败，甚至不按套路出牌抛出干扰决议的信息
6. 节点动态增减(dynamic increase)：数据的复制以及负载均衡的重新计算