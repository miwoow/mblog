---
layout: posts
title: freebsd 使用总结
---

## {{ page.title }}

### 挂载msdos分区，显示中文文件名：

    sudo mount_msdosfs -L zh_CN.UTF-8 /dev/da0 /mnt

### freebsd 使用 jfbterm

因为最近发现freebsd运行xfce的时候电脑会变的很烫，所以暂时不计划长时间使用X环境了。这样就催生了在term下显示中文的问题：

    sudo pkg install jfbterm

安装jfbterm之后，启动会出错，因为路径不对，一个字体文件找不到，替换为正确的路径就可以了。所以，需要替换fontset那行为：

    fontset: iso10646.1,pcf,U,/usr/local/share/fonts/gnu-unifont/unifont.pcf.gz

### freebsd 切换音箱和耳机

    sudo sysctl hw.snd.default_unit=1

我这里默认是0，输出到音箱。设置为1之后输出到耳机了。不同的电脑设置的值可能不一样。

### freebsd 内核支持ipfw

在内核配置文件中添加下面几行：
    
    options     IPFIREWALL      #firewall
    options     IPFIREWALL_VERBOSE  #enable logging to syslogd(8)
    options     IPFIREWALL_VERBOSE_LIMIT=100    #limit verbosity
    options     IPFIREWALL_DEFAULT_TO_ACCEPT    #allow everything by default
    options     IPFIREWALL_NAT      #ipfw kernel nat support
    options     IPDIVERT        #divert sockets
    options     LIBALIAS

注意：启用nat支持之后，最后一样必须要添加的，否则会报错。详细信息可以看：/usr/src/sys/conf/NOTES。

### ipfw 丢掉某个IP地址发来的tcp rst包

因为有的防火墙会发送rst包来重置tcp链接。可以使用这个命令丢掉这些数据包，增加访问成功的可能性。

    ipfw add 110 deny tcp from IP地址 to any tcpflags rst

对应的iptables命令：

    iptables -A INPUT -t filter -p tcp --tcp-flags rst rst -s IP地址 -j DROP

iptables的命令用在Android手机上还是挺好使的。

### pkg install size mismatch 解决

有的时候安装软件出现size mismatch导致安装失败。运行下面的命令解决。

    sudo pkg update -f
