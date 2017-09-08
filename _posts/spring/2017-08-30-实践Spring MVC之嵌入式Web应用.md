---
layout: post
title: "实践Spring MVC之嵌入式Web应用"
date: 2017-08-30 17:08:00 +0800
categories: Spring
tags: spring-mvc theme
---

DispatcherServletAutoConfiguration

EmbeddedServletContainerAutoConfiguration

WebMvcAutoConfiguration

### 替换web.xml

Servlet 3.0容器会查找**javax.servlet.ServletContainerInitializer**接口的实现来配置Servlet（以前是通过web.xml），Spring 3.1提供了这个接口的实现类**SpringServletContainerInitializer**，这个实现类进而查找**WebApplicationInitializer**接口的实现类，Spring 3.2提供了这个接口的抽象实现**AbstractAnnotationConfigDispatcherServletInitializer**

WebApplicationInitializer



SpringBootServletInitializer