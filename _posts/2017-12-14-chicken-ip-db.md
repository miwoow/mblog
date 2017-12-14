---
layout: posts
title: GIT提交了一个肉鸡IP库
---

## {{ page.title }}

### 问题背景

前段时间公司的游戏遭遇了大量的CC攻击。攻击者发动大量肉鸡和服务器建立TCP连接并且发送垃圾数据。


### 发现以及整理过程

服务器本身具有垃圾数据发现和丢弃的能力。所以服务器将发现垃圾数据的IP地址记录日之后。通过分析日志，将这些肉鸡IP地址加入到了ipset中拒绝连接。

### IP库下载地址

<https://github.com/miwoow/bscan/tree/master/hackip>

