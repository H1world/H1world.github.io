---
title: JavaScript片段
date: 2019-01-30 10:05:23
tags: [前端,Js]
categories: 随笔
---
  有意思的JavaScript
<!--more-->
### 片段一：unary
控制台执行以下Js代码：
```Js
const arr = ["1","2","3"];
arr.map(parseInt);   //[1,NaN,NaN]
```
下面讲讲为什么返回的不是[1,2,3]
细化以下以上代码
```Js
arr.map((value,index,array)=>parseInt(value,index));
```
#### map函数传递参数的定义
```Js
arr.map(callback[,thisArg]);
// map参数
callback
原数组中的元素经过该方法后返回一个新的元素。
currentValue 
     callback的第一个参数，数组中当前被传递的元素。
index
     callback的第二个参数，数组中当前被传递的元素的索引。
array
     callback的第三个参数，调用 map 方法的数组。
thisArg可选.  callback函数里的this值 默认是window对象
```
#### parseInt函数定义
```Js
parseInt(string, radix);
// parseInt参数
string: 需要转化的字符，如果不是字符串会被转换，忽视空格符。
radix：数字2-36之前的整型。默认使用10，表示十进制。
需要注意的是，如果radix在2-36之外会返回NaN。而且字符串string中的数字不能大于radix才能正确返回数字结果值。
```
因为parseInt函数接收2个参数，string，radix，如果map函数返回了这两个参数，不管意义是否是所期望的，都会将其作为参数执行。map的回调函数的参数index索引值作了parseInt的基数radix，导致出现超范围的radix赋值和不合法的进制解析，才会返回NaN。
#### 解决技巧
创建一个只接受一个参数的函数，忽略任何其他参数。
调用提供的函数fn，只传入一个参数。
```Js
const unary = fn => val => fn(val);
// EXAMPLES
['1', '2', '3'].map(unary(parseInt)); // [1, 2, 3]
```
### 小算法
#### 数组内相同属性重新集合
概述：[
    { num: "110", name: "二" },
    { num: "122", name: "三" },
    { num: "113", name: "四" },
    { num: "122", name: "五" },
    { num: "110", name: "六" },
] => [
  [
    { num: "110", name: "二" },
    { num: "110", name: "六" },
  ],
  [
    { num: "122", name: "三" },
    { num: "122", name: "五" },
  ],
  [
    { num: "113", name: "四" },
  ],
]
```js
function fun(arr){
  var transfer = [];
  for (var d = 0; d < arr.length; d++) {
    transfer.push(arr[d].num);
  }
  var newTransfer = [...new Set(transfer)];
  var isOver = new Array();
  for(var i = 0; i < newTransfer.length; i++){
    isOver[i] = new Array;
    for(var j = 0; j < arr.length; j++){
      if(newTransfer[i] == arr[j].domainName){
        isOver[i].push(arr[j])
      }
    }
  }
  return isOver
}
```