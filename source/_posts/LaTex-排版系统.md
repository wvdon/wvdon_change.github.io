---
title: 'LaTex:排版系统'
date: 2019-07-19 00:11:41
tags:
  - LaTex
categories: tools
mathjax: true
---

最后更新时间:2019.08.29

### 前言：

LaTex 是一种基于ΤΕΧ的排版系统，开发之初目的利用这种格式，人们在没有程序设计与排版的情况下也能利用其强大的功能在很短的时间内完成书籍的排版。然而其对于复杂表格和数学公式的展示具有很优秀的表现，在很多文字排版系统都可以看得到LaTex的插件。
目前我使用LaTex的原因是，他能够帮助我在MarkDown的书写过程中快速的插入数学公式。

> 1. 有些站点不支持LaTex语法的我们可以借助[在线 LaTeX 编辑器](https://link.jianshu.com/?t=https://www.codecogs.com/latex/eqneditor.php) 进行转换，然后复制为html代码插入进去
> 2. hexo的许多主题都是支持LaTex的，只需在文章头文字部分设置 mathjax: true

### LaTex常用语法：

\$\$
LaTex公式
\$$

#### 分数表示

> \frac{1}{2} 代表分数 
> $$
> \frac{1}{2}
> $$
> 

#### 上下标

> _{}下标  ^{} 上标
>
> C\_{n}^{m}代表 
> $$
> C_{n}^{m}
> $$
> 