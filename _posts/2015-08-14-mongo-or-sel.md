---
layout: posts
title: mongoengine中使用或查询
---

## {{ page.title }}

### 问题描述

昨天在客户那里需要修改一个功能。在django+mongodb+mongoengine的情况下。有一个model中的字段。这个字段刚开始约定是默认为0，其他情况下为1到4.但是随着项目进展，一些人员存储数据的时候不小心。在没有这个字段的时候并没有存储0.而是直接没有存储这个字段。导致有些记录有这个字段，有些记录没有这个字段，有些记录则设置了默认值0.

当界面统计尚未处理的记录时，本来只需要查询字段为0的数量。但是现在就需要查询记录为0，记录不存在和记录为None的情况。

这就需要使用Or查询。之前没有使用过mongoengine的Or查询。

### 问题解决

查了一下资料。原来mongoengine有一个Q的累。多个Q之间可以进行&和|计算。这样就可以执行与计算和或计算。

	from mongnengine import Q

	evs = Event.objects(Q(level=0) | Q(level=None) | Q(level__exists=False))

这样就可以查询到所有的默认情况了。