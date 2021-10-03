---
title: 'Ubuntu18下破解xmind8'
date: 2019-02-10 12:10:30
tags: 
  - xmind
categories:
mathjax: true
---



## 工具下载

  xmind8安装包 <a href="https://www.xmind.cn/download/xmind8">官网地址</a>

  破解文件  <a href="https://github.com/wvdon/soft/blob/master/xmind/XMind_amd64.tar.gz">下载地址</a>

  平台:ubuntu 18.04lts

将下载的安装包解压到指定目录
` unzip xmind-8-update8-linux.zip`
解压破解文件 将下载的破解文件解压到XMind_amd64/ 并选择替换
修改host


<pre><code>sudo vim /etc/hosts
</code></pre>

在最后添加

<pre><code>127.0.0.1 www.xmind.net
</code></pre>

安装软件 回到安装目录 

![](https://img-blog.csdnimg.cn/20190803130354496.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1dlRG9uX3Q=,size_16,color_FFFFFF,t_70)

执行  
`sudo ./setup.sh`    
进入XMind_amd64/ 点击运行XMind

点击 帮助 序列号
输入 邮箱（随便输入）
输入序列号（复制下面的） 
`XAka34A2rVRYJ4XBIU35UZMUEEF64CMMIYZCK2FZZUQNODEKUHGJLFMSLIQMQUCUBXRENLK6NZL37JXP4PZXQFILMQ2RG5R7G4QNDO3PSOEUBOCDRYSSXZGRARV6MGA33TN2AMUBHEL4FXMWYTTJDEINJXUAV4BAYKBDCZQWVF3LWYXSDCXY546U3NBGOI3ZPAP2SO3CSQFNB7VVIY123456789012345`

激活成功 

![](https://img-blog.csdnimg.cn/2019080313040536.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1dlRG9uX3Q=,size_16,color_FFFFFF,t_70)

## 创建桌面快捷方式

进入到XMind_amd64/并创建运行脚本文件
`vim run.sh`
输入以下
`cd /usr/soft/xmind/xmind-8-update8-linux/XMind_amd64/`
`/usr/soft/xmind/xmind-8-update8-linux/XMind_amd64/XMind`
为run.sh加上可执行权限
`chmod +x ./run.sh`
进入到applications目录下
`cd /usr/share/applications/`
创建xmind.desktopp
`sudo vim xmind.desktop`

输入以下

<pre><code>[Desktop Entry]
Name=XMind
Exec=/usr/soft/xmind/xmind-8-update8-linux/XMind_amd64/run.sh
Icon=/home/wvdon/space/down/macOS11/apps/128/xmind.png
Path=/usr/soft/xmind/xmind-8-update8-linux/XMind_amd64/
Type=Application
Categories=GTK;GNOME;Office;
</code></pre>

<blockquote>
  字段解释：<br />
  Name: 应用文件名，本例中此处填写 XMind 。<br />
  Exec: 应用执行路径，必须准确填写。<br />
  Icon: 图标路径。(自己选择下载的图片所在路径） 
  Path：应用所在路径。 
  Type: .desktop 类型，此处我们应填写 Application 。<br />
  sudo chmod a+x XMind.desktop
  6.复制XMind.desktop 到桌面粘贴即可创建快捷方式
    也可以 super 搜索 xmind 添加到收藏夹
      <img src="http://wvdon.com/&quot;wp-content/uploads&quot;/2019/02/2019-02-10-15-32-54屏幕截图.png" alt="" />
</blockquote>
---

