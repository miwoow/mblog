---
layout: posts 
title: Linux 全局 shadowsocks 代理
---

## {{ page.title }}


#### {{ page.date | date: "%Y-%m-%d" }}

### 起源

今天在Ubuntu下编译OpenWRT。遇到一些软件包需要从*.org网站下载，无奈被墙。下载操作写在Makefile里。不好自己修改。

在Linux服务器上运行了sslocal（Linux上的ShadowSocks客户端）之后，如何让git也使用这个代理呢？

### 解决方法

使用tsocks

这个已经十几年没更新的项目没想到还能使用。而且就在ubuntu的软件源里。

So：

    sudo apt-get install tsocks
    sudo vi /etc/tsocks.conf

看配置文件，把shadowsocks的配置写到里面。

    tsocks git clone git://xxx.org/opkg

顺利了。

tsocks 可以让本终端内的程序都使用sock4或sock5代理。运行方法：

    tsocks [programe] [args]

### 注意

我在编译openWRT的时候，opkg始终下载失败。我使用代理之后还是无法下载。

我看Make的时候，使用的是git clone http://xxx.org/git/opkg， 通过http协议来下载的。
尝试换成git协议之后就可以下载了(git clone git://xxx.org/opkg)。最终我是手动完成了opkg软件包的下载之后才继续编译的。


### 其他

因为tsocks已经十几年没更新了，所以即使有什么Bug也不会有人修复。

感兴趣的也可以试试他的后继者: socat  http://freecode.com/projects/socat/

