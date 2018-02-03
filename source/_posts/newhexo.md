---
title: windows搭建hexo，部署至github
date: 2016-07-28 00:33:25
tags: [hexo搭建]
categories: "hexo"
---

> 第一次玩hexo时记录的日志，一篇hexo搭建教程。

<!-- more -->
### 背景
本人半吊子前端研发一枚，phper。
其实对于php，一开始我是抗拒的，机缘巧合，2014年7月毕业之后,误打误撞进入了大PHP开发行列。但一年半的php研发，几乎都是做的服务端的工作，因为我是对前端页面，html，js这些东西也是很抗拒的。
于2015年12月31日，和我的人生中第一份工作say goodbye，仿佛剧本都是一样的，入职大途牛之后，一些抗拒的事情又迎面过来了，慢慢的开始接触写js。然而我是个奇葩，在几乎没有js基本功的时候，组内PC开发用的JQ，wap页面开发用的zepto，然后我写了半年的zepto和JQ。
至此，不知道自己到底是属于什么，以至于我现在简历都不知道自己的求职方向是什么~
stop，废话扯得有点儿多，言归正传，作为一个phper，一直想给自己搭一个blog，但是苦于一者没有足够的时间（纯属扯淡），二者我尝试过了N多php开源的框架，但最终都被我pass掉了。我感觉那些php框架有点过于笨重，直至前几天，和[表哥](https://mazhichao300.github.io/)闲聊，他给我推荐了markdown，我深深的被安利了，然后他推荐给了我jekyll 和 hexo 。
我大致百度了一下这两个框架，最后我选择了基于node的hexo，另外一个没用也不好说其好与坏。反正我玩hexo才两天的感觉就是，即使一个人不会写代码，稍微懂点计算机知识，也是可以很快的使用hexo搭建一个自己的blog。很赞。
这篇文章一者是用来记载搭建hexo的过程，二者是为自己的blog写第一篇文章。
废话到此结束，进入正文。

### 安装流程
本人作为一个混迹在程序猿世界里的末流开发，竟然没用过github，感到很惭愧。
本文仅针对windows
- git
- node.js
- github 账号

#### 安装git
git官网下载安装git，至于版本什么的，我不知道有啥区别，反正我安装的是最新版，个人有强迫症，有新的不安装旧的~  git命令什么的，熟悉点最好，（本人表示也仅仅知道一点点，尴尬。。。）
#### 安装node
如果您和我一样有强迫症，那也选择最新版吧，虽然我也不知道有啥区别。感觉新的好吧，哈哈哈。。。在别人的经验上看到安装完node 要重启下电脑，可能是要配置什么东东吧，建议重启下吧。安装步骤，默认安装即可。
#### 偏不写申请github账号，先本地部署吧
git和node安装完成之后，首先在本地搭建个环境吧。
1. 任意位置Git Bash here 输入 
```
npm install hexo-cli -g
```
2. 待安装完hexo之后，可输入
```
hexo -v
```
查看hexo版本号。正确安装之后，下一步需要init hexo。code为init hexo <文件夹> 。建议首先新建一个文件夹用来存储该项目，然后在该项目内鼠标右键 Git Bash here ，输入
```
hexo init
```
这个时候就可以看到该文件夹下面自动生成了项目，是不是感觉有一点点小激动呢~别着急，马上就好。
3. hexo源文件使用markdown，在/source/_posts/目录下，新建的文件下面有一个hello-world.md 文件，这个时候我们可以输入
```
hexo generate
```
该命令会把source下面的md文件都生成html，最终生成的文件在/public 目录下。
4. 这个时候已经在本地生成了一个完整的项目了，然后本地执行
```
hexo server 
```
就会完成本地服务的启动，然后访问localhost:4000 就可以看到一个beautiful hexo啦~
#### 新建文章
新建文章使用git命令
```
hexo new "your post name" 
```
这时候就会生成一个.md 文件，然后使用编辑器按照md的语法规则写文章就可以啦，推荐使用网易云笔记的md实时编辑，同时作为一个“伪前端”，有必要向大家安利下编辑器写代码，自从用了sublime，eclipse再也没打开过，马上就要卸载了。

#### 部署github 
github作为一个强大的代码托管站。。。。不扯淡了，其实我也扯不好。
首先申请个账号，然后新建一个repository，命名为youname.github.io,新建成功之后把url拷贝出来，然后在_config.yml中最下面找到deploy 把你的url替换到repository处。
```
deploy:
  type: git
  repository: https://github.com/return2511/return2511.github.io.git
  branch: master
```
ps：首次我使用的type是github，然后git提示我
```
ERROR Deployer not found : github
```
然后经过度娘，谷歌，使用了type：git
需要在git中执行
```
npm install hexo-deployer-git --save
```
然后在git中执行
```
hexo generate
hexo deploy
```
即可将本地的项目部署到github上。访问路径为 https://youname.gihub.io
其实在部署中还涉及到提交代码的时候让输入用户名密码，网上有教程可以设置ssh至github，确保不用每次提交都输入密码。
### 结束
至此一个简单的hexo 算是搭建完成了，可是我花了近两个小时才搞定，深深的挫败感。
PS：付一些hexo的命令简写，
```
$ hexo g #完整命令为hexo generate,用于生成静态文件
$ hexo s #完整命令为hexo server,用于启动服务器，主要用来本地预览
$ hexo d #完整命令为hexo deploy,用于将本地文件发布到github上
$ hexo n #完整命令为hexo new,用于新建一篇文章
```
最后，原生的hexo主题可能不符合口味，那么就去github上面换一些符合口味的主题吧，好多口味可供选择~~~~

end~~
