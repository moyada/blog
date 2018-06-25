---
title: maven
categories: 项目管理
date: 2017-03-31 01:42:17
tags:
---

# pom配置文件

## 命令
* mvn clean: 清除target目录
* mvn compile: 编译主程序
* mvn test-compile: 编译测试程序
* mvn test: 执行测试
* mvn package: 打包项目输出到target目录下
* mvn install: 安装项目到本地仓库
  安装本地jar包: mvn install:install-file -Dfile=a.jar -DgroupId=com.a -DartifactId=a -Dversion=0.0.1 -Dpackaging=jar
* mvn deploy: 将打包好的包上传到远程仓库,`[-N]`跳过子模块
* mvn site: 生成站点

跳过测试用例 -Dmaven.test.skip=true

## 依赖

### 坐标
1. <groupid> 项目坐标: 一般为公司域名倒写+项目名
2. <artifactId> 模块坐标: 项目子模块名
3. <version> 版本: 带SNAPSHOT表示为一个不稳定的版本，REALEASE表示为一个正式的版本

### 依赖的范围
依赖的范围可以通过<scope>标签来指定
1. compile: 主程序范围的依赖，对主程序有效，对测试程序有效，参与打包
2. test: 测试范围的依赖，只对测试程序有效，不参与打包，比如junit
3. provided: 不参与打包，只在开发阶段有效，比如servlet-api.jar

### 依赖的排除
对于不需要的传递性依赖，可以通过<exclusions>标签来排除依赖引用
```
<dependency>
	...
	<exclusions>
		<exclusion>
			<groupid>com.xxx</groupid>
			<artifactid>xxxx</artifactid>
		</exclusion>
	</exclusions>
</dependency>
```

### 版本号管理
对于多个同版本号的依赖可以通过<properties>配置来统一管理，在<version>以${标签名}引用
```
<properties>
	<com.spring.version>4.3.0-REALEASE</com.spring.version>
</properties>
...
<version>${com.spring.version}</version>
```

另外，还可以通过<properties>配置项目默认配置
```
<!-- 配置项目字符集配置 -->
<properties>
	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
</properties>
```

## 继承
继承功能可以统一管理各个模块工程中对依赖的版本，配置继承后要先安装父工程
```
<!-- 子工程中声明父工程 -->
<parent>
   <groupid>com.xxx</groupid>
   <artifactId>xxxx</artifactId>
   <version>0.0.1-SNAPSHOT</version>
   <!-- 以当前工程的pom.xml为基准的父工程pom.xml路径 -->
   <relativePath>../xxxx/pom.xml</relativePath>
</parent>
```

```
<!-- 父工程依赖管理 -->
<dependencyManagement>
  <dependencies>
    <dependency>
      ...
    </dependency>
  </dependencies>
</dependencyManagement>
```

## 聚合
聚合用于统一安装子工程模块，在父工程的pom.xml文件中配置
```
<modules>
  <!-- 指定子工程的相对路径 -->
  <module>../xxx</module>
</modules>
```
## 构建
maven通过命令mvn package 可以将项目打包，对于多模块的项目，
可以使用mvn package -pl 子项目名 -am 或 mvn package --projects 子项目名 --also-make 
将父项目和子项目一起进行打包同时构建所需依赖

maven 还可以使用org.codehaus.cargo这个插件直接将项目打包部署到远程服务器上
```
<!-- 配置构建过程 -->
<build>
  <finalName>工程名</finalName>
  <!-- 插件 -->
  <plugins>
    <plugin>
      <!-- cargo是启动/停止/配置servlet容器插件 -->
      <groupid>org.codehaus.cargo</groupid>
      <artifactId>cargo-maven2-plugin</artifactId>
      <version>1.2.3</version>
      <configuration>  
        <container>  
            <containerId>tomcat7x</containerId>  
            <home>/usr/local/devtools/apache-tomcat-7.0.55</home>  
        </container>  
        <configuration>  
            <type>existing</type>  
            <home>/usr/local/devtools/apache-tomcat-7.0.55</home>  
            <properties>  
                <!-- 更改监听端口 -->  
                <cargo.servlet.port>8088</cargo.servlet.port>  
            </properties>  
        </configuration> 
      </configuration>
      <!-- 配置声明周期阶段 -->
      <executions>  
        <execution>  
            <id>cargo-run</id>  
            <!-- 声明周期的阶段 -->
            <phase>install</phase>
            <!-- 插件的目标 -->
            <goals>  
                <goal>run</goal>  
            </goals>
        </execution>  
        <execution>  
            <id>clean-deployer</id>  
            <!-- 声明周期的阶段 -->
            <phase>deploy</phase>
            <!-- 插件的目标 -->
            <goals>  
                <goal>deployer-undeploy</goal>  
            </goals>  
        </execution>    
      </executions>  
    </plugin>
  </plugins>
</build>
```

| Goals     | Description       | 
| --------- |:-----------------:|
| cargo:start   | Start a container.|
| cargo:run   | Start a container and wait for the user to press CTRL + C to stop.|
| cargo:stop   | Stop a container.|
| cargo:restart   | Stop and start again a container. If the container was not running before calling cargo:restart, it will simply be started.|
| cargo:configure   | Create the configuration for a local container, without starting it. Note that the cargo:start and cargo:run goals will also install the container automatically (but will not call cargo:install).|
| cargo:package   | Package the local container.|
| cargo:daemon-start  | Start a container via the daemon.|
| cargo:daemon-stop  | Stop a container via the daemon.|
| cargo:deployer-deploy (aliased to cargo:deploy)   | Deploy a deployable to a running container.|
| cargo:deployer-undeploy(aliased to cargo:undeploy)  | Undeploy a deployable from a running container.|
| cargo:deployer-start  | Start a deployable already installed in a running container.|
| cargo:deployer-stop  | Stop a deployed deployable without undeploying it.|
| cargo:deployer-redeploy(aliased to cargo:redeploy)  | Undeploy and deploy again a deployable. If the deployable was not deployed before calling cargo:deployer-redeploy (or its alias cargo:redeploy) it will simply be deployed.|
| cargo:uberwar  | Stop a deployed deployable without undeploying it.|
| cargo:deployer-stop | Merge several WAR files into one.|
| cargo:install | Installs a container distribution on the file system. Note that the cargo:start goal will also install the container automatically (but will not call cargo:install).|
| cargo:help | Get help (list of available goals, available options, etc.).|

# 仓库
## 本地仓库

## 远程仓库
1. 私服: 搭建在局域网环境中，为局域网范围内的所有Maven工程服务
2. 中央仓库: 架设在Internet上，为全世界所有Maven工程服务
3. 中央仓库镜像: 架设在各大洲，为中央仓库分担流量。减轻中央仓库的压力，同时更快的响应用户请求

# setting.xml配置文件

## 配置maven的本地仓库目录
localRepository标签

## 设置默认jdk版本

```
<profiles>
    <profile>
      <id>jdk-1.8</id>
      <activation>
        <activeByDefault>true</activeByDefault>
        <jdk>1.8</jdk>
      </activation>
      <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
      </properties>
    </profile>
</profiles>
```
