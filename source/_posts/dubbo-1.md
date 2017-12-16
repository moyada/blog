---
title: dubbo学习1-注册中心
categories: dubbo
date: 2017-04-04 10:34:34
tags:
---

# 注册中心
注册中心是为了服务提供者的注册与发现，在提供者与消费者之间起了协调作用
dubbo使用的是[zookeeper](https://xueyikang.github.io/2017/06/20/zookeeper-init/)作为注册中心使用

使用dubbo-admin这个项目作为管理控制台管理注册的服务。

如果项目使用jdk8来构建，则需要对dubbo-admin的依赖进行调整:

1. webx的依赖改为3.1.6版以上
```
<dependency>
    <groupId>com.alibaba.citrus</groupId>
    <artifactId>citrus-webx-all</artifactId>
    <version>3.1.6</version>
</dependency>
 ```

2. 添加velocity的依赖
```
 <dependency>
    <groupId>org.apache.velocity</groupId>
    <artifactId>velocity</artifactId>
    <version>1.7</version>
</dependency>
 ```

3. 对依赖项dubbo添加exclusion，避免引入旧spring

```
 <dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>dubbo</artifactId>
    <version>${project.parent.version}</version>
    <exclusions>
        <exclusion>
            <groupId>org.springframework</groupId>
            <artifactId>spring</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```
 

4、webx已有spring 3以上的依赖，因此注释掉dubbo-admin里面的spring依赖
```
 <!--<dependency>-->
    <!--<groupId>org.springframework</groupId>-->
    <!--<artifactId>spring</artifactId>-->
<!--</dependency>-->
```

确保spring的依赖一致后打war包，替换tomcat的ROOT目录
保证zookeeper的路径正确后启动tomcat将可以正确访问dubbo的管理界面
