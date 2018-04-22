---
title: 离线应用与客户端存储
date: 2018-3-31 00:01:00
tags: [js,offline]
categories: js
---

> js红宝书第23章--离线应用与客户端存储的读书分享，在此做一个记录。

<!-- more -->

## web应用和传统客户端的比较

+ web应用 

    - 无需安装程序，通过浏览器访问
    - 消耗很小的用户硬盘空间或者无
    - update容易，用户端不需要更新
    - 基于浏览器，所以是跨平台的 
<br>

+ 传统客户端 
    - 减轻服务端负担
    - 客户端存储部分资源，节省流量
    - 安全性比web应用高
    - 用户体验好

总体来说web程序更灵活，更方便，更易用，
在网络带宽及速度不断提高的情况下，web应用则越来越流行

## 开发离线应用

- 首先确定设备是否能上网（离线检测） {:&.rollIn}
- 其次应用必须能访问一定资源（使用离线缓存）
- 最后有本地空间用于保存数据（浏览器保存数据）

## 离线检测

使用h5属性：navigator.onLine
- true表示可以上网
- false表示不可上网

通常使用

```
if(navigator.onLine) {
    //正常工作
} else {
    //执行离线状态
}
```

 

## 离线检测 h5事件


 HTML5定义两个事件：online和offline，当网络状态变化时，分别触发这两个事件：
 
```
EventUtil.addHandler(window, 'online', function () {
    console.log('online');
});
EventUtil.addHandler(window, 'offline', function () {
    console.log('offline');
});
```
 

## 应用缓存


HTML5 的应用缓存（application cache），简称 appcache，是专门为开发离线 Web 应用而设计的。


## 描述文件以及使用

该文件用于描述需要下在和缓存的资源，其文件后缀为 manifest，不过现在推荐用 appcache。在文件中可以通过描述指定资源缓存的规则，下面是一个简单的示例：
```
CACHE MANIFEST
# Comment

file.js
file.css
```
1: 使用
```
<html manifest="/filename.manifest" >
```
 

## applicationCache 对象属性

- 属性：通过 applicationCache.status 可以知道缓存的状态，该属性返回一个常量

状态码 | 意义
---|---
0 | 无缓存
1 | 闲置，即应用缓存未得到更新
2 | 检测中，即正在下载描述文件并检测更新
3 | 下载中，即应用缓存正在下载描述文件中指定的资源
4 | 更新完成，通过 swapCache() 来使用
5 | 废弃，即应用缓存描述文件已经不在，因此页面无法再访问应用缓存

 

## applicationCache 对象事件
事件名 | 意义
---|---
checking | 浏览器为应用缓存检测更新时触发
error | 检测更新或下载出错时触发
noupdate | 检测描述文件发现文件无更新时触发
downloading | 开始下载资源时触发
progress | 下载期间继续不断触发
updateready | 下载完成且可通过 swapCache() 使用是触发
cached | 在应用缓存完整可用是触发

 

## applicationCache 对象方法
方法名 | 意义
---|---
swapCache() | 启用新缓存
update() | 检查描述文件是否更新，如果更新继续执行后续操作
 

## Cookie（HTTP Cookie） 

- 最初是用于存储会话信息。该标准要求服务器对任意HTTP请求发送Set-Cookie HTTP头作为相应的一部分，其中包括会话信息。

```
HTTP/1.1 200 ok
Content-type: text/html
Set-Cookie: name=value
Other-header: other-header-value
```
- 其中name和value在传送时都必须是URL编码的。


## Cookie 限制

- cookie在性质上是绑定在特定的域名下的。 {:&.rollIn}
- 当设定了一个cookie后，再给创建它的域名发送请求时，request header会包含所有cookie。
- 这个限制确保了存储在cookie中的信息只能让批准的接受者访问，而无法被其他域访问
- cookie在客户端计算机大小和数量都有限制 根据浏览器不同单个域下最多20-50个 整个cookie长度限制在2095b


## Cookie 结构

+ 名称name： 唯一确定一个cookie必须经过url编码 {:&.rollIn}
+ 值value： cookie中存储的字符串也必须经过url编码
+ 域domain： 在哪个域下是有效的，默认是设置cookie的域
+ 路径path： 可以指定域中的路径
+ 失效时间expire： Cookie何时被删除的时间戳
+ 安全标志secure： 如果设置必须用SSL连接才能发送到服务器



## Cookie 结构

- secure是一个只包含secure的单词,唯一一个非名值对的部分。
- 另外只有名值对儿才会被发送到服务器，其他参数都是服务器给浏览器的指示。
```
HTTP/1.1 200 ok
Content-type: text/html
Set-Cookie: name=value; domain=.ctrip.com; path=/; secure
Other-header: other-header-value
```

 
## javascript中的Cookie


- js中使用用BOM的document.cookie接口操作cookie

```
    //返回所有cookie字符串 ;需要进一步分割
    consile.log(document.cookie);//"a=b;c=d"

    //并不会覆盖原来的cookie 除非名称已经存在
    document.cookie="key=value";
```
- 因其读写操作比较麻烦，还需要进行urlencode，封装了CookieUtil对象来处理cookie的读、写、删除操作
 
## JavaScript Cookie操作扩展 
```
var CookieUtil = {
    get: function(name) {
        var cookie = document.cookie;
        var cookieName = encodeURIComponent(name) + "=";
        var start = cookie.indexOf(cookieName);
        var value = null;
        if (start > -1)
        {
            var end = cookie.indexOf(";", start);
            if (end == -1)
                end = cookie.length;
            value = decodeURIComponent(cookie.substring(start + cookieName.length, end));
        }
        return value;
    },
    set: function(name, value, expires, path, domain, secure) {
        var cookieText = encodeURIComponent(name) + "=" + encodeURIComponent(value);
        if (expires instanceof Date)
            cookieText += "; expires=" + expires.toGMTString();
        if (path)
            cookieText += "; path=" + path;
        if (domain)
            cookieText += "; domain=" + domain;
        if (secure)
            cookieText += "; secure";
            
        document.cookie = cookieText;
    },
    unset: function(name, path, domain, secure) {
        this.set(name, "", new Date(0), path, domain, secure);
    }
};
```
 
## 子cookie


- 为了绕开浏览器对单域名下面的cookie的个数限制，使用子cookie

```
   key=subKey1=subValue1&subKey2=subValue2;
```
- 其原理是在value中包含子cookie；但是虽然绕过了数量的限制另一方面却增加了cookie的长度。
- special tip : 因为cookie是不安全的，所以用户敏感数据不要存在cookie中。

 
## IE用户数据

- 在IE5中，微软通过自定义行为引入了持久化用户数据的概念。允许每个文档最多128K，每个域名最多1MB的数据。
- 首先在某个元素上指定userData行为

```
<div style="behavior:url(#default#userData)" id="dataStore"></div>
```

 
## IE用户数据的操作

- 写入数据
```
var dataStore = document.getElementById("dataStore");
dataStore.setAttribute("name","ctrip");
dataStore.setAttribute("book", "professional javascript");
dataStore.save("BookInfo");
```
- 读取数据
```
dataStore.load("BookInfo");
alert(dataStore.getAttribute("name"));
alert(dataStore.getAttribute("book"));
```
- 删除数据
```
dataStore.removeAttribute("name");
dataStore.removeAttribute("book");
dataStore.save("BookInfo");
```

 
## IE用户数据小结

- 限制是和cookie是类似的，因为其必须需要同一个域名，同一个路径，相同的脚本协议才可以访问。
- 都是非安全的，因此都不可以存放敏感数据。
- 不同点是用户数据是跨会话持久存在的，不会过期，需要手动释放。

 
## Web存储机制
- Web Storage 的目的是克服由 cookie 带来的一些限制。 {:&.rollIn}
- Web Storage可以保证数据被严格控制在客户端上，也无须持续地将数据发回服务器。
- Web Storage 的两个主要目标是: 
  + 提供一种在 cookie 之外存储会话数据的途径； 
  + 提供一种存储大量可以跨会话存在的数据的机制。

- 最初的 Web Storage 规范包含了两种对象的定义：sessionStorage 和 globalStorage。（以windows对象形式存在） 
- 支持这两个属性的浏览器包括 IE8+、 Firefox 3.5+、Chrome 4+和 Opera 10.5+。

 
## Web存储机制   storage类型

- Storage类型提供最大的存储空间（因浏览器而异）来存储名值对
- 只能存储字符串。

  1. clear()：删除所有值 {:&.rollIn}
  2. getItem(name)：根据指定的名称name获取对应的值
  3. removeItem(name)：删除由name指定的名值对
  4. setItem(name, value)：为指定的name设置一个对应的值
  5. key(index)：获得index处的值

 
## Web存储机制   sessionStorage对象

- sessionStorage对象存储是基于会话存在的，如果浏览器关闭，sesssionStorage将会消失。（刷新不会消失）
- 部分浏览器支持崩溃重启依然可用（firefox和webkit支持，IE不支持）


 
## Web存储机制   sessionStorage对象

- sessionStorage对象本质是storage的实例。
- 有两种存储数据的方法

```
sessionStorage.setItem("name","ctrip"); //使用方法
sessionStorage.book("javascripe") ;  // 使用属性
```

- 类似的，获取、删除也有两种方法

```
//get 
var name = sessionStorage.getItem("name");  //使用方法
var book = sessionStorage.book;   // 使用属性

//delete 
sessionStorage.removeItem("name"); //使用方法
delete sessionStorage("book");  //使用delete（在webkit中无效）
```

 
## Web存储机制   sessionStorage对象  IE8写入问题

- 不同浏览器写入数据略有不同，firefox和webkit实现同步写入。
- IE写入数据是异步的，所以设置数据和实际写入有延迟。

```
//只适用于IE8  强制写入磁盘
sessionStorage.begin();
sessionStorage.name = "ctrip";
sessionStorage.commit();
```

- sessionStorage 对象主要是用于针对会话存储，如果需要跨越会话存储，还需要使用globalStorage和localStorage。

 
## Web存储机制   globalStorage对象

- firefox2中对此进行了实现。
- 使用方法

```
globalStorage["www.ctrip.com"].name="ctrip" ;
```

- 需要指明可以访问的域名，使用方法很不友好。
- 支持定义域名为空（但不建议使用，存在安全问题），所以需要指定域名。

 
## Web存储机制   localStorage对象

- 在html5规范中，针对持久化保存客户端数据的方案，localStorage用来替代globalStorage。 
- 因为localStorage对象也是Storage的实例。所以使用方法同sessionStorage，在此不再赘述。

---

- 关于浏览器对localStorage和sessionStorage的支持情况

特性 | Chrome  | Firefox (Gecko)| Internet Explorer | Opera | Safari (WebKit)
---|---|---|---|---|--- 
localStorage   |4 | 3.5| 8| 10.50 | 4
sessionStorage | 5| 2  | 8| 10.50 | 4

 
## Web存储机制  三者的异同


特性 | Cookie | localStorage | sessionStorage
---|---|---|---
数据的生命期  | 可设置失效时间，默认是关闭浏览器后失效  | 除非被清除，否则永久保存  | 仅在当前会话下有效，关闭页面或浏览器后被清除
存放数据大小  | 4K左右  | 一般为5MB  | 一般为5MB
与服务器端通信  | 每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题  | 仅在客户端（即浏览器）中保存，不参与和服务器的通信  | 仅在客户端（即浏览器）中保存，不参与和服务器的通信
易用性  | 需要程序员自己封装，源生的Cookie接口不友好  | 源生接口可以接受，亦可再次封装来对Object和Array有更好的支持  | 源生接口可以接受，亦可再次封装来对Object和Array有更好的支持

 
## IndexDB

- IndexedDB（对象数据库）可以说是浏览器端数据库，可以被网页脚本程序创建和操作。允许储存大量数据，提供查找接口，建立索引等。

![indexedDB存储结构](/7.10.4.jpg "indexedDB存储结构")

- IndexedDB最大的特色是：使用对象保存数据，而不是使用表来保存数据。一个IndexedDB数据库，就是一组位于相同命名空间下的对象的集合。

 
## IndexDB 操作

- 创建数据库  {:&.rollIn}
- 对象存储空间
- 事物
- 游标
- 键范围
- 索引

 
## IndexDB 安全性限制和使用场景
- IndexedDB 使用同源原则，这意味着它把存储空间绑定到了创建它的站点的源（典型情况下，就是站点的域或是子域），所以它不能被任何其他源访问。   {:&.rollIn}
- 要着重指出的一点是，IndexedDB 不适用于从另一个站点加载进框架的内容 (不管是用 frame 还是 iframe)。
- 由于前端数据库需求较少，并且IndexDB对IOS和早期的android支持很不友好，因此使用场景较少。

 
## 小结

- 离线检测   {:&.rollIn}
    + navigator.onLine
- 离线缓存
    + 使用描述文件做appcache
- 浏览器保存数据
    + Cookie
    + IE 用户数据
    + stroge
        + sessionStorge
        + globalStorage(localStorage)
    + IndexDB
- 虽然有了这些技术，我们可以在浏览器上存储大量数据。但是有一点仍需要注意的就是，客户端存在安全隐患，不应该存储隐私敏感数据。







