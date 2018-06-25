---
title: sbt-repository
categories: scala
date: 2017-05-03 16:29:58
tags:
---

国内使用sbt也需要像maven一样设置一些国内的代理仓库

sbt修改镜像会稍微麻烦一些，需要对sbt-launch.jar解压后修改配置文件，再重新打包。

需要修改其中的 ./sbt/sbt.boot.properties 文件，
将 [repositories] 标签处修改为如下内容

```
  local
  # 公司仓库
  company: https://repo.souche-inc.com/repository/public/
  # 阿里仓库
  aliyun: http://maven.aliyun.com/nexus/content/groups/public/
  repox-maven: http://repox.gtan.com:8078/
  jcenter: http://jcenter.bintray.com/

  local-preloaded-ivy: file:///${sbt.preloaded-${sbt.global.base-${user.home}/.sbt}/preloaded/}, [organization]/[module]/[revision]/[type]s/[artifact](-[classifier]).[ext]
  local-preloaded: file:///${sbt.preloaded-${sbt.global.base-${user.home}/.sbt}/preloaded/}
  repox-ivy: http://repox.gtan.com:8078/, [organization]/[module]/(scala_[scalaVersion]/)(sbt_[sbtVersion]/)[revision]/[type]s/[artifact](-[classifier]).[ext], bootOnly
  typesafe-ivy-releases: http://repo.typesafe.com/typesafe/ivy-releases/, [organization]/[module]/[revision]/[type]s/[artifact](-[classifier]).[ext], bootOnly
  sbt-ivy-snapshots: https://repo.scala-sbt.org/scalasbt/ivy-snapshots/, [organization]/[module]/[revision]/[type]s/[artifact](-[classifier]).[ext], bootOnly
  
  maven-central: http://repo1.maven.org/maven2/

  sonatype-snapshots: http://oss.sonatype.org/content/repositories/snapshots
```

保存后使用命令对其重新打包，需要使用 -M 参数，否则 ./META-INF/MANIFEST.MF 会被修改，导致运行时会出现 “./sbt-launch.jar中没有主清单属性” 的错误。
```
rm ./sbt-launch.jar         # 删除旧的
jar -cfM ./sbt-launch.jar .       # 重新打包
ls | grep -v "sbt-launch.jar" | xargs rm -r    # 解压后的文件已无用，删除
```
