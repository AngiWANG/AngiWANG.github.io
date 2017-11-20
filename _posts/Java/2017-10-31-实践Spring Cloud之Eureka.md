---
layout: post
title: "实践Spring Cloud之Eureka"
date: 2017-10-31 11:08:00 +0800
categories: Java
tags: spring spring-boot java spring-cloud Eureka
---

![Spring Cloud Eureka Architecture](/images/spring-cloud-eureka-architecture.png)

**注：spring-cloud-starter-eureka已经依赖了spring-cloud-starter-ribbon**

## Eureka Server

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-eureka-server</artifactId>
</dependency>
```

@EnableEurekaServer

org.springframework.cloud.netflix.eureka.server.EurekaServerConfigBean，此类实现了com.netflix.eureka.EurekaServerConfig.所有的配置属性以eureka.server开头

```properties
# Eureka server响应缓存更新时间间隔，Eureka server面板绕过响应缓存的，可以实时看到最新的注册信息
eureka.server.responseCacheUpdateIntervalMs=30
# Eureka server启动是尝试从其他Eureka server同步注册信息，最多5次
eureka.server.numberRegistrySyncRetries=5
# 如果最终失败，则5分钟内阻止客户端从此Eureka server获取注册信息
eureka.server.getWaitTimeInMsWhenSyncEmpty=5
```



自我保护

```
EMERGENCY! EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT. RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEING EXPIRED JUST TO BE SAFE.
```

```properties
eureka.server.enable-self-preservation=false
```

```
THE SELF PRESERVATION MODE IS TURNED OFF.THIS MAY NOT PROTECT INSTANCE EXPIRY IN CASE OF NETWORK/OTHER PROBLEMS.
```

服务同步

给注册

提供注册信息

失效剔除

### region

默认：default

```properties
# 自定义region
eureka.client.region=shanghai
```

### zone

默认：defaultZone

```properties
# 自定义zone
eureka.client.available-zone=dev,test
```

Ribbon默认优先访问位于同一zone的服务端，其次才是其他zone的服务端

## Eureka Client

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-eureka</artifactId>
</dependency>
```

@EnableDiscoveryClient

org.springframework.cloud.netflix.eureka.EurekaClientConfigBean，此类实现了com.netflix.discovery.EurekaClientConfig，所有的配置属性以eureka.client开头

```properties
# 定义Eureka server地址
eureka.client.service-url.defaultZone=http://192.168.70.139:8888/eureka/
# 定义Eureka server地址刷新时间间隔（默认5分钟），可以用来实现动态修改Eureka server地址
eureka.client.eureka-service-url-poll-interval-seconds=300
# 是否注册到Eureka server
eureka.client.register-with-eureka=true
# 向Eureka server发送心跳间隔
eureka.instance.lease-renewal-interval-in-seconds=30
# 如果发送心跳失败，则以2的指数形式延长心跳时间间隔，直到指数上线，然后不断重试
eureka.client.heartbeatExecutorExponentialBackOffBound
# 如果Eureka server连续90秒内没有收到心跳，则会将其移出Eureka server
eureka.instance.lease-expiration-duration-in-seconds=90
# 首次向Eureka server发送InstanceInfo
eureka.client.initialInstanceInfoReplicationIntervalSeconds=40
# 以后每次发送InstanceInfo时间间隔
eureka.client.instanceInfoReplicationIntervalSeconds=30
# 是否从Eureka server获取注册信息
eureka.client.fetch-registry=true
# 获取注册信息时间间隔
eureka.client.registryFetchIntervalSeconds=30
# 如果获取注册信息失败，则以2的指数形式延长心跳时间间隔，直到指数上线，然后不断重试
eureka.client.cacheRefreshExecutorExponentialBackOffBound
# 默认是增量获取注册信息
eureka.client.shouldDisableDelta=true
```



```properties
eureka.instance.prefer-ip-address=true
```

org.springframework.cloud.netflix.eureka.EurekaInstanceConfigBean，此类间接实现了com.netflix.appinfo.EurekaInstanceConfig，所有的配置属性以eureka.instance开头。

服务注册

服务续约（renew）

获取服务

服务调用

缓存

服务下线

更新

负载均衡

## Eureka Instance

```properties
# 实例主机名（基于主机名注册的情况下，同一主机名下不可以运行多个eureka server）
eureka.instance.hostname=peer1
# 默认主机名注册，可以修改为ip注册优先
eureka.instance.prefer-ip-address=false
```

