---
title: mysql调优
categories: mysql
date: 2017-09-13 02:10:07
tags:
---

# 执行顺序
mysql解析器对sql的加载顺序为
FROM -> JOIN -> WHERE -> GROUP BY -> HAVING -> SELECT -> DISTINCT -> ORDER BY -> LIMIT

# 调优语句
## explain
性能分析，可用desc代替

## show warnings
查看语句警告，可显示语句性能缺陷

## show engines
查看mysql所提供的存储引擎

## show variables like "%storage_engine%"
查看当前的默认存储引擎

## show profile
性能跟踪诊断(set profiling=1 打开)

## show processlist
显示正在运行的线程

## slow queries

# 工具
## mysqlsla
分析mysql慢日志的工具

## mysqldumpslow
mysql自带慢查询分析工具

# 配置
## show slow log
启用慢查询日志配置
slow_query_log=true  
slow_query_log_file = "/var/lib/mysql/slow.log"  
long_query_time=1  #超时时间(秒)
