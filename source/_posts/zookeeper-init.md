---
title: zookeeper安装
categories: 分布式
date: 2017-06-20 02:02:34
tags:
---

# 安装
1. 解压zookeeper安装包后，将conf目录下zoo_sample.cfg 拷贝一份为zoo.cfg
2. 在zookeeper的目录下新建data和logs目录，修改zoo.cfg，添加
```
dataDir=.../zookeeper/data
dataLogDir=.../zookeeper/logs

...
server.1=192.168.0.1:2888:3888
```

* initLimit: 初始化连接最大接受的心跳时间间隔数
* syncLimit: 请求和应答最大接受的心跳时间间隔数
* server.A=B:C:D: 该配置中，A为服务器序号，B为服务器ip，C为通信端口，D为主服务器挂了后的重新选择

3. 在data目录下创建myid文件，编辑配置当前机器的服务器序号，如 server.1
4. 配置环境变量
5. 通过zkServer.sh start启动zookeeper，输入jps可以查看到进程(QuorumPeerMain)
6. 配置开机启动 su - root -c 'zkServer.sh start'

# 使用
在启动zkServer.sh start后，新开命令行窗口，通过zkCli.sh开启客户端

1. 查看 – ls path [watch]
通过ls加绝对路径可以查看到当前zk服务器下的节点，path不可为空，且必须以/开头

2. 创建 – create [-s] [-e] path data [acl]
创建zookeeper节点，-s表示顺序节点，创建真实节点路径为在path后拼接10位整数，-e表示创建临时节点，创建的节点在当前会话结束后清除

3. 读取 – get path [watch]
获取指定节点的数据内容和属性信息

4. 更新 – set path data [version]
更新指定节点的数据内容，version为指定被更新的数据版本，一般不指定，如果数据版本已经更新，则指定旧版本时会报错

5. 删除 – delete path [version]
删除指定节点，version为指定被删除的数据版本，一般不指定，如果数据版本已经更新，则指定旧版本时会报错

6. 权限 – Scheme:id:permission
使用检验策略控制节点的权限，不具有继承关系，可以通过getAcl获取节点Acl信息

1、world: 设置所有用户的权限，world:anyone:crdwa
2、auth: 表示给认证通过的用户设置acl权限，并非指定用户，可以通过addauth命令进行认证用户的添加，addauth digest <username>:<password>
3、digest: 指定用户权限，username:password必须经过SHA-1和BASE64编码，BASE64(SHA1(username:password))，addauth不用进行编码
4、ip: 指定ip地址操作权限，ip:127.0.0.1:crdwa
5、super: 超级管理员，启动时在命令参数中配置，-Dzookeeper.DigestAuthenticationProvider.superDigest=admin:015uTByzA4zSglcmseJsxTo7n3c=，用户名密码需要编码








