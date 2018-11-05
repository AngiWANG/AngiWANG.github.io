---
layout: post
title: "实践代码质量管理之SonarQube"
date: 2018-08-22 11:22:00 +0800
categories: 测试
tags: test Sonar SonarQube
---

[SonarQube](https://www.sonarqube.org/), The leading product for Continuous Code Quality.

## 安装

下载（sonarqube-7.4.zip）解压即可。

新建数据库schema和user。

修改配置文件sonar.properties，设置数据库连接相关信息。

```shell
$ sonar.sh { console | start | stop | restart | status | dump }
```

## plugin

插件可以通过Web页面上的marketplace安装，或者人工安装插件jar到`extensions/plugins`目录。

[Community plugins for SonarQube](https://github.com/SonarQubeCommunity)

[[Plugin Library](https://docs.sonarqube.org/display/PLUG/Plugin+Library)](https://docs.sonarqube.org/display/PLUG/Plugin+Library)

[[Plugin Version Matrix](https://docs.sonarqube.org/display/PLUG/Plugin+Version+Matrix)](https://docs.sonarqube.org/display/PLUG/Plugin+Version+Matrix)

SonarJava

[findbugs](https://github.com/spotbugs/sonar-findbugs/)

[cobertura](https://github.com/galexandre/sonar-cobertura)

[checkstyle](https://github.com/checkstyle/sonar-checkstyle)

[pmd](https://github.com/jensgerdes/sonar-pmd)

[chinese pack](https://github.com/SonarQubeCommunity/sonar-l10n-zh)

[Android Lint](https://github.com/ofields/sonar-android)