---
title: Linux学习
categories: linux
date: 2017-03-12 09:48:56
tags:
---
# 系统
## 启动过程
1. load bios(hardware information)
2. read MBR's config to find out the OS
3. load the kernel of the OS
4. init process start
5. execute /etc/rc.d/sysinit
6. start other modules (etc/modules.conf)
7. excute the run level scripts
8. execute /etc/rc.d/rc.local
9. execute bin/login
10. shell started

## 启动级别(run level)
* 0 - 系统停机状态
* 1 - 单用户工作状态
* 2 - 多用户状态(没有NFS)
* 3 - 多用户状态(有NFS)
* 4 - 图形界面
* 5 - 系统正常关机并重新启动

---
# 命令
## monut

命令格式：
&emsp;mount [-t vfstype] [-o options] device dir
&emsp;unmount device

# 技巧
## 快捷删除已输入命令
&emsp;ctrl + w 或 esc + delete 删除一个单词的命令
<br/>
&emsp;ctrl + u 清空全部命令

## 光标定位
&emsp;ctrl + a 定位到句首
&emsp;ctrl + e 定位到句尾
