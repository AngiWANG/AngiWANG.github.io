---
layout: post
title: "实践SecureCRT"
date: 2018-10-11 09:58:13 +0800
categories: 工具
tags: SecureCRT tool
---

## 安装

deepin15.7下安装

```shell
$ apt list scrt-sfx
正在列表... 完成
scrt-sfx/panda 8.0.2-1118 amd64
$ sudo apt install scrt-sfx
```

破解

```shell
$ wget http://download.boll.me/securecrt_linux_crack.pl
$ sudo perl securecrt_linux_crack.pl /usr/bin/SecureCRT 
crack successful

License:

	Name:		xiaobo_l
	Company:	www.boll.me
	Serial Number:	03-94-294583
	License Key:	ABJ11G 85V1F9 NENFBK RBWB5W ABH23Q 8XBZAC 324TJJ KXRE5D
	Issue Date:	04-20-2017
```

运行SecureCRT，会提示注册，选择“Enter License Manually”，然后输入上述信息即可。

参考[Linux MacOSX SecureCRT 完全破解](https://www.boll.me/archives/680#comment-2421)

## 超时

可以通过两个入口进行设置： 
1、右击Session中的连接，选择Properties->Terminal->Anti-idle->勾选Send protocol NO-OP。 
2、当已经建立连接的情况，可以通过上面方法，另外也可以通过点击Options->Session Options->Terminal->Anti-idle->勾选Send protocol NO-OP

选中之后，后面显示60秒是默认值，可对其进行增减，只要这个时间段小于自动断开连接的时间就可以了。