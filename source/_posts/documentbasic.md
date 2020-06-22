---
title: javascript基础系列之dom
date: 2020-6-23 09:00:00
tags: [js]
categories: js
---

> js基础Dom相关方法和属性知识回顾

<!-- more -->

### 背景

随着前端领域逐渐被 React/Vue 这两个MVVM的框架占领，我们前端开发过程中越来越少的存在去操作dom的场景了。通常情况下出现的比较多的就是用虚拟dom去替换了传统的dom操作。但是我始终认为Dom操作是前端最最基础的一个点，这篇文章来汇总下常见的Dom操作。

### Dom操作分类

常规的来说Dom操作主要分为增删改查以及插入五个类别，下面分别阐述：

#### 新增（创建）Dom

主要分为元素、属性、文本、注释节点的创建

- 元素节点的创建

该方法在我们工作中应该是用的比较多的一个方法了，入参字符串为当前创建的节点的html标签名。

``` javascript
const div = document.createElement('div')
console.log('this is a div', div)
```

- 属性节点的创建

  - 设置Dom元素的基本属性

  ``` javascript
    document.body.id = 'root'
    // or 上面的div
    div.id = 'divid'
    // but class 是js的关键字，这里面类似JSX使用className 代替
    div.className = 'divclass'
  ```

  - 设置自定义属性节点 **createAttribute**

  ``` javascript
    const attr = document.createAttribute('myAttrId');
    attr.value = 'new attr'

    div.setAttributeNode(attr)
  ```

  具体有啥用？ 展开说说，就是说 createAttribute 方法会自定义一个属性，也就是类似我们之前JQ年代的data-xxx 属性一样，上面的代码作用如下

    1. 创建一个attr 名称为 myattrid（html中规定所有的attr均为小写）
    2. 设置该 myattrid 的值为 'new attr' 字符串
    3. 把该属性增加到上面新建的div 上

  三步完成之后，之前新建的div上面就会有一个 叫做myattrid的属性，其值为 'new attr'

- 文本节点的创建

``` javascript
const text = document.createTextNode('this is text');
console.log(text);
typeof text // 'object'
```

创建一个文本节点，可以用来作为html标签内容（ps:可以看到该节点是一个object，应该和普通字符串区分）

- 注释节点的创建

创建一个注释 不再赘述

``` javascript
const comment = document.createComment('这是一个注释');
console.log(comment); // <!--这是一个注释-->
```

#### 删除Dom

主要分为两类

- 删除dom节点

``` javacript
const div = document.getElementById('id');
// 删除body下所有属性为id的div 节点
document.body.removeChild(div);
// or div直接执行 remove，即删除div
div.remove();
```

- 删除dom节点的属性

``` javacript
// 移除dom基本元素
div.removeAttribute('class');
// 移除dom自定义元素
div.removeAttributeNode('myattrid');
```

#### 修改（替换）Dom

``` javascript
const newElement = document.createElement('p');
const span = document.getElementById('span');
//查找
const parent = span.parentNode;
//替换元素
const replace = parent.replaceChild(newElement, span);
```

#### 查找Dom

客观的说，查dom可能才是用的最多的操作。

主要分为五类查找

- 根据id查找

``` javascirpt
const id = document.getElementById('test')
console.log(id) // 返回id为test的标签，仅返回第一个，查询效率高
```

- 根据class查找

``` javascript
var className = document.getElementsByClassName('class');
console.log(className); // 返回class为‘class’的标签集合(HTMLCollection),类数组
```

- 根据标签名查找

``` javascript
var tagName = document.getElementsByTagName('div');
console.log(tagName); // 返回tag为‘div’的标签集合(HTMLCollection),类数组
```

- 根据属性名查找

``` javascript
const attr = document.getElementsByName('attr');
console.log(attr); // 返回属性为attr的标签集合（NodeList），类数组
```

- **css选择器查找**

核心为两个方法 querySelector 和 querySelectorAll（忘记之前在某一篇文章看过一句话，这两个方法是根据jquery实现的，但是正是这两个方法加速了jquery坠落的速度）

``` javascript
document.querySelector('.element')
document.querySelector('#element')
document.querySelector('div')
document.querySelector('[name="username"]')
document.querySelector('div + p > span')
```

不用jquery的情况，用下面一句话可以简单替换基本查找的方法

``` javascript
const $ = document.querySelector.bind(document)
```

#### 插入Dom

可以直接使用appendChild 对已经存在的标签插入一个节点

``` javascript
document.body.appendChild(div)
```

也可以使用 insertAdjacentHTML 来进行插入

``` javascript
document.body.insertAdjacentHTML(
  'beforeend',
  div
)
```

insertAdjacentHTML 主要有四个插入位置

- 'beforebegin': 元素之前
- 'afterbegin': 元素内，位于现存的第一个子元素之前
- 'beforeend': 元素内，位于现存的最后一个子元素之后
- 'afterend': 元素之后

具体如下

``` javascript
<!-- beforebegin -->
<div>
  <!-- afterbegin -->
  <span></span>
  <!-- beforeend -->
</div>
<!-- afterend -->
```

### 总结

这些方法其实就是jquery这个红极一时的js库的核心知识点。所以应该对技术怀有一颗敬畏之心，切不可眼高手低。万丈高楼平地而起，夯实基础很重要。
