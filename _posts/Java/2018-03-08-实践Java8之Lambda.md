---
layout: post
title: "实践Lambda表达式"
date: 2018-02-25 11:08:00 +0800
categories: Java
tags: java 
---
## Lambda介绍

Lambda表达式（λ）本质上可以看做匿名函数，例如函数：

```java
public int add(int x, int y) {
	return x + y;
}
```

转换成Lambda表达式：

```java
(int x, int y) -> x + y;
// 参数类型有时也可以省略，如果Java编译器能够根据上下文推断出来
(x, y) -> x + y; 
// 显式指明返回值
(x, y) -> { return x + y; } 
```

可见Lambda表达式由三部分组成：`参数列表`，`箭头（->）`，以及一个`表达式或语句块`。

```java
// 没有参数，也没有返回值（比如：Ruuable的run方法）
() -> { System.out.println("Hello Lambda!"); }
// 只有一个参数且参数类型可以省略时，参数括号可以不要
c -> { return c.size(); }
```

## Lambda类型

Java8加入了对Lambda表达式（λ）的支持，Lambda表达式的`目标类型（target type）`是`函数接口（function interface）`。

```
一个接口，如果只有一个显式声明的抽象方法，那么它就是一个函数接口
```

一般用注解`@FunctionalInterface`标注（非必须），例如：

```java
package java.lang;
@FunctionalInterface
public interface Runnable {
    public abstract void run();
}
```

Lambda表达式也可以被看做是一个Object，前提是必须转型为一个函数接口，例如：

```java
// ERROR! Object is not a functional interface!
Object obj = () -> {System.out.println("Hello Lambda!");}; 
// 先转型为具体的函数接口
Runnable r1 = () -> {System.out.println("Hello Lambda!");};
Object obj = r1;
// 或者直接强转
Object o = (Runnable) () -> { System.out.println("hi"); };
```

JDK预定义了一些函数接口，比如：`Function`、`Consumer`和`Predicate`等

## Lambda使用

### 内部类

Lambda表达式主要用于替换以前广泛使用的内部匿名类，比如回调等。

```java
// 传统
Thread oldSchool = new Thread( new Runnable () {
	@Override
    public void run() {
        System.out.println("This is from an anonymous class.");
    }
});
// 使用Lambda表达式
Thread gaoDuanDaQiShangDangCi = new Thread( () -> {
  	System.out.println("This is from an anonymous method (lambda exp).");
} );
```

### 集合类批处理操作

```java
// 传统
for(Object o: list) {
	System.out.println(o);
}
// 使用Lambda表达式
list.forEach(o -> {System.out.println(o);});
```



## 参考

[Java8 Lambda表达式教程](http://blog.csdn.net/ioriogami/article/details/12782141)