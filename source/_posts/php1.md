---
title: php排序之array_multisort
date: 2016-07-29 17:40:28
tags: [php排序,array_multisort]
categories: php
---
#### 定义和用法
> by w3school 的解释
>
> array_multisort() 函数返回排序数组。您可以输入一个或多个数组。函数先对第一个数组进行排序，接着是其他数组，如果两个或多个值相同，它将对下一个数组进行排序。
>
> 注释：字符串键名将被保留，但是数字键名将被重新索引，从 0 开始，并以 1 递增。
>
> 注释：您可以在每个数组后设置排序顺序和排序类型参数。如果没有设置，每个数组参数会使用默认值。

看完定义表示一脸懵逼。
继续看W3的语法
#### 语法
```
array_multisort(array1,sorting order,sorting type,array2,array3...)
```

参数 | 描述
---|---
array1 | 	必需。规定数组。
sorting order | 可选。规定排列顺序。可能的值：SORT_ASC - 默认。按升序排列 (A-Z)。SORT_DESC - 按降序排列 (Z-A)。
sorting type | 	可选。规定排序类型。SORT_REGULAR - 默认。把每一项按常规顺序排列（Standard ASCII，不改变类型）。SORT_NUMERIC - 把每一项作为数字来处理。SORT_STRING - 把每一项作为字符串来处理。SORT_LOCALE_STRING - 把每一项作为字符串来处理，基于当前区域设置（可通过 setlocale() 进行更改）。SORT_NATURAL - 把每一项作为字符串来处理，使用类似 natsort() 的自然排序。SORT_FLAG_CASE - 可以结合（按位或）SORT_STRING 或 SORT_NATURAL 对字符串进行排序，不区分大小写。
array2 | 可选， 扩展数组
array3 | 同上

表示继续一脸懵逼

#### coding 
带着懵逼的心情开始coding来尝试。

```
$arr1 = array(2,1,5);
array_multisort($arr1); 
print_r($arr1);
```
输出结果为 Array ( [0] => 1 [1] => 2 [2] => 5 ) ;
继续coding

```
$arr1 = array(2,1,5);
$arr2 = array(6,5,4);
array_multisort($arr1,$arr2); 
print_r($arr1);
print_r($arr2);
```
神奇的输出结果为 1,2,5 和 5,6,4 ;
感觉好像是每一列是一个单元，$arr2是按照arr1的排序key值输出。决定再加一个$arr3来验证下

```
$arr1 = array(2,1,5,2);
$arr2 = array(6,5,4,2);
$arr3 = array(3,4,5,6);
array_multisort($arr1,$arr2,$arr3); 
print_r($arr1);
print_r($arr2);
print_r($arr3);
```
这次输出结果为1,2,2,5  、 5,2,6,4 和 4,6,3,5
仿佛验证了刚刚的感觉，另外还发现了，加入$arr1中有两个变量值一样，就按照$arr2中的值排序，推测以此类推，此处没有进一步加以验证。
* PS 该方法仅返回true or false         ；此外，经验证，该方法中，$arr参数如果有多个，必须保持length一致，否则会报错。

#### 对二维数组排序
在php coding中经常会遇到对二维数组按照某一field排序的场景。我们可以依据上面验证的规则使用array_multisort();方法来对二维数组进行排序。
由于上面的验证的规则是：每次排序，把每一列对应当做一个单元，$arr2排序规则完全依照$arr1排序之后的key。这样我们可以把二维数组的排序字段按照顺序抽出来用来作为排序的规则$arr1 ，原二维数组我们用来作为$arr2，这样就可以根据规则对二维数组排序。
##### 废话少说，show me code

```
$arr = array(  
        array('id'=>4,'exData'=>'a'),  
        array('id'=>3,'exData'=>'b'),  
        array('id'=>7,'exData'=>'c'),
        array('id'=>5,'exData'=>'d')  
    );
$id = array();
foreach($arr as $key => $value){
    $id[$key] = $value['id'];
}       
array_multisort($id, SORT_ASC, $arr);
print_r($arr);
```
输出结果为
Array ( 
    [0] => Array ( [id] => 3 [exData] => b ) 
    [1] => Array ( [id] => 4 [exData] => a ) 
    [2] => Array ( [id] => 5 [exData] => d ) 
    [3] => Array ( [id] => 7 [exData] => c ) 
);
##### 封装方法
这个是网上看到的一个封装好的方法，感觉挺好的。思路也比较清晰。

```
$arr = array(  
        array('id'=>4,'exData'=>'a'),  
        array('id'=>3,'exData'=>'b'),  
        array('id'=>7,'exData'=>'c'),
        array('id'=>5,'exData'=>'d')  
    );
$sort = array(  
        'direction' => 'SORT_ASC', //排序顺序标志 SORT_DESC 降序；SORT_ASC 升序  
        'field'     => 'id',       //排序字段  
		);  
		$arrSort = array();  
		foreach($arr as $uniqid => $row){  
		    foreach($row as $key=>$value){  
		        $arrSort[$key][$uniqid] = $value;  
		    }  
		}  
		if($sort['direction']){  
		    array_multisort($arrSort[$sort['field']], constant($sort['direction']), $arr);  
		} 
```
#### 总结
array_multisort();方法主要在使用进行多个数组排序，以及二维数组排序时使用。
多个数组必须要保持所有数组的length值一样，二维数组需要进一步操作（实质是转化为两个数组进行排序）。
