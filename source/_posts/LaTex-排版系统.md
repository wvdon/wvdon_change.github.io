---
title: 'LaTex:排版系统'
date: 2019-07-19 00:11:41
tags:
  - LaTex
categories: tools
mathjax: true
---

最后更新时间:2019.10.24

### 前言：

LaTex 是一种基于ΤΕΧ的排版系统，开发之初目的利用这种格式，人们在没有程序设计与排版的情况下也能利用其强大的功能在很短的时间内完成书籍的排版。然而其对于复杂表格和数学公式的展示具有很优秀的表现，在很多文字排版系统都可以看得到LaTex的插件。
目前我使用LaTex的原因是，他能够帮助我在MarkDown的书写过程中快速的插入数学公式。

> 1. 有些站点不支持LaTex语法的我们可以借助[在线 LaTeX 编辑器](https://www.codecogs.com/latex/eqneditor.php) 进行转换，然后复制为html代码插入进去
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

求和压缩符合

> \sum_{i=0}^{n}{(x_i+y_i)}
>
> \prod_{i=0}^{n}{x_i\cdot{y_i}}_
>
> \lim_{x\to0}\frac{sinx}{x}_
>
> \int_{a}^{b}{sinx}dx
>
> <a href="https://www.codecogs.com/eqnedit.php?latex=\sum{i=0}^{n}{(x_i&plus;y_i)}\" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\sum{i=0}^{n}{(x_i&plus;y_i)}\" title="\sum{i=0}^{n}{(x_i+y_i)}\" /></a>
>
> <a href="https://www.codecogs.com/eqnedit.php?latex=\prod{i=0}^{n}{x_i\cdot{y_i}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\prod{i=0}^{n}{x_i\cdot{y_i}}" title="\prod{i=0}^{n}{x_i\cdot{y_i}}" /></a>
>
> <a href="https://www.codecogs.com/eqnedit.php?latex=\lim{x\to0}\frac{sinx}{x}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\lim{x\to0}\frac{sinx}{x}" title="\lim{x\to0}\frac{sinx}{x}" /></a>
>
> <a href="https://www.codecogs.com/eqnedit.php?latex=\int_{a}^{b}{sinx}dx" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\int_{a}^{b}{sinx}dx" title="\int_{a}^{b}{sinx}dx" /></a>

根号 

>  \sqrt[x]{y}
>
> <a href="https://www.codecogs.com/eqnedit.php?latex=\sqrt[x]{y}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\sqrt[x]{y}" title="\sqrt[x]{y}" /></a>

## 参考

[参考](https://www.cnblogs.com/endlesscoding/p/9797237.html)