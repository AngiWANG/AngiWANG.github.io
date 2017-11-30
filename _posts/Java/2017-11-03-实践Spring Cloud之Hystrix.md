---
layout: post
title: "实践Spring Cloud之Hystrix"
date: 2017-11-03 11:08:00 +0800
categories: Java
tags: spring spring-boot java spring-cloud Hystrix
---

Hystrix实现服务容错保护，提供了超时机制、断路器、线程隔离等一系列服务保护功能。



```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-hystrix</artifactId>
</dependency>
```



```java
@EnableCircuitBreaker
```



```java
// 断路器包装外部请求，指定后备方法【请求失败、超时、被拒绝以及断路器打开时都会执行回退逻辑】
@HystrixCommand(fallbackMethod = "helloFallback")
public String hello() {
	return restTemplate.getForEntity("http://HELLO-SERVICE/hello", String.class).getBody();
}

// 后备方法
public String helloFallback() {
	return "error";
}
```



feign默认使用断路器（hystrix）包裹所有方法。

## Hystrix Dashboard

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-hystrix-dashboard</artifactId>
</dependency>
```

提供/hystrix.stream端点