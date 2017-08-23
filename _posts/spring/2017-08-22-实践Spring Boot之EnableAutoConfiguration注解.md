---
layout: post
title: "实践Spring Boot之EnableAutoConfiguration注解"
date: 2017-08-22 17:08:00 +0800
categories: Spring
tags: spring spring-boot enableAutoConfiguration
---

Spring Boot用于创建可执行的Spring应用程序，进一步升华了习惯优于配置，其中核心之一就是@EnableAutoConfiguration，此注释自动载入应用程序所需的所有Bean，Spring Boot在类路径中的查找META-INF/spring.factories，在spring-boot-autoconfigure和spring-boot-autoconfigure这两个jar中存在，会import其中的@Configuration。



| auto Configuration            | 备注                          |      |
| ----------------------------- | --------------------------- | ---- |
| TestDatabaseAutoConfiguration | 测试数据库（嵌入式）自动配置，根据驱动类自动识别并创建 |      |
| H2ConsoleAutoConfiguration    | H2 Console自动配置              |      |
|                               |                             |      |