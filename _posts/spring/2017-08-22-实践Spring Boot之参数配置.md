---
layout: post
title: "实践Spring Boot之参数配置"
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

### 应用程序以外的配置文件

config目录或应用根目录下application.properties或者appliaction.yml文件

### 应用程序内的配置文件

config包或classpath根目录下application.properties或者appliaction.yml文件

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



### @ConfigurationProperties

为了能够更加方便的分门别类的定义和使用属性，@ConfigurationProperties能够将相关属性自动绑定到java bean【通常称为配置类】

，使用配置类有两种方式，一种是用@Component定义配置bean，另一种是使用@EnableConfigurationProperties。

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

java bean Config对应的配置如下：

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

会自动进行类型转换，还支持嵌套属性注入。

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

