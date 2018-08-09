---
layout: post
title: "实践在Linux上配置SFTP服务"
date: 2018-08-09 13:53:00 +0800
categories: Linux
tags: linux sftp
---

## 配置sftp服务

以下操作都在root用户下执行

```shell
# 添加组
$ groupadd sftpusers
# 添加sftp用户，不允许ssh登录
$ useradd -g sftpusers -s /sbin/nologin testuser
# 用户根目录属主必须是root
$ chown root:sftpusers /home/testuser
# 权限可以是755或者750
$ chmod 755 -R /home/testuser
$ passwd testuser
```

由于用户根目录不允许上传，只能下载

新建testuser可以上传的目录（755），其他用户默认不能上传

```shell
$ mkdir /home/testuser/uploads
$ chown testuser:sftpusers /home/testuser/uploads
```

新建testuser不能上传，但是非sftpuser用户可以上传的目录

```shell
$ mkdir /home/testuser/downloads

$ chown testuser:sftpusers /home/testuser/downloads
$ chown 557 -R /home/testuser/downloads
# 或者直接修改目录属主为其他非sftpuser用户
$ chown ctsapp:sftpusers /home/testuser/downloads
```



备注：其实新建的目录是否能够上传是看Linux的目录权限配置。

编辑`/etc/ssh/sshd_config`

注释下面这行，不需要了

```
# Subsystem sftp /usr/libexec/openssh/sftp-server 
```

添加下面6行

```
Subsystem sftp internal-sftp
Match Group sftpusers # 匹配用户组
ChrootDirectory /home/%u # 锁定用户访问的根文件夹，属主必须是root
ForceCommand internal-sftp
AllowTcpForwarding no
X11Forwarding no # 设置不允许SSH的X转发
```

重启服务

```shell
$ service sshd restart
```

### 测试

```shell
$ sftp testuser@192.168.70.139
testuser@192.168.70.139's password: 
Connected to 192.168.70.139.
sftp> pwd
Remote working directory: /
sftp> ls
work  
sftp> put LICENSE.txt 
Uploading LICENSE.txt to /LICENSE.txt
remote open("/LICENSE.txt"): Permission denied
sftp> cd work/
sftp> put LICENSE.txt 
Uploading LICENSE.txt to /work/LICENSE.txt
LICENSE.txt
sftp>
```

## 常用命令

### put

上传文件

```
sftp>put test.txt
```

上传文件夹

```
sftp> put -r test
```

备注：上传文件夹则需要远程服务器有同名文件夹，否则报错。

### get

下载文件

```
sftp>get test.txt
```

下载文件夹

```
sftp>get -r test
```

### lcd

切换本地目录，即从本地上传或下载到本地的本地相对目录

## QA

### ChrootDirectory配置目录的属主非root

```shell
$ sftp testuser@192.168.70.139
testuser@192.168.70.139's password: 
packet_write_wait: Connection to 192.168.70.139 port 22: Broken pipe
Couldn't read packet: Connection reset by peer
```

ChrootDirectory配置的目录的属主不是root导致