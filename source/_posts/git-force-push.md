---
title: Git操作：完全替换分支
categories: git
date: 2017-06-11 19:07:30
tags:
---

# 场景
一个是master分支，一个是develop分支，两个分支有冲突且数量大。

# 目标
将develop分支内容替换master分支

# 方法

* 1、如果无需关心本地分支内容则可以 git push origin develop:master -f 将develop分支强行推送至master分支
* 2、用git checkout master切换至master分支，使用git reset --hard develop ，先将本地的master分支重置成develop
使得本地master分支与develop分支同步，然后再 git push origin master --force ，推送到远程仓库，

# 注意
强制推送是不安全的操作，如果有对master分支进行保护，则需要先关闭分支保护才能成功推送