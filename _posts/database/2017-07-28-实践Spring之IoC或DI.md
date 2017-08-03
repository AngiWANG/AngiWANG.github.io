---
layout: post
title: "实践Spring之IoC或DI"
date: 2017-07-28 09:25:00 +0800
categories: Spring
tags: spring IoC DI
---

面向对象编程中很重要的一步就是进行对象设计和定义（比如类或接口等），而下一步就要考虑如何使用这些对象，一般是先通过实例化对象来生产实例，然后调用实例的方法来执行逻辑。其中实例化有多种方法，传统的new，Class.forName()和反射等，当项目较大，对象和实例将会非常多，依赖关系也会较为复杂，一般传统的方式会较难控制。Spring提供的IoC或DI，使用配置的方式来完成**实例容器**、**实例化**、**实例间的依赖关系（依赖注入）**和**生命周期管理**等。

## 实例容器

BeanFactory

ApplicationContext

常用的有：

ClassPathXmlApplicationContext

AnnotationConfigApplicationContext

## 基于XML

## 基于注解（notation）

### @Configuration

定义@bean的载体，好比一个定义<bean>的xml文件，可以被实例容器加载

#### @Import

引入其他的@Configuration

### @Bean

bean定义

### @Scope

默认是单例（singleton），也可以指定为原型（prototype），

```java
@Scope("singleton")
@Scope("prototype")
```

### 依赖注入

1. 引用方法注入
2. 方法参数注入

### 生命周期

```java
@Bean(initMethod = "init", destroyMethod = "destory")
```

### 样例

SaveVideoInfoDao.java

```java
public class SaveVideoInfoDao(){
	public void init(){
      
  	}
  	public void destory(){
      
  	}
}
```

VideoService.java

```java
public class VideoService(){
  	private SaveVideoInfoDao saveVideoInfoDao;
	public VideoService(SaveVideoInfoDao saveVideoInfoDao){
    	this.saveVideoInfoDao = saveVideoInfoDao;
	}
}
```

App1Config.java

```java
@Configuration
@Import(App2Config.class)
public class App1Config{
	@Bean(initMethod = "init", destroyMethod = "destory")
    public SaveVideoInfoDao saveVideoInfoDao() {
        return new SaveVideoInfoDao();
    }
    //引用方法注入
    @Bean
    public VideoService videoService1() {
        VideoService videoService = new VideoService(saveVideoInfoDao());
        return videoService;
    }
}
```

App2Config.java

```java
@Configuration
public class App2Config{
	@Bean(initMethod = "init", destroyMethod = "destory")
    public SaveVideoInfoDao saveVideoInfoDao() {
        return new SaveVideoInfoDao();
    }
    //方法参数注入
    @Bean
    public VideoService videoService2(SaveVideoInfoDao saveVideoInfoDao) {
        VideoService videoService = new VideoService(saveVideoInfoDao);
        return videoService;
    }
}
```
SampleMain.java
```java
public class SampleMain{
    public static void main(String[] args) {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(App1Config.class);
        VideoService videoService1 = ctx.getBean("videoService1");
        VideoService videoService2 = ctx.getBean("videoService2");
    }
}
```



### @ComponentScan

Configures component scanning directives for use with @`Configuration` classes. Provides support parallel with Spring XML's `<context:component-scan>` element.

One of `basePackageClasses()`, `basePackages()` or its alias `value()` may be specified to define specific packages to scan. If specific packages are not defined scanning will occur from the package of the class with this annotation.

Note that the `<context:component-scan>` element has an `annotation-config` attribute, however this annotation does not. This is because in almost all cases when using `@ComponentScan`, default annotation config processing (e.g. processing `@Autowired` and friends) is assumed. Furthermore, when using `AnnotationConfigApplicationContext`, annotation config processors are always registered, meaning that any attempt to disable them at the `@ComponentScan` level would be ignored.

## 自动配置