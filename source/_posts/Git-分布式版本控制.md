---
title: 'Git:分布式版本控制'
date: 2019-08-28 23:42:57
tags: [git]
categories: tools
description: 生产力工具
---

## Git简介

#### 什么是Git

> git是一种分布式版本控制系统

##### 什么是版本控制系统：

> 

##### 什么是分布式&集中式：

>  优势：
>
> SVN

##### Git的历史：

>  2002年大神Linus 为了管理Linux源码，花了两个星期使用c语言编写的。

#### 安装使用

windows上在[官网](https://git-scm.com/downloads)下载安装即可
命令行验证一下 git 是否安装完成。

ubuntu 上

```shell
sudo apt-get install git
```

### Git学习笔记

#### 1、建立与连接远程仓库

建立git仓库

``` git init```  

添加项目中的文件  

```git add *```  

提交到仓库

```git commit -m "注释"```

与远程仓库建立关联  

```git remote add origin https://github.com/wvdon/learn_tools.git```

上传之前pull一下

```git pull origin master```  

上传到GitHub远程仓库（master分支）

```git push -u origin master```

#### 2、git基本操作

查看仓库状态

```git status```

## 参考资料

[廖雪峰git教程](https://www.liaoxuefeng.com/wiki/896043488029600)

精通Git（第2版）

![1567175758887](C:\Users\wvdon\AppData\Roaming\Typora\typora-user-images\1567175758887.png)