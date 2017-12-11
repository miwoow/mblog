---
layout: posts
title: GIT提交超过100M的大文件
---

## {{ page.title }}

### 问题背景

有的时候GIT上需要管理.sql, .txt, .png, .psd, .db, .datax等二进制数据。这种二进制数据有的可能比较大。对于github来说，能提交的文件最大不能超过100M。这些超过100M的文件无法直接提交到github上去。所以就需要使用git large file storage。

### Git LFS 安装

<https://github.com/git-lfs/git-lfs/wiki/Installation>

### Git LFS 使用

<https://git-lfs.github.com/>
