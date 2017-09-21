---
layout: post
title: "实践Spring Boot之属性配置和使用"
date: 2017-09-14 17:08:00 +0800
categories: Spring
tags: spring spring-boot properties
---

Spring Boot内置很多属性，具体可以参见[common-application-properties](https://docs.spring.io/spring-boot/docs/1.5.3.RELEASE/reference/html/common-application-properties.html)，也可以自定义属性。

## 设置属性

Spring Boot支持多种方式设置属性值，优先级从高到低如下：

### 命令行参数

即java命令行参数，只是形式稍有区别，前面增加“--”，格式：--key[=value]），多个用空格分隔，例如：

```shell
$ java -jar app.jar --debug --server.port=9090
```

可以通过`SpringApplication.setAddCommandLineProperties(false)`禁用命令行配置。

### java:comp/env 里的JNDI属性

### Java系统属性

格式：-Dkey=value

```shell
$ java -Dname="isea533" -Dgender=1 -jar app.jar
```

### 操作系统环境变量

### 随机数

Spring Boot通过**RandomValuePropertySource**定义了随机数random供使用，例如：

```properties
my.secret=${random.value}
my.number=${random.int}
my.bignumber=${random.long}
my.number.less.than.ten=${random.int(10)}
my.number.in.range=${random.int[1024,65536]}
```

`random.int*`支持`value`参数和`max`参数，当提供`max`参数的时候，`value`就是最小值。

### 应用程序外基于Profile的配置文件

config目录或应用根目录下`application-{profile}.properties`或者`appliaction-{profile}.yml`文件或者`application.yml`(带`spring.profiles`)

### 应用程序内基于Profile的配置文件

config包或classpath根目录下`application-{profile}.properties`或者`appliaction-{profile}.yml`文件或者`application.yml`(带`spring.profiles`)

### 应用程序外的配置文件

config目录或应用根目录下`application.properties`或者`appliaction.yml`文件(不带`spring.profiles`)

### 应用程序内的配置文件

config包或classpath根目录下`application.properties`或者`appliaction.yml`文件(不带`spring.profiles`)

### 通过 @PropertySource 引入的配置文件

### 默认属性

通过`SpringApplication.setDefaultProperties`设置的默认属性，例如：

```java
SpringApplication application = new SpringApplication(Application.class);
Map<String, Object> defaultMap = new HashMap<String, Object>();
defaultMap.put("name", "Isea-Blog");
//还可以是Properties对象
application.setDefaultProperties(defaultMap);
application.run(args);
```

## 使用属性

### @Value

```java
@Value("${book.name}")
private String name;
```

@Value除了支持属性注入外，还支持如下参数注入：

1. 注入普通字符（@Value("hello, world")）
2. 注入操作系统属性（@Value("#systemProperties['os.name']")）
3. 注入表达式运算结果（@Value("#{ T(java.lang.Math).random()*100.0 }")）
4. 注入其他Bean的属性（@Value("#{demoService.username}")）
5. 注入文件内容（@Value("classpath:text.txt")）
6. 注入网址内容（@Value("http://www.baidu.com")）
7. 注入属性（@Value("${book.name}")）

### @ConfigurationProperties

为了能够更加方便的分门别类的定义和使用属性，@ConfigurationProperties能够将相关属性自动绑定到Java Bean【通常称为配置类】，会自动进行类型转换，还支持嵌套属性。使用配置类有两种方式，一种是用@Component定义配置bean，另一种是使用@EnableConfigurationProperties。

应用于Java Bean：

```java
@ConfigurationProperties(prefix="my")
public class Config {
    private String name;
    private Integer port;
    private List<String> servers = new ArrayList<String>();

    public String geName(){
        return this.name;
    }

    public Integer gePort(){
        return this.port;
    }
    public List<String> getServers() {
        return this.servers;
    }
}
```

Java Bean Config对应的配置如下：

```properties
my.name=Isea533
my.port=8080
my.servers[0]=dev.bar.com
my.servers[1]=foo.bar.com
```

也可以直接绑定到@Bean，例如：

```java
@ConfigurationProperties(prefix = "foo")
@Bean
public Config configComponent() {
    ...
}
```

### @EnableConfigurationProperties

### 属性占位符

```properties
app.name=MyApp
app.description=${app.name:默认名称} is a Spring Boot application
```

由于`${}`方式会被Maven处理。如果你pom继承的`spring-boot-starter-parent`，Spring Boot 已经将`maven-resources-plugins`默认的`${}`方式改为了`@ @`方式，例如`@name@`。

如果你是引入的Spring Boot，你可以修改使用[其他的分隔符](http://maven.apache.org/plugins/maven-resources-plugin/resources-mojo.html#delimiters)

自定义属性：

```properties
server.port=${port:8080}
```

### 属性名匹配规则

```java
@Component
@ConfigurationProperties(prefix="person")
public class ConnectionSettings {

    private String firstName;

}
```

`firstName`可以使用的属性名如下：

1. `person.firstName`，标准的驼峰式命名
2. `person.first-name`，虚线（`-`）分割方式，推荐在`.properties`和`.yml`配置文件中使用
3. `PERSON_FIRST_NAME`，大写下划线形式，建议在系统环境变量中使用

## 验证

可以使用JSR-303注解进行属性验证，例如：

```java
@Component
@ConfigurationProperties(prefix="connection")
public class ConnectionSettings {

    @NotNull
    private InetAddress remoteAddress;

    // ... getters and setters

}
```

