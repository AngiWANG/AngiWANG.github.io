---
layout: post
title: "实践Spring Test"
date: 2018-04-25 11:22:00 +0800
categories: 测试
tags: test spring-test spring
---

## spring-test

```xml
	    <dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
          	 <version>4.1.2.RELEASE</version>
			<scope>test</scope>
		</dependency>
```



```java
@RunWith(SpringJUnit4ClassRunner.class)
//bean容器配置
@ContextConfiguration(classes = { App1Config.class })
//@ContextConfiguration(locations = {"app-context.xml"})
public class SampleTest {

	@Autowired
	@Qualifier("videoService1")
	private VideoService videoService11;

	@Autowired
	@Qualifier("videoService2")
	private VideoService videoService22;

	@Test
	public void testAbc1() {
		videoService11.sayHello("Angi");
		videoService22.sayHello("Wang");
		Assert.assertNotEquals(videoService11, videoService22);
	}

}
```



## @ContextConfiguration

加载上下文供集成测试之用

## @WebAppConfiguration

声明`Web上下文`，结合`@ContextConfiguration`实例化应用上下文，会自动注入`WebApplicationContext`，利用`WebApplicationContext`可以创建`MockMvc`实例。

## @MockMvc

### MockMvcBuilders#standaloneSetup

```java
@RunWith(SpringJUnit4ClassRunner.class) 
@ContextConfiguration(locations = {"classpath:applicationContext.xml"})
public class UserControllerStandaloneSetupTest {  
  	@Autowired
  	private UserController userController;
  
    private MockMvc mockMvc;  
    @Before  
    public void setUp() {   
        mockMvc = MockMvcBuilders.standaloneSetup(userController).build();  
    }  
} 
```

### MockMvcBuilders#webAppContextSetup

```java
    //XML风格  
    @RunWith(SpringJUnit4ClassRunner.class)  
    @WebAppConfiguration(value = "src/main/webapp")  
    @ContextConfiguration(locations = {"classpath:applicationContext.xml"})
    public class UserControllerWebAppContextSetupTest {  
      
        @Autowired  
        private WebApplicationContext wac;  
        private MockMvc mockMvc;  
      
        @Before  
        public void setUp() {  
            mockMvc = MockMvcBuilders.webAppContextSetup(wac).build();  
        }  
    }  
```

## @Rollback

如果希望执行完测试后，数据库等支持事务的资源还原，可以在单元测试上增加`@Transactional`，由于`@Rollback`默认值为true，因而会回滚，如果不希望回滚可以不添加`@Transactional`或者设置`@Rollback(false)`

## @Commit

相当于`@Rollback(false)`

## @TransactionConfiguration

不再推荐使用，推荐使用@Rollback和@Commit