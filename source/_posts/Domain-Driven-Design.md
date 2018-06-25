---
title: 领域驱动入门
categories: 随笔
date: 2018-05-01 23:54:02
tags:
---

在传统设计中我们经常将实体类分为DO、DTO、PO、BO、VO、QTO等等，几乎每一个类都有其对应的模型，当业务模型发生变化时也将牵涉不少的变更。


领域驱动(DDD)设计与传统设计中方向则截然不同。

其主要将模型分成四层:

- UI层：负责界面展示。
- 应用层(Application Layer)：负责程序与外界的交互。
- 领域层(Domain Layer)：负责领域逻辑，包括数据库持久，业务领域模型。
- 基建层(Infrastructure Layer)：负责提供基础底层操作。

领域驱动包含的属性：扩展度、灵活度、与组织的对应度

> https://www.jianshu.com/p/b6ec06d6b594
> https://tech.meituan.com/DDD%20in%20practice.html