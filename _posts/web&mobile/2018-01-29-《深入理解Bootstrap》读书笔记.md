---
layout: post
title: "《深入理解Bootstrap》读书笔记"
date: 2018-01-29 09:03:13 +0800
categories: 移动互联网
tags: bootstrap javascript web mobile
---

作者徐涛，基于Bootstrap 3.1.0



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
  	<meta http-equiv="X-UA-Compatible" content="IE=edge"/>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
  	<title>Bootstrap基础模板</title>
    <link href="css/bootstrap.css" rel="stylesheet">
  	<!-- 以下2个插件是用于在IE8支持HTML5元素和媒体查询的，如果不用可移除 -->
  	<!-- 注意：Respond.js不支持file://方式的访问 -->
  	<!--[if lt IE 9]>
	<script src="css/respond.min.js"></script>
	<script src="css/html5shiv.js"></script> 
	<![endif]-->
</head>
  <body>
  	<script src="js/jquery.js"></script>
    <script src="js/bootstrap.js"></script>
  </body>
</html>
```

meta query

user-scalable=no

maximum-scale=1.0

### 栅格系统

```css
// 超小型是默认实现
// 小型
@media (min-width: 768px){
	.container{width:750px}
}
// 中型
@media (min-width: 992px){
	.container{width:970px}
}
// 大型
@media (min-width: 1200px){
	.container{width:1170px}
}

```



### JavaScript插件架构

HTML布局规则：插件通过设定HTML代码和相应的属性（或自定义属性）来实现，JavaScript代码会自动检测这些标记，并自动绑定相应的事件，而无需再添加额外的JavaScript代码。

alert：data-dismiss

dropdown：data-target

JavaScript实现步骤：

插件调用方法：HTML声明式或JavaScript调用式