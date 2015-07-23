---
layout: posts
title: freebsd 配置 realtek 无线网卡
---

## {{ page.title }}

### 问题描述

很早就欢了Freebsd 的系统，但是系统的无线网卡一直没有驱动器来。今天早jd上买了一个便宜的usb无线网卡，现在总算是不许要在家里拉网线了。但是要配置还是比较麻烦的。

### 问题解决

内核就不说了，默认的内核就已经包含了使用无线网络所需要的模块。如果你对内核还有问题，可以参考手册自己把相应的内核模块编译进去。

    sudo pkg install wpa_supplicant   #安装wpa客户端
    sudo vi /etc/wpa_supplicant.conf   # 加入你的无线网络配置
    例如：我的配置如下
    network={
	ssid="myssid"
    	psk="mypass"
    }

修改rc.conf:

    wlans_urtwn0="wlan0"
    ifconfig_wlan0="WPA SYNCDHCP"

因为我使用的是realtek网卡，所以系统里识别出来的是urtwn0网卡。网卡名称需要根据情况确定。

realtek网卡还需要接受一个什么协议。所以需要在/boot/load.conf里面加上：

    legal.realtek.license_ack=1

然后重启网络：

    sudo service netif restart

现在应该就可以使用无线上网了。


