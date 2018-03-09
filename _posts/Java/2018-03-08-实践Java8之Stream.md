---
layout: post
title: "实践Java8之Stream"
date: 2018-02-25 11:08:00 +0800
categories: Java
tags: java pipeline stream
---
method expression

String::toLowerCase

```

```



flatmap(Function)

Stream

generate()

iterate()

peek()

```java
// 传统
for(Object o: list) {
	System.out.println(o);
}
// 使用Lambda表达式
list.forEach(o -> {System.out.println(o);});
```



```java
// 挑出蓝色的，然后将这些蓝色的物体喷成红色
public void findBlueAndToRed(String... numbers) {
    List<Shape> shapes = Arrays.asList(numbers);
    shapes.stream()
      .filter(s -> s.getColor() == BLUE)
      .forEach(s -> s.setColor(RED));
}
```



```java
// 并行处理
shapes.parallelStream(); // 或shapes.stream().parallel()
```



```java
// 给出一个String类型的数组，找出其中所有不重复的素数
public void distinctPrimary(String... numbers) {
	List<String> l = Arrays.asList(numbers);
	List<Integer> r = l.stream()
      .map(e -> new Integer(e))
      .filter(e -> Primes.isPrime(e))
      .distinct()
      .collect(Collectors.toList());
	System.out.println("distinctPrimary result is: " + r);
}
```



```java
// 给出一个String类型的数组，找出其中各个素数，并统计其出现次数
public void primaryOccurrence(String... numbers) {
	List<String> l = Arrays.asList(numbers);
	Map<Integer, Integer> r = l.stream()
      .map(e -> new Integer(e))
      .filter(e -> Primes.isPrime(e))
      .collect( Collectors.groupingBy(p->p, Collectors.summingInt(p->1)) );
	System.out.println("primaryOccurrence result is: " + r);
}
```



```java
//给出一个String类型的数组，求其中所有不重复素数的和
public void distinctPrimarySum(String... numbers) {
	List<String> l = Arrays.asList(numbers);
    int sum = l.stream()
      .map(e -> new Integer(e))
      .filter(e -> Primes.isPrime(e))
      .distinct()
      .reduce(0, (x,y) -> x+y); // equivalent to .sum()
    System.out.println("distinctPrimarySum result is: " + sum);
}
```



```java
// 统计年龄在25-35岁的男女人数、比例
public void boysAndGirls(List<Person> persons) {
    Map<Integer, Integer> result = persons.parallelStream()
      .filter(p -> p.getAge()>=25 && p.getAge()<=35).
      collect(Collectors.groupingBy(p->p.getSex(), Collectors.summingInt(p->1)));
    System.out.print("boysAndGirls result is " + result);
    System.out.println(", ratio (male : female) is " +(float)result.get(Person.MALE)/result.get(Person.FEMALE));
}
```

`stream`：一个流通常以一个集合类实例为其数据源，然后在其上定义各种操作。

`pipeline`：流的API设计使用了管道（pipelines）模式，对流的一次操作会返回另一个流。

`lazy`：不会实时执行（管道贯通式）。

`eager`：实时执行，并且会触发lazy的执行。

forEach(Consumer)，map(Function)，filter(Predicate)，distinct()，toMap(Function, Function)，groupBy(Function, Collector)，

reduce()，sum()，max()，min()
