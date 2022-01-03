---
title: 'Hexo:使用记录'
date: 2019-09-27 23:42:57
tags: 
	- hexo
categories: [tools]
description: 生产力工具
---

> 快速、简洁且高效的博客框架。一键部署博客到Github Pages，并原生态支持Markdown。

文件头

> ```yaml
> ---
> title: '题目'
> date: 2019-08-02 15:50:39
> tags: 
>   - 1
>   - 2
>   - 3
> categories: [分类]
> mathjax: true
> description: 描述
> ---
> ```
>
> 

备忘：

> hexo s 部署到本地
>
> hexo g 生成静态文件
>
> hexo d 部署到github上

重新部署本地时

> 安装node.js  url地址是 
>
> https://nodejs.org/dist/ 。（高版本在同步到git 可能会出现问题`TypeError [ERR_INVALID_ARG_TYPE]: The "mode" argument must be integer. Received an instance of Object`）
>
> 选择低版本 `https://nodejs.org/dist/v12.4.0/node-v12.4.0-x64.msi`
>
> npm安装 ` npm install -g hexo-cli ` 



`<!-- more -->` 可以手动截取显示的字数。
