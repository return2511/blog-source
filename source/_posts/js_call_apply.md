---
title: 基于js二维数组降维的一点思考
date: 2018-1-7 
tags: [js,js数组,apply]
categories: js
---

> 在研究js数组降维的时候，遇到的apply方法的巧用，深深感触，如果每次都多想一点，多尝试一点，一定可以写出更合理的代码。

<!-- more -->

#### 背景
最近准备重新拾起js，励志做一个合格的FE开发。于是买了几本书（装作好像是认真的样子），其中自己比较中意的有两本，一本是node的深入浅出，一本是js的蝴蝶书。认真翻阅书的时候，才会发现以前自己代码中遇到的错误，以及以前“不小心”解决的问题，但不知道原理是什么，现在搞懂了一点。感觉很有成就感，最近几日遇到的问题，想着一并记下来。也许在未来的某一天，我会翻阅到这篇文章，发现其中的错误并改正，但我觉得能发现问题就是好事。<br>

#### js降维问题
前几天看到一个js问题--- 二维数组降维的问题。第一眼看到这个问题，我X，这特喵的不是侮辱智商么。当即控制台调出来。
##### 二次遍历循环降维

```js
var myArr = [
    ['h', 'e', 'l', 'l', '0'],
    ['w', 'o', 'r', 'l', 'd'],
    ['!'],
    ['I', 'l', 'o', 'v', 'e'],
    ['j', 's', '!']
];
function reduceArr ( arr ) {
    var result =[] ;
    for (var i = 0 ; i < arr.length; i++) {
        for(var j = 0; j < arr[i].length; j++){
            result.push(arr[i][j]);
        }
    }
    return result;
}
console.log(reduceArr(myArr)); 
```
##### 使用cancat方法降维
然而狗哥细细一想，事情可能并没有那么简单，怎么说自己也要做一个有节操的jser，这段代码写得丝毫没有技术含量。于是狗哥沉思一段时间，想到了concat方法。
```js
var myArr = [
    ['h', 'e', 'l', 'l', '0'],
    ['w', 'o', 'r', 'l', 'd'],
    ['!'],
    ['I', 'l', 'o', 'v', 'e'],
    ['j', 's', '!']
];
function reduceArr ( arr ) {
    var result =[] ;
    for (var i = 0 ; i < arr.length; i++) {
        result = result.concat(arr[i]);
    }
    return result;
}
console.log(reduceArr(myArr)); 
```
使用concat方法可以减少一次内部循环，看起来好很多了。暂时告了一个段落。
##### apply方法的使用
可是在我浏览论坛的时候，看到这个面试题，我顺手看了答案。网上普遍认为这个答案比较好。
```js
var myArr = [
    ['h', 'e', 'l', 'l', '0'],
    ['w', 'o', 'r', 'l', 'd'],
    ['!'],
    ['I', 'l', 'o', 'v', 'e'],
    ['j', 's', '!']
];
function reduceArr ( arr ) {
    return Array.prototype.concat.apply([], arr);
}
console.log(reduceArr(myArr)); 
```
其实内心是一脸懵逼状态，apply还有这种打开方式？？<br>
#### 再学习call和apply方法
以前仅仅知道call和apply方法是为了动态改变this指向而存在的。
>call 和 apply 都是为了改变某个函数运行时的 context 即上下文而存在的，换句话说，就是为了改变函数体内部 this 的指向。因为 JavaScript 的函数存在「定义时上下文」和「运行时上下文」以及「上下文是可以改变的」这样的概念。

可是并不知道还有这种打开方式。顺手贴一段call和apply方法的基本用法。

```js
var func = function(arg1, arg2) {};
```
可以用如下方式调用

```js
func.call(this, arg1, arg2);
//or
func.apply(this, [arg1, arg2]);
```
其中 this 是你想指定的上下文，他可以任何一个 JavaScript 对象(JavaScript 中一切皆对象)，call 需要把参数按顺序传递进去，而 apply 则是把参数放在数组里。

另外，在js中，如果函数的参数个数是不固定的，因此要说适用条件的话，当你的参数是明确知道数量时，用 call，而不确定的时候，用 apply，然后把参数 push 进数组传递进去。当参数数量不确定时，函数内部也可以通过 arguments 这个数组来便利所有的参数。<br>
那么apply用来降维数组究竟为何故呢？
#### apply的灵活应用
apply调用的时候，第一个参数是对象，第二个参数是一个数组集合，但是在apply执行时，会把数组解析为一个个参数列表。比如(['a','b','c']转换为 a,b,c ) ,这样就有点厉害了，渐渐的好像也懂了上面那段代码的意图了。
比如说

```js
var arr = ['1', '2', '3', '4', '5'];
var max = Math.max.apply(null,arr);
console.log(max);
``` 
再比如

```js
var arr1 = ['4', '5', '6'];
var arr2 = ['1', '2', '3'];
Array.prototype.push.apply(arr1,arr2);   
console.log(arr1); //["4", "5", "6", "1", "2", "3"]
```
push不可以接受数组，但是可以接受参数列表。所以再看这段代码
```js
var myArr = [
    ['h', 'e', 'l', 'l', '0'],
    ['w', 'o', 'r', 'l', 'd'],
    ['!'],
    ['I', 'l', 'o', 'v', 'e'],
    ['j', 's', '!']
];
function reduceArr ( arr ) {
    return Array.prototype.concat.apply([], arr);
}
console.log(reduceArr(myArr)); 
```
apply把二维数组降为一维的参数列表。然后使用cancat拼接到一个数组内，从而完成了降维。这个方法姑且不谈时间复杂度，但是能把最初的最low的方法优化到一行代码还是很开心的。
#### 总结
js，对之一直怀着一颗敬畏之心，越学习，越感觉自己的无知，但同时也想认真的去探索其中的奥秘。同时coding也要多多执着，不能将就。

