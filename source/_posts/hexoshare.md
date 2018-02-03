---
title: hexo经验分享
date: 2018-1-2 
tags: [hexo,next主题]
categories: hexo
---
> 前段时间重新搭建hexo博客，以及blog中添加大量图片的解决方法。

<!-- more -->
##### 背景

由于自己想要写一篇文章，内嵌很多高清图片，在最初动手写之前想过了几个方案。

- 微信公众号
- qq空间
- 通用类博客网站，比如简书

首先，本人不喜欢公众号推文，其次，看了看qq空间的结构之后，感觉这个编辑排版自己不喜欢，给我一种很凹凸的赶脚。然后我准备用markdown来写点东西，自从之前用过了几次，还是被这种简洁风深深的安利到了的。

然后我看了下，简书是支持markdown的，并且简书有很多牛人都有在写博文。但是为啥我感觉简书对一些markdown的语法支持的不是很好呐。当我满怀法克心情的时候，忽然想到了hexo，这个之前自己用它搭建过博客。但是由于之前只是把博客部署到了guthub和coding的pages页面上。但是在自己一不小心玩坏了自己的电脑之后，才发现自己的博客源码并没有传到github上。。。后来慢慢的也就没有再去写博文（原谅我给自己找的理由。。。）

##### 搭建hexo
于是说干就干，就自己动手搭建了hexo博客，并且还用了之前很中意的next主题，知乎上面搜索了下，发现NexT主题貌似是hexo最火的一款主题，哎哎哎，我喜欢自己喜欢的东西永远都火不起来，自己喜欢就好。就像很多时候我在网易云听一首歌感觉很好听，就希望这首歌永远都火不起来，永远也不要看到评论区是999+（喂，大哥，你又跑题了）

[搭建hexo的步骤请移步之前的一篇博文](http://our62.top/2016/07/28/hexo-create/#more)（你丫不是说之前的东西丢了么，好吧，我的源码丢了，但是我的文章还在呀，阿哒！因为有有道云笔记备份了文章）

##### 着手写博文
满心欢喜，这下终于可以愉快的写博文了。转念一想，图片咋办？一张张高清超过10M的图片我可不敢之前静态资源存储，所以必须找个图床。于是又想到之前的博客上用过骑牛去测试了图片加载速度（~~好像记得我在某篇文章上允诺的要写一个相册出来~~）啊，你说啥，我看不到。<br>
然后就把我所有的图片上传到七牛上了，用外链地址插入图片至博文中，比如：
![image](http://ocdgyglqe.bkt.clouddn.com/6L2A0219.jpg)
就可以看到帅气如我的图片了。<br>
氮素，这个是有bug的，当我写完上篇博文的时候，插了大约十几张图片的一篇博文，我发现部署之后手机访问特别慢，图片也没有按照我预想的很快加载出现，网页也有明显卡顿。打开chrome调试，发现原来虽然经过七牛作为图床之后，如果直接链接图片地址，虽然经过七牛的cdn加速，但是还是把整个图片都down了下来，这是不合理的，因为我一个页面插入十几个高清的图片，这样整篇文章需要加载近200M的图片，如果分享到朋友圈，对于一些本来流量就不多的朋友来说，着实是一种残忍。
经过查阅相关文档，发现是可以压缩的，再次满怀欣喜

```
* 原图
![image](http://ocdgyglqe.bkt.clouddn.com/6L2A0219.jpg)
*压缩图片
![image](http://ocdgyglqe.bkt.clouddn.com/6L2A0219.jpg?imageMogr2/quality/50)
```
详情请参见[七牛图片高级处理](https://developer.qiniu.com/dora/manual/1270/the-advanced-treatment-of-images-imagemogr2)，这里我设置了50的quality，整篇文章大约精简至20M+的流量，对于现在大部分动辄几个G的流量赠送，这点也不算什么了，主要是4G加持的加载速度，打开页面还是很快的。

##### 主题设置问题
上文已经说到了，我再次选用了next主题，但是在我搜索各个文章介绍next美化的时候看到一些很炫酷的场景，比如页面背景的canvas动画，这里面说到了这个炫酷的东西，好像在我第一次玩next主题的时候它还是不存在的。但是最新版的theme已经自带这个功能了，好评，好评<br>
我知道要show you code ，打开theme 下的配置文件_config.yml 文件

```
# Canvas-nest
canvas_nest: true

# three_waves
three_waves: false

# canvas_lines
canvas_lines: false

# canvas_sphere
canvas_sphere: false
```
这里面有几种动画背景，可以一一尝试，发现还是喜欢第一款。
另外有一些比如说点击出现♥形图案的修改、添加浏览访问次数啦等等，请移步[网上搜索的这篇博文](http://blog.csdn.net/qq_33699981/article/details/72716951)，感觉写的还是蛮详细的。

##### 写在最后
即便coding作为一个兴趣爱好，也要好好的珍视它！<br>
 > ### You got a dream, you gotta protect it. <br>
 > ### People can't do something themselves,they wanna tell you you can't do it.<br>
 > ### If you want something, go get it.


