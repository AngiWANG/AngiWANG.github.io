---
layout: post
title: "实践Spring Data"
date: 2017-07-26 17:14:00 +0800
categories: 数据库
tags: database nosql spring-data
---

[Spring Data](http://projects.spring.io/spring-data/)’s mission is to provide a familiar and consistent, Spring-based programming model for **data access** while still retaining the special traits of the underlying data store.
It makes it easy to use data access technologies, **relational** and **non-relational** databases, **map-reduce** frameworks, and **cloud-based** data services. This is an umbrella project which contains many subprojects that are specific to a given database. The projects are developed by working together with many of the companies and developers that are behind these exciting technologies.

**Spring Data Release Train - BOM**

```xml
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>org.springframework.data</groupId>
      <artifactId>spring-data-releasetrain</artifactId>
      <version>${release-train}</version>
      <scope>import</scope>
      <type>pom</type>
    </dependency>
  </dependencies>
</dependencyManagement>
```

*当前release-train版本为Ingalls-SR6*

