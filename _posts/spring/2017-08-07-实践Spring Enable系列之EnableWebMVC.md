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

Similar to support found in `<mvc:annotation-driven/>` namespace.

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

If `WebMvcConfigurer` does not expose some advanced setting that needs to be configured, consider **removing** the `@EnableWebMvc` annotation and extending directly from `WebMvcConfigurationSupport`or `DelegatingWebMvcConfiguration`, e.g.:

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

### 原理分析

@EnableWebMvc实际导入了DelegatingWebMvcConfiguration

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Documented
@Import(DelegatingWebMvcConfiguration.class)
public @interface EnableWebMvc {
}
```

`````java
@Configuration
public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
...
}
`````

以下仅以interceptor为例分析原理，其他类似。

WebMvcConfigurationSupport中有如下拦截器相关的方法：

```java
protected final Object[] getInterceptors() {
	if (this.interceptors == null) {
		InterceptorRegistry registry = new InterceptorRegistry();
    	// 添加拦截器
		addInterceptors(registry);
		registry.addInterceptor(new ConversionServiceExposingInterceptor(mvcConversionService()));
		registry.addInterceptor(new ResourceUrlProviderExposingInterceptor(mvcResourceUrlProvider()));
		this.interceptors = registry.getInterceptors();
	}
	return this.interceptors.toArray();
}
// 由子类覆盖添加拦截器
protected void addInterceptors(InterceptorRegistry registry) {
}
```
DelegatingWebMvcConfiguration覆盖了addInterceptors(InterceptorRegistry)方法

```java
@Override
protected void addInterceptors(InterceptorRegistry registry) {
	this.configurers.addInterceptors(registry);
}
```

且

```java
private final WebMvcConfigurerComposite configurers = new WebMvcConfigurerComposite();

// 自动注入所有WebMvcConfigurer
@Autowired(required = false)
public void setConfigurers(List<WebMvcConfigurer> configurers) {
	if (configurers == null || configurers.isEmpty()) {
		return;
	}
	this.configurers.addWebMvcConfigurers(configurers);
}
```

可见添加拦截器最终代理给了`WebMvcConfigurer`，以及`WebMvcConfigurerAdapter`。