---
layout: post
title: "实践Spring Test"
date: 2018-04-25 11:22:00 +0800
categories: 测试
tags: test spring-test spring
---



## @ContextConfiguration

加载上下文供集成测试之用

## @WebAppConfiguration

声明`Web上下文`，结合`@ContextConfiguration`实例化上下文

## @MockMvc

### MockMvcBuilders#standaloneSetup

```java
public class UserControllerStandaloneSetupTest {  
    private MockMvc mockMvc;  
    @Before  
    public void setUp() {  
        UserController userController = new UserController();  
        mockMvc = MockMvcBuilders.standaloneSetup(userController).build();  
    }  
} 
```

### MockMvcBuilders#webAppContextSetup

```java
    //XML风格  
    @RunWith(SpringJUnit4ClassRunner.class)  
    @WebAppConfiguration(value = "src/main/webapp")  
    @ContextHierarchy({  
            @ContextConfiguration(name = "parent", locations = "classpath:spring-config.xml"),  
            @ContextConfiguration(name = "child", locations = "classpath:spring-mvc.xml")  
    })  
      
    //注解风格  
    //@RunWith(SpringJUnit4ClassRunner.class)  
    //@WebAppConfiguration(value = "src/main/webapp")  
    //@ContextHierarchy({  
    //        @ContextConfiguration(name = "parent", classes = AppConfig.class),  
    //        @ContextConfiguration(name = "child", classes = MvcConfig.class)  
    //})  
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

## @Commit

## @TransactionConfiguration