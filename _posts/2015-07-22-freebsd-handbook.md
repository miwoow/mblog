---
layout: posts
title: freebsd 使用总结
---

## {{ page.title }}

### 这是一些freebsd的使用经验总结

挂载msdos分区，显示中文文件名：

    sudo mount_msdosfs -L zh_CN.UTF-8 /dev/da0 /mnt

因为最近发现freebsd运行xfce的时候电脑会变的很烫，所以暂时不计划长时间使用X环境了。这样就催生了在term下显示中文的问题：

    sudo pkg install jfbterm

安装jfbterm之后，启动会出错，因为路径不对，一个字体文件找不到，替换为正确的路径就可以了。所以，需要替换fontset那行为：

    fontset: iso10646.1,pcf,U,/usr/local/share/fonts/gnu-unifont/unifont.pcf.gz


