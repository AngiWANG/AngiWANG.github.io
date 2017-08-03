---
layout: post
title: "实践Maven"
date: 2017-08-03 16:03:13 +0800
categories: 工具
tags: maven tool build
---

## 介绍

[Maven](https://maven.apache.org/)是一个构建工具，类似ant/gradle

## 安装

*详情可以参考[官方安装说明](https://maven.apache.org/install.html)*

### 二进制发行包

到官方下载二进制发行包，解压缩，设定环境变量即可。

### 通过包管理器

*nix系统可以使用SDKMAN，Mac可以使用Homebrew，Windows可以使用Scoop

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

## pom.xml

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

gradle本地仓库默认路径：${home}/.gradle/caches/modules-2/files-2.1/

## Gradle Wrapper

Gradle Wrapper的作用是将gradle的版本纳入项目管理中，实现gradle的版本由项目控制，与运行环境无关。首次运行时自动下载相应版本的gradle到当前环境，升级也会非常容易。不会使用当前环境已安装的gradle

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

## properties

### plugin属性

此处以maven-compiler-plugin插件为例，通过帮助了解plugin及其goal

```shell
$ mvn compiler:help
[INFO] Scanning for projects...
[INFO]                                                                         
[INFO] ------------------------------------------------------------------------
[INFO] Building sample-spring-javaconfig 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-compiler-plugin:2.3.2:help (default-cli) @ sample-spring-javaconfig ---
[INFO] org.apache.maven.plugins:maven-compiler-plugin:2.3.2

Maven Compiler Plugin
  The Compiler Plugin is used to compile the sources of your project.

This plugin has 3 goals:

compiler:compile
  Compiles application sources

compiler:help
  Display help information on maven-compiler-plugin.
  Call
    mvn compiler:help -Ddetail=true -Dgoal=<goal-name>
  to display parameter details.

compiler:testCompile
  Compiles application test sources.


[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 0.394s
[INFO] Finished at: Thu Aug 03 15:59:23 CST 2017
[INFO] Final Memory: 7M/303M
[INFO] ------------------------------------------------------------------------
angi@angi-deepin:~/workspace/sample-spring-javaconfig$
```

继续详细了解goal

```shell
$ mvn compiler:help -Ddetail=true -Dgoal=compile
[INFO] Scanning for projects...
[INFO]                                                                         
[INFO] ------------------------------------------------------------------------
[INFO] Building sample-spring-javaconfig 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-compiler-plugin:2.3.2:help (default-cli) @ sample-spring-javaconfig ---
[INFO] org.apache.maven.plugins:maven-compiler-plugin:2.3.2

Maven Compiler Plugin
  The Compiler Plugin is used to compile the sources of your project.

compiler:compile
  Compiles application sources

  Available parameters:

    annotationProcessors
      Names of annotation processors to run. Only applies to JDK 1.6+ If not
      set, the default annotation processors discovery process applies.

    compilerArgument
      Sets the unformatted argument string to be passed to the compiler if fork
      is set to true.
      
      This is because the list of valid arguments passed to a Java compiler
      varies based on the compiler version.

    compilerArguments
      Sets the arguments to be passed to the compiler (prepending a dash) if
      fork is set to true.
      
      This is because the list of valid arguments passed to a Java compiler
      varies based on the compiler version.

    compilerId (Default: javac)
      The compiler id of the compiler to use. See this guide for more
      information.

    compilerVersion
      Version of the compiler to use, ex. '1.3', '1.5', if fork is set to true.

    debug (Default: true)
      Set to true to include debugging information in the compiled class files.

    debuglevel
      Keyword list to be appended to the -g command-line switch. Legal values
      are none or a comma-separated list of the following keywords: lines, vars,
      and source. If debuglevel is not specified, by default, nothing will be
      appended to -g. If debug is not turned on, this attribute will be ignored.

    encoding (Default: ${project.build.sourceEncoding})
      The -encoding argument for the Java compiler.

    excludes
      A list of exclusion filters for the compiler.

    executable
      Sets the executable of the compiler to use when fork is true.

    failOnError (Default: true)
      Indicates whether the build will continue even if there are compilation
      errors; defaults to true.

    fork (Default: false)
      Allows running the compiler in a separate process. If 'false' it uses the
      built in compiler, while if 'true' it will use an executable.

    generatedSourcesDirectory (Default:
    ${project.build.directory}/generated-sources/annotations)
      Specify where to place generated source files created by annotation
      processing. Only applies to JDK 1.6+

    includes
      A list of inclusion filters for the compiler.

    maxmem
      Sets the maximum size, in megabytes, of the memory allocation pool, ex.
      '128', '128m' if fork is set to true.

    meminitial
      Initial size, in megabytes, of the memory allocation pool, ex. '64', '64m'
      if fork is set to true.

    optimize (Default: false)
      Set to true to optimize the compiled code using the compiler's
      optimization methods.

    outputFileName
      Sets the name of the output file when compiling a set of sources to a
      single file.

    proc
      Sets whether annotation processing is performed or not. Only applies to
      JDK 1.6+ If not set, both compilation and annotation processing are
      performed at the same time.
      
      Allowed values are: none - no annotation processing is performed. only -
      only annotation processing is done, no compilation.

    showDeprecation (Default: false)
      Sets whether to show source locations where deprecated APIs are used.

    showWarnings (Default: false)
      Set to true to show compilation warnings.

    source (Default: 1.5)
      The -source argument for the Java compiler.

    staleMillis (Default: 0)
      Sets the granularity in milliseconds of the last modification date for
      testing whether a source needs recompilation.

    target (Default: 1.5)
      The -target argument for the Java compiler.

    verbose (Default: false)
      Set to true to show messages about what the compiler is doing.


[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 0.412s
[INFO] Finished at: Thu Aug 03 16:01:12 CST 2017
[INFO] Final Memory: 6M/240M
[INFO] ------------------------------------------------------------------------
```

则可以通过如下properties定义相关属性

```xml
<maven.compiler.source>1.8</maven.compiler.source>
<maven.compiler.target>1.8</maven.compiler.target>
```

