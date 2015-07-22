---
layout: posts
title: freebsd 使用总结
---

## {{ page.title }}

### 这是一些freebsd的使用经验总结

挂在msdos分区，显示中文文件名：

    sudo mount_msdosfs -L zh_CN.UTF-8 /dev/da0 /mnt
