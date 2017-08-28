---
layout: post
title: "实践Spring Enable系列之EnableWebMVC"
date: 2017-08-04 13:41:00 +0800
categories: Spring
tags: spring web mvc spring-mvc
---

Adding this annotation to an `@Configuration` class imports the Spring MVC configuration from `WebMvcConfigurationSupport`, e.g.:

```java
 @Configuration
 @EnableWebMvc
 @ComponentScan(basePackageClasses = { MyConfiguration.class })
 public class MyWebConfiguration {

 }
 
```

Similar to support found in <mvc:annotation-driven> namespace.

To customize the imported configuration, implement the interface `WebMvcConfigurer` or more likely extend the empty method base class `WebMvcConfigurerAdapter` and override individual methods, e.g.:

```java
 @Configuration
 @EnableWebMvc
 @ComponentScan(basePackageClasses = { MyConfiguration.class })
 public class MyConfiguration extends WebMvcConfigurerAdapter {

 	   @Override
 	   public void addFormatters(FormatterRegistry formatterRegistry) {
         formatterRegistry.addConverter(new MyConverter());
 	   }

 	   @Override
 	   public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
         converters.add(new MyHttpMessageConverter());
 	   }

     // More overridden methods ...
 }
 
```

If `WebMvcConfigurer` does not expose some advanced setting that needs to be configured, consider removing the `@EnableWebMvc` annotation and extending directly from `WebMvcConfigurationSupport`or `DelegatingWebMvcConfiguration`, e.g.:

```java
 @Configuration
 @ComponentScan(basePackageClasses = { MyConfiguration.class })
 public class MyConfiguration extends WebMvcConfigurationSupport {

 	   @Override
	   public void addFormatters(FormatterRegistry formatterRegistry) {
         formatterRegistry.addConverter(new MyConverter());
	   }

	   @Bean
	   public RequestMappingHandlerAdapter requestMappingHandlerAdapter() {
         // Create or delegate to "super" to create and
         // customize properties of RequestMappingHandlerAdapter
	   }
 }
 
```

- Since:

  3.1

- Author:

  Dave Syer

  Rossen Stoyanchev