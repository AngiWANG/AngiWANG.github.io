---
layout: post
title: "实践Spring Enable系列之EnableConfigurationProperties"
date: 2017-08-06 13:41:00 +0800
categories: Spring
tags: spring property
---

Enable support for `ConfigurationProperties` annotated beans. `ConfigurationProperties` beans can be registered in the standard way (for example using `@Bean` methods) or, for convenience, can be specified directly on this annotation.

## @ConfigurationProperties

Annotation for externalized configuration. Add this to a class definition or a `@Bean` method in a `@Configuration` class if you want to bind and validate some external Properties (e.g. from a .properties file).

Note that contrary to `@Value`, SpEL expressions are not evaluated since property values are externalized.