---
layout: post
title: "实践Spring Cloud之Config"
date: 2017-11-03 11:08:00 +0800
categories: Java
tags: spring spring-boot java spring-cloud Config
---

![Spring Cloud Config Architecture](/images/spring-cloud-config-architecture.png)

## Config Server

依赖：

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

```java
@EnableConfigServer
```

### 仓库配置

三种占位符：

* {application}，应用名称，默认application
* {profile}，环境，默认default
* {label}，分支，默认master

URL路径和配置文件的映射关系：

/{application}/{profile}[/{label}]

[/{label}]/{application}-{profile}.yml

[/{label}]/{application}-{profile}.properties

```properties
# 固定仓库
spring.cloud.config.server.git.uri=http://192.168.70.244/ITS/config.git

# 不同的应用不同的仓库（利用占位符）
spring.cloud.config.server.git.uri=http://192.168.70.244/ITS/{application}.git

# 本地仓库
spring.cloud.config.server.git.uri=file://home/angi/sts_3.8.3_koolyun/config

# 本地文件系统
spring.profiles.active=native
# 默认src/main/resource目录下检索配置文件，可以指定
spring.cloud.config.server.native.searchLocation=/home/angi/config

# 固定子目录
spring.cloud.config.server.git.search-paths=ITS
# 不同的应用不同的目录（利用占位符），支持逗号分隔多个，默认包含根目录（/）
spring.cloud.config.server.git.search-paths={application}
```

多仓库

```properties
# 基于{application}/{profile}统配匹配，当无法匹配时取spring.cloud.config.server.git.uri
# 可以实现某一类应用使用某一个仓库
spring.cloud.config.server.git.repos.cts.pattern=cts-*/*
spring.cloud.config.server.git.repos.cts.uri=git@192.168.70.244:ITS/cts-config.git
spring.cloud.config.server.git.repos.cts.searchPaths={application}

spring.cloud.config.server.git.repos.ets.pattern=ets-*/*
spring.cloud.config.server.git.repos.ets.uri=git@192.168.70.244:ETS/ets-config.git
spring.cloud.config.server.git.repos.ets.searchPaths={application}
```

默认会从仓库checkout一份到本地`/tmp/config-repo-随机数`目录，可以通过参数定制此目录：

```properties
spring.cloud.config.server.git.basedir=~/config-basedir
```



### 仓库认证

用户需要Reporter或以上角色

#### http

```properties
spring.cloud.config.server.git.uri=http://192.168.70.244/ITS/config.git
spring.cloud.config.server.git.username=***
spring.cloud.config.server.git.password=***
```

#### ssh

基于{user.home}/.ssh/密钥访问

```properties
spring.cloud.config.server.git.uri=git@192.168.70.244:ITS/config.git
```

### 默认属性

### 安全保护

### 加密解密

### 高可用

## Config Client

依赖：

```xml
<dependency>
   <groupId>org.springframework.cloud</groupId>
   <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

bootstrap.properties/yml

```properties
# {application}
spring.application.name=sample
# config server
# 由于生产环境一般是独立的，默认配置为非生产，生产环境的参数通过命令行参数配置（shell启动脚本）
spring.cloud.config.uri=http://localhost:7003/
# {profile}，配置建议同config server
# spring.cloud.config.profile=default
# {label}，配置建议同config server
# spring.cloud.config.label=master
```

其他配置从config server获取

### 应用属性文件

{application}-{profile}.properties/yml

{application}.properties/yml（default profile）

通过`spring.cloud.config.profile=dev`设定profile

### 动态刷新

* 添加依赖`spring-boot-starter-actuator`，带有`/refresh`端点

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

* 添加@RefreshScope

  ```java
  @RefreshScope
  @RestController
  public class TestController {

      @Value("${from}")
      private String from;

      @RequestMapping("/from")
      public String from() {
          return from;
      }
  }
  ```

* 修改配置后，POST提交请求到客户端`/refresh`，即可刷新配置。例如：

  ```shell
  $ curl -d '' http://localhost:7002/refresh
  ["config.client.version","from"]
  ```

  ​