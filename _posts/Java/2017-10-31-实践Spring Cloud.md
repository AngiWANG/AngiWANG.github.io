---
layout: post
title: "实践Spring Cloud"
date: 2017-10-31 11:08:00 +0800
categories: Java
tags: spring spring-boot java spring-cloud
---

[Spring Cloud](http://projects.spring.io/spring-cloud/)构建在Spring Boot之上，提供了服务治理相关的依赖管理。



```xml
<properties>
	<spring-cloud.version>Dalston.SR4</spring-cloud.version>
</properties>
```



```xml
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-dependencies</artifactId>
      <version>${spring-cloud.version}</version>
      <type>pom</type>
      <scope>import</scope>
    </dependency>
  </dependencies>
</dependencyManagement>
```

