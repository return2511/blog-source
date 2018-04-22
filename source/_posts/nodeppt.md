---
title: nodeppt的使用
date: 2018-3-30
tags: [nodeppt]
categories: tools
---

> nodeppt使用的基本介绍。

<!-- more -->

### 背景
在豚厂的这段时间，说实话还是学到了很多东西的。

- 接受react的相关工作，把react从理论第一次搬到实践中，深刻感觉到理论和实践的差别。
- 每周一次的分享会，大家会分享自己的经验

nodeppt是我在做分享会的时候使用的。因为我不想用ppt，或者说自己的ppt做的很烂，感觉特别low。所幸就自己查找相关的工具，在一番选择之后，自己选用了nodeppt这个工具。

### nodeppt 
#### 安装
基于node，很显然npm安装啊

```
npm install -g nodeppt
```
#### 启动

```
# 获取帮助
nodeppt start -h
# 绑定端口
nodeppt start -p <port>
nodeppt start -p 8090 -d path/for/ppts
# 绑定host，默认绑定0.0.0.0
nodeppt start -p 8080 -d path/for/ppts -H 127.0.0.1
# 使用socket通信（按Q键显示/关闭二维码，手机扫描，即可控制）
# socket须知：1、注意手机和pc要可以相互访问，2、防火墙，3、ip
```

#### 语法

基于markdown的语法编写文档

这样就可以完成一个基本的在线ppt

### 参考文档

其实自己也就用了一次，对很多动画效果，回调函数，以及嵌入html语言等等，我都没有仔细研究，写此文的目的也就是做一个记录。方便日后使用。

### 插曲

当天分享，由于是第一次分享，结果到了会议室才发现自己的电脑忘记开远程了，所以就直接用nodeppt生成的ip去访问，发现在一个网段内竟然可以直接开放访问，不知道是豚厂的it没有限制呢，还是什么原因，以待后续研究。

参考文档：[nodePPT - 让你爱上做分享！](https://www.awesomes.cn/repo/ksky521/nodeppt)
