---
layout: post
title: "实践Spring Boot之EnableAutoConfiguration注解"
date: 2017-08-22 17:08:00 +0800
categories: Spring
tags: spring spring-boot enableAutoConfiguration
---

Spring Boot用于快速高效地创建可执行的Spring应用程序，进一步升华了习惯优于配置，其中核心之一就是**@EnableAutoConfiguration**，此注释自动载入应用程序所需的所有Bean，Spring Boot在类路径中查找META-INF/spring.factories，**spring-boot-autoconfigure**和**spring-boot-test-autoconfigure**这两个jar中都有这个文件，会import其中的@Configuration。



| auto onfiguration             | 备注                                       |
| ----------------------------- | ---------------------------------------- |
| TestDatabaseAutoConfiguration | 根据驱动类自动识别并创建测试数据库（嵌入式）及数据源               |
| H2ConsoleAutoConfiguration    | 是否自动开启H2 Console，spring.h2.console.enabled=true/false |
|                               |                                          |

