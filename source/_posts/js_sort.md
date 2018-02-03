---
title: 基于js的排序问题
date: 2018-1-29
tags: [js,排序]
categories: js
---

> 基于js语言实现几种基本的数据结构排序问题。

<!-- more --> 
#### 背景
前段时间，正式回归到了码农届，工作了一段时间，还是发现自己比较适合coding，做自己喜欢的事和擅长的事，都是幸福的。

言归正传，在面试豚厂的时候，由于面的是纯js。可能在一顿巴拉巴拉的js基础之后，面试官让手写了几段代码。其中竟然有一段是用js写快排，然而最尴尬的事情是好久没写代码了，竟然写错了，还好面试官是一个很nice的人，也是目前带我的师傅。提醒我考虑考虑重新写，所幸第二次还是没有问题写了出来。

一直就想着，找个时间好好的把这些数据结构的知识补充补充。

#### 几种排序方式

##### 冒泡排序
首先，最简单的，冒泡排序。这个排序在我印象中应该是最简单的，也是接触数据结构第一个学习的排序算法。
其原理也很简单，一个数组从第1个值开始，和下一个比较，如果第1个数字大，就和2交换；否则没有操作。这样一次循环之后会把最大的交换到最后一位数字（即找到最大的数字）。然后开始第二轮，找到次大的，放在倒数第二位；以此类推，循环n（n为数组长度），即可得到一组有序数组。<br>
很显然这种排序需要O(n²)的时间复杂度，但还是一个相对稳定的排序，就是讲不管什么样的数组，都需要进行这么多次遍历。
代码如下：

``` javascript
function buddlesort (arr) {
    var len = arr.length ;
    var temp ; 
    for (var i = 0 ; i < len ; i++ ) {
        for(var j = 0 ; j < len - i ; j++ ) {
            if(arr[j] > arr[j+1]) {
                temp = arr[j] ;
                arr[j] = arr[j+1] ;
                arr[j+1] = temp;
            }
        }
    }
}
```
代码比较简单，也没什么好讲的。

##### 选择排序
其实这玩意和冒泡几乎没有什么区别。都是两层循环遍历。唯一的区别就是冒泡每次比较结束如果满足条件都是立即swap一下两个数字。但选择排序是标记最大的数字，然后循环完一轮之后发生一次swap。本质上感觉还是冒泡。

``` javascript
function selectionSort(arr) {
    var len = arr.length;
    var minIndex, temp;
    for (var i = 0; i < len - 1; i++) {
        minIndex = i;
        for (var j = i + 1; j < len; j++) {
            if (arr[j] < arr[minIndex]) {     //寻找最小的数
                minIndex = j;                 //将最小数的索引保存
            }
        }
        temp = arr[i];
        arr[i] = arr[minIndex];
        arr[minIndex] = temp;
    }
    return arr;
}
```
这两种算法在空间上的消耗都不需要额外的空间。在处理数据比较小的排序时，应该是不错的算法。相对也稳定。

##### 插入排序
插入排序，看过一篇文章，说这个东西和打扑克一样。假设有10张牌，你每次哪一张，然后拿在手里的时候手动插好牌，插扑克的过程就是插入排序的过程。

``` javascript 
function insertsort(arr) {
    var len = arr.length ;
    var pre , curr ;
    for (var i = 1 ; i< len ; i++) {    //从第二个数字开始插入
        pre = i - 1 ;                   //设置岗哨 
        curr = arr[i] ;
        while( pre >= 0 && arr[pre] > curr) {
            arr[pre+1] = arr[pre] ;
            pre -- ;
        }
        arr[pre+1] = curr ;
    }
    return arr;
}
```
整体思路就是从后往前比较，如果当前这个和比较的这个岗哨要小一点，那么岗哨的值往后挪一位，空出个位置，直到找到确定该放的位置。
有一种优化的二分插入。
``` javascript 
// 二分插入排序
function binaryInsertSort(arr){
    for (var i = 1; i < arr.length; i++) {
        var key = arr[i], left = 0, right = i - 1;
        while (left <= right) {
            var middle = parseInt((left + right) / 2);
            if (key < arr[middle]) {
                right = middle - 1;
            } else {
                left = middle + 1;
            }
        }
        for (var j = i - 1; j >= left; j--) {
            arr[j + 1] = arr[j];
        }
        arr[left] = key;
    }
    return arr;
}
```
其实思路还是比较清晰的，无非就是基本的插入排序，是从当前已排序的最后一个数字作为基准；而二分插入法，每次取得基准都是中间的数字，虽然最坏时间复杂度没有提升，但平均时间消耗肯定是会减少的。
##### 快速排序
快排，顾名思义可以快速的排序。名字起的就很简单粗暴，传言在处理大数据时有着超强的优势。虽然其worst case 的情况时间复杂度也是O(n²)，但是在大部分情况下，其优秀的时间控制要比O(n log n) 更好。
> 快速排序的最坏运行情况是O(n²)，比如说顺序数列的快排。但它的平摊期望时间是O(n log n) ，且O(n log n)记号中隐含的常数因子很小，比复杂度稳定等于O(n log n)的归并排序要小很多。所以，对绝大多数顺序性较弱的随机数列而言，快速排序总是优于归并排序。

快排的思路也很简单，一共可分三步
1. 选择一个数为基准（一般选择中间数字）
2. 所有小于基准的数字都放在左边，所有大于基准的数字都放在右边
3. 递归分别对左右边继续调用快排，直到数组长度为1

``` javascript 
function quicksort(arr) {
	if( arr.length <= 1 ) {
		return arr ;
	} 
	var flag = Math.floor(arr.length / 2) ;
	var left = [] ,
		right = [] ;
	for (var i = 0; i < arr.length; i++) {
		if ( flag !== i ) {
			if( arr[flag] > arr[i] ) {
				left.push(arr[i]);
			}else {
				right.push(arr[i]);
			}
		}
	}
	return quicksort(left).concat(arr[flag]).concat(quicksort(right));
}
```
以上代码为上次面试手写的代码，今天码了一遍，控制台跑了一下，应该是没有什么问题的
#### 写在最后
写完快排本来想写剩下的堆排序等等，可是忽然感觉自己把那几种算法忘了，待我有时间，多多补充点数据结构知识，然后继续完成下篇排序文章。

少抱怨，多多思考，coding的世界也很美好。