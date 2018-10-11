---
layout: post
title: "实践SecureCRT"
date: 2018-10-11 09:58:13 +0800
categories: 工具
tags: SecureCRT tool
---

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