---
layout: post
title: "实践Spring Cloud之Ribbon"
date: 2017-10-31 11:08:00 +0800
categories: Java
tags: spring spring-boot java spring-cloud Eureka
---

客户端调用服务端时，如何定位服务端呢？

## DiscoveryClient



### 使用LoadBalancerClient

LoadBalanceClient是Spring Cloud Commons自带的。根据服务名查找选择服务，然后拼装服务地址。

```java
@RestController
public class HelloClientController {
	
	private final Logger logger = LoggerFactory.getLogger(getClass());
	
	@Autowired
    LoadBalancerClient loadBalancerClient;

	@Autowired
	private RestTemplate restTemplate;

	@RequestMapping(value = "/helloClient", method = RequestMethod.GET)
	public String hello() {
		logger.info("helloClient received.");
		ServiceInstance serviceInstance = loadBalancerClient.choose("hello-service");
         String url = "http://" + serviceInstance.getHost() + ":" + serviceInstance.getPort() + "/hello";
         return restTemplate.getForObject(url, String.class);
	}
}
```

### 使用Ribbon

自动基于服务名组装服务地址

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-ribbon</artifactId>
</dependency>
```

```java
@EnableDiscoveryClient
@SpringBootApplication
public class HelloClientApplication {

	@Bean
	@LoadBalanced
	RestTemplate restTemplate() {
		return new RestTemplate();
	}

	public static void main(String[] args) {
		SpringApplication.run(HelloClientApplication.class, args);
	}
}
```



```java
@RestController
public class HelloClientController {
	
	private final Logger logger = LoggerFactory.getLogger(getClass());
	
	@Autowired
	private RestTemplate restTemplate;

	@RequestMapping(value = "/helloClient", method = RequestMethod.GET)
	public String hello() {
		logger.info("helloClient received.");
		return restTemplate.getForEntity("http://HELLO-SERVICE/hello", String.class).getBody();
	}
}
```

**注：spring-cloud-starter-eureka和spring-cloud-starter-consul-discovery已经依赖了spring-cloud-starter-ribbon**



```properties
# 缓存刷新时间间隔
ribbon.ServerListRefreshInterval=30
```



## 方法

### get

### post

### put

### delete

* ILoadBalancer
  * BaseLoadBalancer
    * IPing
      * IPingStrategy
    * IRule
      * RoundRobinRule
  * DynamicServerListLoadBalancer：动态更新，过滤
    * ServerListFilter
  * ZoneAwareLoadBalancer：默认



