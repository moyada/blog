---
title: linux资源查看
categories: linux
date: 2017-08-22 23:47:36
tags:
---

# cpu
## top
是基本的系统监控命令，可以查看到当前系统的总cpu，内存，负载情况，以及各个进程的情况。
在多核cpu的系统中可以按`1`切换每个逻辑cpu的情况。

## vmstat
是一种低开销的系统性能观察方式，并且还能够查看到cpu队列(r)、io阻塞(b)、中断被处理数目(in)、上下文切换数目(cs)

## dstat
dstat是实时地监控所有系统资源
常见选项
-l ：显示负载统计量
-m ：显示内存使用率（包括used，buffer，cache，free值）
-r ：显示I/O统计
-s ：显示交换分区使用情况
-t ：将当前时间显示在第一行
–fs ：显示文件系统统计数据（包括文件总数量和inodes值）
–nocolor ：不显示颜色（有时候有用）
–socket ：显示网络统计数据
–tcp ：显示常用的TCP统计
–udp ：显示监听的UDP接口及其当前用量的一些动态数据
–output file [-cdn]：输出监控情况为文件保存，可用-cdn设置保存csv

常见插件
-–disk-util ：显示某一时间磁盘的忙碌状况
-–freespace ：显示当前磁盘空间使用率
-–proc-count ：显示正在运行的程序数量
-–top-bio ：指出块I/O最大的进程
-–top-cpu ：图形化显示CPU占用最大的进程
-–top-io ：显示正常I/O最大的进程
-–top-mem ：显示占用最多内存的进程

## iostat、mpstat、sar
使用 yum install sysstat 安装就会有这些功能

iostat是监控物理设备的 I/O 负载情况

常见用法
iostat -d -k 1	          #查看TPS和吞吐量信息(磁盘读写速度单位为KB)
iostat -d -m 1            #查看TPS和吞吐量信息(磁盘读写速度单位为MB)
iostat -d -x -k 1	      #查看设备使用率（%util）、响应时间（await） iostat -c 1 10 #查看cpu状态

mpstat是实时系统监控工具，并且可以查看多核心cpu中每个计算核心的统计数
常见用法
mpstat -P ALL 1			  #查看整体cpu与各个cpu的使用情况

sar是对系统的活动进行监控报告，包括文件读写、系统调用、磁盘I/O、CPU效率、内存使用、进程活动、IPC
常见选项
-A：所有报告的总和

-u：输出CPU使用情况的统计信息

-v：输出inode、文件和其他内核表的统计信息

-d：输出每一个块设备的活动信息

-r：输出内存和交换空间的统计信息

-b：显示I/O和传送速率的统计信息

-a：文件读写情况

-c：输出进程统计信息，每秒创建的进程数

-R：输出内存页面的统计信息

-y：终端设备活动情况

-w：输出系统交换活动信息

-o file:将命令结果以二进制格式存放在文件中，file 是文件名

常见用法
sar -uq					#查看CPU使用情况
sar -brw				#查看内存使用情况
sar -bud				#查看I/O使用情况

# 内存

## pmap
用于显示一个或多个进程的内存状态，需要带上进程端口号

# 流量

## nicstat
监控网卡及网络流量的工具

## netstate
用于显示各种网络相关信息

常见参数
-a (all)显示所有选项，默认不显示LISTEN相关
-t (tcp)仅显示tcp相关选项
-u (udp)仅显示udp相关选项
-n 拒绝显示别名，能显示数字的全部转化成数字。
-l 仅列出有在 Listen (监听) 的服務状态

-p 显示建立相关链接的程序名
-r 显示路由信息，路由表
-e 显示扩展信息，例如uid等
-s 按各个协议进行统计
-c 每隔一个固定时间，执行该netstat命令。

## ifstat
是一个统计网络接口活动状态的工具
## iftop
## nmap



