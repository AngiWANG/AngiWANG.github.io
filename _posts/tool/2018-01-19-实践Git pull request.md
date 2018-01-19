---
layout: post
title: "实践Git pull request"
date: 2017-08-14 15:58:13 +0800
categories: 工具
tags: git scm "pull request"
---

1. 基于远程源版本库fork出自己的远程版本库，通过github或gitlab界面操作完成
2. 从自己的远程版本库克隆到本地（自己的远程版本库默认为origin）

```shell
$ git clone URL
```

3. 添加远程源版本库（定义为upstream）

```shell
$ git remote add upstream URL
```

4. 远程源版本库更新

```shell
$ git fetch upstream
```

5. 合并远程源版本到本地

```shell
$ git merge upstream/master
```

6. push到自己的远程版本库，这样就可以更新远程原版本库的内容到自己的远程原版本库

```shell
$ git push origin master
```

7. pull request，通过github或gitlab界面操作，等待远程原版本库作者处理