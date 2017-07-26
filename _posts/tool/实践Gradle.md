---
layout: post
title: "实践Gradle"
date: 2017-07-19 09:13:13 +0800
categories: 工具
tags: gradle tool maven build
---

## 介绍

[Gradle](https://gradle.org/)是一个构建工具，类似ant/maven

## 安装

需要jdk/jre7.0或以上版本

*可以参考[官方安装说明](https://gradle.org/install/)*

### 二进制发行包

到官方下载二进制发行包，解压缩，设定环境变量即可。

### 通过包管理器

Unix类可以使用SDKMAN，Mac可以使用Homebrew，Windows可以使用Scoop

```shell
$ sdk install gradle 4.0
```

```shell
$ gradle -v

------------------------------------------------------------
Gradle 4.0
------------------------------------------------------------

Build time:   2017-06-14 15:11:08 UTC
Revision:     316546a5fcb4e2dfe1d6aa0b73a4e09e8cecb5a5

Groovy:       2.4.11
Ant:          Apache Ant(TM) version 1.9.6 compiled on June 29 2015
JVM:          1.8.0_101 (Oracle Corporation 25.101-b13)
OS:           Linux 4.9.0-deepin6-amd64 amd64

```

## build.gradle

```
jar {
    baseName = 'sample-gradle'
    version =  '0.1.0'
}
```



### 任务（Tasks）

```shell
$ gradle task
Starting a Gradle Daemon (subsequent builds will be faster)

> Task :tasks

------------------------------------------------------------
All tasks runnable from root project
------------------------------------------------------------

Build tasks
-----------
assemble - Assembles the outputs of this project.
build - Assembles and tests this project.
buildDependents - Assembles and tests this project and all projects that depend on it.
buildNeeded - Assembles and tests this project and all projects it depends on.
classes - Assembles main classes.
clean - Deletes the build directory.
jar - Assembles a jar archive containing the main classes.
testClasses - Assembles test classes.

Build Setup tasks
-----------------
init - Initializes a new Gradle build.

Documentation tasks
-------------------
javadoc - Generates Javadoc API documentation for the main source code.

Help tasks
----------
buildEnvironment - Displays all buildscript dependencies declared in root project 'sample-neo4j'.
components - Displays the components produced by root project 'sample-neo4j'. [incubating]
dependencies - Displays all dependencies declared in root project 'sample-neo4j'.
dependencyInsight - Displays the insight into a specific dependency in root project 'sample-neo4j'.
dependentComponents - Displays the dependent components of components in root project 'sample-neo4j'. [incubating]
help - Displays a help message.
model - Displays the configuration model of root project 'sample-neo4j'. [incubating]
projects - Displays the sub-projects of root project 'sample-neo4j'.
properties - Displays the properties of root project 'sample-neo4j'.
tasks - Displays the tasks runnable from root project 'sample-neo4j'.

IDE tasks
---------
cleanEclipse - Cleans all Eclipse files.
eclipse - Generates all Eclipse files.

Verification tasks
------------------
check - Runs all checks.
test - Runs the unit tests.

Rules
-----
Pattern: clean<TaskName>: Cleans the output files of a task.
Pattern: build<ConfigurationName>: Assembles the artifacts of a configuration.
Pattern: upload<ConfigurationName>: Assembles and uploads the artifacts belonging to a configuration.

To see all tasks and more detail, run gradle tasks --all

To see more detail about a task, run gradle help --task <task>


BUILD SUCCESSFUL in 6s
1 actionable task: 1 executed

```

### 插件（Plugins）

```gradle
apply plugin: 'java'
```

```shell
$ gradle build
```

### 依赖

```
dependencies {
    compile "joda-time:joda-time:2.2"
}
```



### 仓库

```
repositories {
    mavenLocal()
    mavenCentral()
    maven { url "http://m2.neo4j.org/content/repositories/releases" }
}
```



## Gradle Wrapper

Gradle Wrapper的作用是将gradle的版本纳入项目管理中，实现gradle的版本由项目控制，与运行环境无关。通过首次运行时自动下载相应版本的gradle到当前环境实现。不会使用当前环境已安装的gradle。

```shell
# build.gradle中新增如下配置
task wrapper(type: Wrapper) {
    gradleVersion = '4.0'
}
```

```shell
# 初始化gradle wrapper
$ gradle wrapper
```

```shell
# 生成如下gradle wrapper相关文件
└── sample-gradle
    └── gradlew
    └── gradlew.bat
    └── gradle
        └── wrapper
            └── gradle-wrapper.jar
            └── gradle-wrapper.properties
```

```shell
# 使用gradle wrapper
$ ./gradlew build
```

```shell
# 升级gradle wrapper使用的gradle版本
$ ./gradlew wrapper --gradle-version=4.0.1 --distribution-type=bin
```

