---
title: Mac下PostgreSQL完全删除
categories: postgresql
date: 2017-06-11 19:03:53
tags:
---

这里针对的是 9.3 版本。如是其他版下面的仅供参考。

1、如果是postgresql.app的形式，这个简单，跟其他app一样，删除app即可。

2、如果是使用installer图形界面方式安装的。则需要打开终端命令行。

3、执行

open /Library/PostgreSQL/9.2/uninstall-postgresql.app
     可能会提示你输入密码。
4、等待上一步执行完成后，删除postgresql文件夹       

sudo rm -rf /Library/PostgreSQL
     可能会提示你输入密码
5、删除配置文件

sudo rm /etc/postgres-reg.ini
     可能会提示你输入密码
6、在用户管理中删除postgresql的用户， 系统偏好设置－－》用户及用户组。
7、删除共享内存设置 （我没有做过特殊设置，所以我本机是没有这个文件的，如果有，可以删除。）
sudo rm /etc/sysctl.conf

原文链接：http://stackoverflow.com/questions/8037729/completely-uninstall-postgresql-9-0-4-from-mac-osx-lion