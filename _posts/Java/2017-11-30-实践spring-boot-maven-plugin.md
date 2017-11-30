---
layout: post
title: "实践spring-boot-maven-plugin"
date: 2017-11-30 17:08:00 +0800
categories: Java
tags: spring-boot-maven-plugin spring-boot maven
---

spring-boot-starter-parent的`<pluginManagement/>`下定义了spring-boot-maven-plugin

```xml
<plugin>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-maven-plugin</artifactId>
	<executions>
		<execution>
			<goals>
				<goal>repackage</goal>
			</goals>
		</execution>
	</executions>
	<configuration>
		<mainClass>${start-class}</mainClass>
	</configuration>
</plugin>
```



## repackage

repackage，顾名思义，再打包，默认再打包成一个可执行的archive（包含依赖（非test）），原打包的archive被重命名（后面追加.original），绑定到maven的package阶段

| 参数                 | 描述                                       | 默认值   |
| ------------------ | ---------------------------------------- | ----- |
| attach             | true：安装部署repackage的包，false：安装部署原始包（不带.original后缀） | true  |
| classifier         | repackage包追加的后缀，原始包则不追加.original，此时两个包都会被安装部署 | 空     |
| excludes           |                                          |       |
| excludeGroupIds    |                                          |       |
| excludeArtifactIds |                                          |       |
| skip               | 跳过执行                                     | false |
| excludeDevtools    | exclude devtools                         | true  |
|                    |                                          |       |

### help

```shell
$ mvn help:describe -Dplugin=spring-boot -Dgoal=repackage -Ddetail
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building hello-client 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-help-plugin:2.2:describe (default-cli) @ cts-hello-client ---
[INFO] Mojo: 'spring-boot:repackage'
spring-boot:repackage
  Description: Repackages existing JAR and WAR archives so that they can be
    executed from the command line using java -jar. With layout=NONE can also
    be used simply to package a JAR with nested dependencies (and no main
    class, so not executable).
  Implementation: org.springframework.boot.maven.RepackageMojo
  Language: java
  Bound to phase: package

  Available parameters:

    attach (Default: true)
      Attach the repackaged archive to be installed and deployed.

    classifier
      Classifier to add to the artifact generated. If given, the artifact will
      be attached with that classifier and the main artifact will be deployed
      as the main artifact. If this is not given (default), it will replace the
      main artifact and only the repackaged artifact will be deployed.
      Attaching the artifact allows to deploy it alongside to the original one,
      see the maven documentation for more details.

    embeddedLaunchScript
      The embedded launch script to prepend to the front of the jar if it is
      fully executable. If not specified the 'Spring Boot' default script will
      be used.

    embeddedLaunchScriptProperties
      Properties that should be expanded in the embedded launch script.

    excludeArtifactIds
      User property: excludeArtifactIds
      Comma separated list of artifact names to exclude (exact match).

    excludeDevtools (Default: true)
      Exclude Spring Boot devtools from the repackaged archive.

    excludeGroupIds
      User property: excludeGroupIds
      Comma separated list of groupId names to exclude (exact match).

    excludes
      Collection of artifact definitions to exclude. The Exclude element
      defines a groupId and artifactId mandatory properties and an optional
      classifier property.

    executable (Default: false)
      Make a fully executable jar for *nix machines by prepending a launch
      script to the jar.
      Currently, some tools do not accept this format so you may not always be
      able to use this technique. For example, jar -xf may silently fail to
      extract a jar or war that has been made fully-executable. It is
      recommended that you only enable this option if you intend to execute it
      directly, rather than running it with java -jar or deploying it to a
      servlet container.

    finalName (Default: ${project.build.finalName})
      Required: true
      Name of the generated archive.

    includes
      Collection of artifact definitions to include. The Include element
      defines a groupId and artifactId mandatory properties and an optional
      classifier property.

    includeSystemScope (Default: false)
      Include system scoped dependencies.

    layout
      The type of archive (which corresponds to how the dependencies are laid
      out inside it). Possible values are JAR, WAR, ZIP, DIR, NONE. Defaults to
      a guess based on the archive type.

    layoutFactory
      The layout factory that will be used to create the executable archive if
      no explicit layout is set. Alternative layouts implementations can be
      provided by 3rd parties.

    mainClass
      The name of the main class. If not specified the first compiled class
      found that contains a 'main' method will be used.

    outputDirectory (Default: ${project.build.directory})
      Required: true
      Directory containing the generated archive.

    requiresUnpack
      A list of the libraries that must be unpacked from fat jars in order to
      run. Specify each library as a <dependency> with a <groupId> and a
      <artifactId> and they will be unpacked at runtime.

    skip (Default: false)
      User property: skip
      Skip the execution.


[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 0.903 s
[INFO] Finished at: 2017-11-30T14:21:45+08:00
[INFO] Final Memory: 15M/240M
[INFO] ------------------------------------------------------------------------

```

## run

运行spring boot应用。

```shell
$ mvn spring-boot:run
```

| 参数           | 描述                                       | 默认值   |
| ------------ | ---------------------------------------- | ----- |
| fork         | User property: fork，是否另起进程，当agent, jvmArguments和workingDirectory设置或devtools存在时自动为true | false |
| jvmArguments | User property: run.jvmArguments          |       |
|              |                                          |       |

### help

```shell
$ mvn help:describe -Dplugin=spring-boot -Dgoal=run -Ddetail
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building hello-client 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-help-plugin:2.2:describe (default-cli) @ cts-hello-client ---
[INFO] Mojo: 'spring-boot:run'
spring-boot:run
  Description: Run an executable archive application.
  Implementation: org.springframework.boot.maven.RunMojo
  Language: java
  Bound to phase: validate
  Before this mojo executes, it will call:
    Phase: 'test-compile'

  Available parameters:

    addResources (Default: false)
      User property: run.addResources
      Add maven resources to the classpath directly, this allows live in-place
      editing of resources. Duplicate resources are removed from target/classes
      to prevent them to appear twice if ClassLoader.getResources() is called.
      Please consider adding spring-boot-devtools to your project instead as it
      provides this feature and many more.

    agent
      User property: run.agent
      Path to agent jar. NOTE: the use of agents means that processes will be
      started by forking a new JVM.

    arguments
      User property: run.arguments
      Arguments that should be passed to the application. On command line use
      commas to separate multiple arguments.

    classesDirectory (Default: ${project.build.outputDirectory})
      Required: true
      Directory containing the classes and resource files that should be
      packaged into the archive.

    excludeArtifactIds
      User property: excludeArtifactIds
      Comma separated list of artifact names to exclude (exact match).

    excludeGroupIds
      User property: excludeGroupIds
      Comma separated list of groupId names to exclude (exact match).

    excludes
      Collection of artifact definitions to exclude. The Exclude element
      defines a groupId and artifactId mandatory properties and an optional
      classifier property.

    folders
      Additional folders besides the classes directory that should be added to
      the classpath.

    fork
      User property: fork
      Flag to indicate if the run processes should be forked. fork is
      automatically enabled if an agent, jvmArguments or working directory are
      specified, or if devtools is present.

    includes
      Collection of artifact definitions to include. The Include element
      defines a groupId and artifactId mandatory properties and an optional
      classifier property.

    jvmArguments
      User property: run.jvmArguments
      JVM arguments that should be associated with the forked process used to
      run the application. On command line, make sure to wrap multiple values
      between quotes. NOTE: the use of JVM arguments means that processes will
      be started by forking a new JVM.

    mainClass
      The name of the main class. If not specified the first compiled class
      found that contains a 'main' method will be used.

    noverify
      User property: run.noverify
      Flag to say that the agent requires -noverify.

    profiles
      User property: run.profiles
      The spring profiles to activate. Convenience shortcut of specifying the
      'spring.profiles.active' argument. On command line use commas to separate
      multiple profiles.

    skip (Default: false)
      User property: skip
      Skip the execution.

    useTestClasspath (Default: false)
      User property: useTestClasspath
      Flag to include the test classpath when running.

    workingDirectory
      User property: run.workingDirectory
      Current working directory to use for the application. If not specified,
      basedir will be used. NOTE: the use of working directory means that
      processes will be started by forking a new JVM.


[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 0.997 s
[INFO] Finished at: 2017-11-30T14:50:16+08:00
[INFO] Final Memory: 15M/240M
[INFO] ------------------------------------------------------------------------
angi@angi-deepin:~/sts_3.8.3_koolyun/demo/cts-hello-client$ 

```

## start

| 参数   | 描述   | 默认值  |
| ---- | ---- | ---- |
|      |      |      |
|      |      |      |
|      |      |      |

### help

```shell
$ mvn help:describe -Dplugin=spring-boot -Dgoal=start -Ddetail
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building hello-client 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-help-plugin:2.2:describe (default-cli) @ cts-hello-client ---
[INFO] Mojo: 'spring-boot:start'
spring-boot:start
  Description: Start a spring application. Contrary to the run goal, this
    does not block and allows other goal to operate on the application. This
    goal is typically used in integration test scenario where the application
    is started before a test suite and stopped after.
  Implementation: org.springframework.boot.maven.StartMojo
  Language: java
  Bound to phase: pre-integration-test

  Available parameters:

    addResources (Default: false)
      User property: run.addResources
      Add maven resources to the classpath directly, this allows live in-place
      editing of resources. Duplicate resources are removed from target/classes
      to prevent them to appear twice if ClassLoader.getResources() is called.
      Please consider adding spring-boot-devtools to your project instead as it
      provides this feature and many more.

    agent
      User property: run.agent
      Path to agent jar. NOTE: the use of agents means that processes will be
      started by forking a new JVM.

    arguments
      User property: run.arguments
      Arguments that should be passed to the application. On command line use
      commas to separate multiple arguments.

    classesDirectory (Default: ${project.build.outputDirectory})
      Required: true
      Directory containing the classes and resource files that should be
      packaged into the archive.

    excludeArtifactIds
      User property: excludeArtifactIds
      Comma separated list of artifact names to exclude (exact match).

    excludeGroupIds
      User property: excludeGroupIds
      Comma separated list of groupId names to exclude (exact match).

    excludes
      Collection of artifact definitions to exclude. The Exclude element
      defines a groupId and artifactId mandatory properties and an optional
      classifier property.

    folders
      Additional folders besides the classes directory that should be added to
      the classpath.

    fork
      User property: fork
      Flag to indicate if the run processes should be forked. fork is
      automatically enabled if an agent, jvmArguments or working directory are
      specified, or if devtools is present.

    includes
      Collection of artifact definitions to include. The Include element
      defines a groupId and artifactId mandatory properties and an optional
      classifier property.

    jmxName
      The JMX name of the automatically deployed MBean managing the lifecycle
      of the spring application.

    jmxPort
      The port to use to expose the platform MBeanServer if the application
      needs to be forked.

    jvmArguments
      User property: run.jvmArguments
      JVM arguments that should be associated with the forked process used to
      run the application. On command line, make sure to wrap multiple values
      between quotes. NOTE: the use of JVM arguments means that processes will
      be started by forking a new JVM.

    mainClass
      The name of the main class. If not specified the first compiled class
      found that contains a 'main' method will be used.

    maxAttempts
      The maximum number of attempts to check if the spring application is
      ready. Combined with the 'wait' argument, this gives a global timeout
      value (30 sec by default)

    noverify
      User property: run.noverify
      Flag to say that the agent requires -noverify.

    profiles
      User property: run.profiles
      The spring profiles to activate. Convenience shortcut of specifying the
      'spring.profiles.active' argument. On command line use commas to separate
      multiple profiles.

    skip (Default: false)
      User property: skip
      Skip the execution.

    useTestClasspath (Default: false)
      User property: useTestClasspath
      Flag to include the test classpath when running.

    wait
      The number of milli-seconds to wait between each attempt to check if the
      spring application is ready.

    workingDirectory
      User property: run.workingDirectory
      Current working directory to use for the application. If not specified,
      basedir will be used. NOTE: the use of working directory means that
      processes will be started by forking a new JVM.


[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 0.927 s
[INFO] Finished at: 2017-11-30T15:29:03+08:00
[INFO] Final Memory: 16M/303M
[INFO] ------------------------------------------------------------------------
```



## stop

### help

```shell
$ mvn help:describe -Dplugin=spring-boot -Dgoal=stop -Ddetail
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building hello-client 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-help-plugin:2.2:describe (default-cli) @ cts-hello-client ---
[INFO] Mojo: 'spring-boot:stop'
spring-boot:stop
  Description: Stop a spring application that has been started by the 'start'
    goal. Typically invoked once a test suite has completed.
  Implementation: org.springframework.boot.maven.StopMojo
  Language: java
  Bound to phase: post-integration-test

  Available parameters:

    fork
      User property: fork
      Flag to indicate if process to stop was forked. By default, the value is
      inherited from the MavenProject. If it is set, it must match the value
      used to StartMojo start the process.

    jmxName
      The JMX name of the automatically deployed MBean managing the lifecycle
      of the application.

    jmxPort
      The port to use to lookup the platform MBeanServer if the application has
      been forked.

    skip (Default: false)
      User property: skip
      Skip the execution.


[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 0.899 s
[INFO] Finished at: 2017-11-30T15:34:58+08:00
[INFO] Final Memory: 16M/303M
[INFO] ------------------------------------------------------------------------

```



## build-info



### help

```shell
$ mvn help:describe -Dplugin=spring-boot -Dgoal=build-info -Ddetail
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building hello-client 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-help-plugin:2.2:describe (default-cli) @ cts-hello-client ---
[INFO] Mojo: 'spring-boot:build-info'
spring-boot:build-info
  Description: Generate a build-info.properties file based the content of the
    current MavenProject.
  Implementation: org.springframework.boot.maven.BuildInfoMojo
  Language: java
  Bound to phase: generate-resources

  Available parameters:

    additionalProperties
      Additional properties to store in the build-info.properties. Each entry
      is prefixed by build. in the generated build-info.properties.

    outputFile (Default:
    ${project.build.outputDirectory}/META-INF/build-info.properties)
      The location of the generated build-info.properties.
```



