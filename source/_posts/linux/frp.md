---
title: '使用frp远程访问Jupyter Notebook'
date: 2021-01-21 22:39:41
tags:
  - frp
categories: tools
mathjax: true
description: 利用frp实现外网访问局域网内的服务。
---

利用frp实现外网访问局域网内的服务。

<!-- more -->

## frp穿透

[软件下载&官方文档](https://github.com/fatedier/frp/blob/master/README_zh.md) 

可以通过下面的链接直接下载。

[下载链接](http://web.wvdon.com/frp_0.29.0_linux_amd64.tar.gz)

### 配置本地与服务端：

1. 下载解压  

```
tar -xzvf  frp_0.29.0_linux_amd64.tar.gz`
```



2. 服务器配置：frpc.ini

```
[common]
bind_port = 7000
vhost_http_port = 8080
```

启动 

```
nohup ./frps -c./frps.ini&
```

3. 客户端配置 frps.ini

```
[common]
server_addr = 111.111.110.211 #你的服务器ip
server_port = 7000

[web]
type = http
local_ip = 127.0.0.1
local_port = 8888 #要映射的jupyter端口
custom_domains = xxx.xxx.com # 映射到访问的web ,不加www 。另外需要提前将域名解析到服务器ip
nohup ./frpc -c./frpc.ini&
```

4. 后台启动 jupyter notebook 

```
nohup jupyter notebook &
```

使用cat查看是否jupyter启动端口和映射端口一致

`cat nohup.out` 一致就不需要再管了，如果不一致可以使用 ps -aux找到该进程，然后kill -9 id 杀死，重新配置然后再启动。

此时可以通过web:8080端口访问穿透的Jupyter了。

### 解决403 问题

如果遇到远程访问403，说明本地是不允许访问的。

可以通过以下方法解决403 不允许访问。

1. 进到python环境里面先设置密码

```
from notebook.auth import passwd
passwd()#设置自己的密码，例如123
#然后两次输入确认生成加密字符串
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/646855/1611238111397-1bac0fe1-11ec-42f6-8b77-a127cd7917c0.png)

记住上面产生的密码

2. 进行配置允许访问

```
jupyter notebook --generate-config
vim ~/.jupyter/jupyter_notebook_config.py
c.NotebookApp.allow_remote_access = True  #允许远程访问
c.NotebookApp.allow_root = True          #允许root访问
c.NotebookApp.ip='*'                     # 所有ip皆可访问  
c.NotebookApp.password = '上面复制的那个字符串''    
c.NotebookApp.open_browser = False       # 禁止自动打开浏览器  
c.NotebookApp.port =8888                 # 端口
c.NotebookApp.notebook_dir = '设置Notebook启动进入的目录' 
```

3. 最后后台启动jupyter notebook

```
nohup jupyter notebook &
```

### 参考

[参考链接](https://blog.csdn.net/lawrencelue/article/details/107848856):

[官网文档解读链接](https://github.com/fatedier/frp/tree/master)