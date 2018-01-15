---
layout: post
title: "实践MyBatis Generator（MBG）"
date: 2018-01-08 14:05:00 +0800
categories: Java
tags: database jpa java
---

[MyBatis Generator (MBG)](http://www.mybatis.org/generator/)

## generatorConfig.xml

### <classPathEntry>

### <context>

defaultModelType：生成model类型，默认值conditional，建议设置为`flat`

* conditional：跟hierarchical类似，只是当表只有一个主键时，不生成其他类
* flat：一个表一个domain类
* hierarchical：生成多个类，`主键`一个，`Blob`字段共一个类，其他字段共一个类



targetRuntime：

#### <properties>

#### <plugin>

#### <commentGenerator>

suppressDate：是否包含时间戳，默认值false，避免重复生成后时间戳的变化导致版本变化，建议设置为true

addRemarkComments：是否包含表和字段的备注，默认值false，建议设置为true

####　<connectionFactory>

#### <jdbcConnection>

#### <javaTypeResolver>

forceBigDecimals：是否强制`DECIMAL`和`NUMERIC`类型的字段转换为Java类型的`java.math.BigDecimal`,默认值为`false`

默认情况下的转换规则为：

1. 如果`精度>0`或者`长度>18`，就会使用`java.math.BigDecimal`
2. 如果`精度=0`并且`10<=长度<=18`，就会使用`java.lang.Long`
3. 如果`精度=0`并且`5<=长度<=9`，就会使用`java.lang.Integer`
4. 如果`精度=0`并且`长度<5`，就会使用`java.lang.Short`

#### <javaModelGenerator>

**targetProject：**This is used to specify a target project for the generated objects.  绝对路径或相对路径，不自动创建目录，关于相对目录，不同运行方式规则不一样，命令行和maven方式，相对项目根目录，比如：`src/main/java`，eclipse plug-in方式，相对工作空间，前面追加项目名称，比如：`sample-mybatis-generator-maven/src/main/java`

**targetPackage：**This is the package where the generated classes will be placed.  In the default generator, the property "enableSubPackages" controls how the actual package is calculated.  If true, then the alculated package will be the targetPackage plus sub packages for the table's catalog and schema if they exist. If false (the default) then the calculated package will be exactly what is specified in the targetPackage attribute.      MyBatis Generator will create folders as required for the generated packages.会自动创建目录

#### <sqlMapGenerator>

#### <javaClientGenerator>

#### <table>

tableName：表名，支持SQL通配符，比如%

## 运行

### 命令行

项目下任意目录新建`generatorConfig.xml`，执行如下命令：

```shell
$ java -jar mybatis-generator-core-1.3.6.jar -configfile src/maing/resources/generatorConfig.xml -overwrite
$ java -cp mybatis-generator-core-x.x.x.jar org.mybatis.generator.api.ShellRunner -configfile generatorConfig.xml -overwrite
```

### maven

添加`mybatis-generator-maven-plugin`

```xml
	<build>
		<plugins>
			<plugin>
				<groupId>org.mybatis.generator</groupId>
				<artifactId>mybatis-generator-maven-plugin</artifactId>
				<version>1.3.6</version>
			</plugin>
		</plugins>
	</build>
```

`src/main/resources`目录下新建`generatorConfig.xml`，也可以通过属性自定义位置

内容参考如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
  "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
	<classPathEntry
		location="/media/angi/Resource/Maven-Repo/mysql/mysql-connector-java/5.1.45/mysql-connector-java-5.1.45.jar" />

	<context id="mysqlTables" defaultModelType="flat">
		<commentGenerator>
			<property name="suppressDate" value="true" />
			<property name="addRemarkComments" value="true"/>
		</commentGenerator>
		
		<jdbcConnection driverClass="com.mysql.jdbc.Driver"
			connectionURL="jdbc:mysql://localhost:3306/test" userId="root"
			password="mysql">
		</jdbcConnection>

		<javaModelGenerator targetPackage="wang.angi.sample.mybatis.generator.maven.model"
			targetProject="src/main/java">
		</javaModelGenerator>

		<sqlMapGenerator targetPackage="wang.angi.sample.mybatis.generator.maven.mapper"
			targetProject="src/main/resources">
		</sqlMapGenerator>

		<javaClientGenerator type="XMLMAPPER"
			targetPackage="wang.angi.sample.mybatis.generator.maven.mapper"
			targetProject="src/main/java">
		</javaClientGenerator>

		<table tableName="city">
			<generatedKey column="id" sqlStatement="MySql" identity="true" />
		</table>
	</context>
</generatorConfiguration>
```



```shell
$ mvn -Dmybatis.generator.overwrite=true mybatis-generator:generate
```

### Eclipse plug-in

项目下任意目录新建`generatorConfig.xml`，右键`generatorConfig.xml`，选择Run As>Run MyBatis Generator

与maven生成的代码格式略不同，会导致版本管理工具识别出被修改过