---
title: spring-boot
categories: Spring-Boot
date: 2017-02-23 00:17:37
tags:
---

# Spring Boot默认yml配置

```
server:
  port: 8080
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:3306/boot
    username: root
    password: root5770
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
 ```