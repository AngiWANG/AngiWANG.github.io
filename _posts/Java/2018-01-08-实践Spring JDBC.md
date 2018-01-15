---
layout: post
title: "实践Spring JDBC"
date: 2018-01-08 14:05:00 +0800
categories: java
tags: database jdbc java spring-jdbc
---

Spring JDBC是Spring对数据库访问的初级封装

## *Template

```
JdbcOperations
```

### JdbcTemplate

KeyHolder

RowMapper

ResultSetExtractor

PreparedStatementSetter

PreparedStatementCreator

RowCallbackHandler

```sql
delete from BookInfo where bid =?
```



### NamedParameterJdbcTemplate

#### SqlParameterSource

```sql
delete from mb_mcht_info t where t.mcht_name = :mchtName
update mb_mcht_info t set t.mcht_address = :mchtAddress where t.mcht_name = :mchtName
select t.* from mb_mcht_info t where t.mcht_name = :mchtName
```



## *DaoSupport

### JdbcDaoSupport

### NamedParameterJdbcDaoSupport

```sql

```



## 异常体系

## 声明式事物