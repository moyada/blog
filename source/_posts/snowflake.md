---
title: snowflake
categories: 算法
date: 2017-03-14 18:30:16
tags:
---

# 简介
&emsp;SnowFlake是Twitter的一个分布式系统64位自增id算法，可以满足基本有序。

这个id主要分为4个部分：
* 1位标记是否使用
* 数字时间戳
* 集群内每台机器的唯一标识
* 递增序号