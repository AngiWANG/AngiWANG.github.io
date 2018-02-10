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

## 栅格系统

|                   | 超小屏幕-手机 (<768px) | 小屏幕-平板 (≥768px)            | 中等屏幕-桌面显示器 (≥992px)        | 大屏幕-大桌面显示器 (≥1200px)       |
| ----------------- | ---------------- | -------------------------- | -------------------------- | -------------------------- |
| 栅格系统行为            | 总是水平排列           | 开始是堆叠在一起的，当大于这些阈值时将变为水平排列C | 开始是堆叠在一起的，当大于这些阈值时将变为水平排列C | 开始是堆叠在一起的，当大于这些阈值时将变为水平排列C |
| `.container` 最大宽度 | None （自动）        | 750px                      | 970px                      | 1170px                     |
| 最大列宽              | 自动               | 60px                       | 78px                       | 95px                       |
| 样式前缀              | `.col-xs-`       | `.col-sm-`                 | `.col-md-`                 | `.col-lg-`                 |



```css
// 超小型是默认实现，宽度为100%
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



```html
<div class="container">
	<div class="row">
      	<div class="col-sm-4">.col-sm-4</div>
      	<div class="col-sm-4">.col-sm-4</div>
      	<div class="col-sm-4">.col-sm-4</div>
  	</div>
</div>   
```



.container：容器

.container-fluid：100%宽度容器

.row：行，每行12列，超过则换行，高度由内容自动决定

.col-*-：列

列合并

```html
<!-- 合并4列 -->
<div class="col-sm-4">.col-sm-4</div>
```

列偏移

```html
<!-- 向右偏移1列，即从第2列开始 -->
<div class="col-md-4 col-md-offset-1">col-md-4 col-md-offset-1</div>
```

列嵌套

```html
         <div class="row">
                <div class="col-sm-9">
                    col-sm-9
                    <div class="row">
                        <div class="col-xs-8">
                            col-xs-8
                        </div>
                        <div class="col-xs-4">
                            col-xs-4
                        </div>
                    </div>   
                </div>
            </div>    
```

列排序

```html
<div class="row">
  	<!-- 向右推3列 -->
	<div class="col-md-9 col-md-push-3">col-md-9 col-md-push-3</div>
  	<!-- 向左拉9列 -->
	<div class="col-md-3 col-md-pull-9">col-md-3 col-md-pull-9</div>
</div>    
```

清除浮动

```html
<div class="container">
	<div class="row">
      	<div class="col-xs-6 col-sm-3">div1: .col-xs-6 .col-sm-3</div>
      	<div class="col-xs-6 col-sm-3">div1: .col-xs-6 .col-sm-3</div>
      	<!-- 清除浮动，避免因为高度问题导致行错乱 -->
      	<div class="clearfix visible-xs"></div>
      	<div class="col-xs-6 col-sm-3">div1: .col-xs-6 .col-sm-3</div>
      	<div class="col-xs-6 col-sm-3">div1: .col-xs-6 .col-sm-3</div>
  	</div>
</div> 
```

## 组件

### 小图标

```html
<span class="glyphicon glyphicon-align-left">
```

### 下拉菜单

3.x版本开始默认不显示（display:none（.open可以打开，JavaScript插件会自动添加.open）且需要position:relative（.dropdown支持））

```html
<div class="dropdown open">
    <ul class="dropdown-menu">
        <li><a href="#">Action</a></li>
        <li><a href="#">Another action</a></li>
        <li><a href="#">Something else here</a></li>
        <li class="divider"></li>
        <li><a href="#">Separated link</a></li>
    </ul>
</div>
```



.dropdown/.dropup

dropdown trigger：下拉菜单触发器

.dropdown-menu

.dropdown-header

.divider

### 按钮

### 按钮组

### 按钮下拉菜单

类同下拉菜单，支持.dropdown换成了.btn-group

```html
<div class="btn-group open">
    <ul class="dropdown-menu">
        <li><a href="#">Action</a></li>
        <li><a href="#">Another action</a></li>
        <li><a href="#">Something else here</a></li>
        <li class="divider"></li>
        <li><a href="#">Separated link</a></li>
    </ul>
</div>
```



```html
<!-- Split button -->
<div class="btn-group open">
  	<button type="button" class="btn btn-danger">Action</button>
    <ul class="dropdown-menu">
        <li><a href="#">Action</a></li>
        <li><a href="#">Another action</a></li>
        <li><a href="#">Something else here</a></li>
        <li class="divider"></li>
        <li><a href="#">Separated link</a></li>
    </ul>
</div>
```



### 输入框组

### 导航

.nav

.nav-tabs

.nav-pills

### 导航条

`<nav/>`

.navbar

.navbar-default

.navbar-inverse

.navbar-header

.navbar-nav

.navbar-brand

.navbar-form

.navbar-toggle

.navbar-collapse

### 面包屑

.breadcrumb

### 分页

.pagination

## JavaScript插件架构

HTML布局规则：插件通过设定HTML代码和相应的属性（或自定义属性）来实现，JavaScript代码会自动检测这些标记，并自动绑定相应的事件，而无需再添加额外的JavaScript代码。

data-toggle：

alert：data-dismiss

dropdown：data-target



JavaScript实现步骤：

插件调用方法：HTML声明式或JavaScript调用式

### 下拉菜单

```html
<div class="dropdown">
    <!-- dropdown trigger -->
    <button class="btn btn-default dropdown-toggle" type="button" data-toggle="dropdown">Dropdown<span class="caret"></span>
    </button>
    <ul class="dropdown-menu">
        <li><a href="#">Action</a></li>
        <li><a href="#">Another action</a></li>
        <li><a href="#">Something else here</a></li>
        <li class="divider"></li>
        <li><a href="#">Separated link</a></li>
    </ul>
</div>
```

