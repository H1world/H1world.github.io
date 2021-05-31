---
title: 简单实现promise.all
categories: 随笔
tags: [Js, 面试]
---
面试记录
<!--more-->
### promise.all
正儿八经需要判断数据类型，这里做简单的处理，只实现promise.all的全部执行完毕才返回，并且其中一个出错则返回失败的处理。
```js
function promiseAll(arr) {
  let len = arr.length, res = [];
  if(len) {
    return new Promise((resolve, rejust) => {
      for(let i = 0; i < len; i++) {
        let promise = arr[i]
        promise.then(response => {
          res[i] = response
          if(res.length === len) { // 当处理到最后一个元素
            resolve(res)
          }
        }, err => {
          rejust(err)
        })
      }
    })
  }
};
```
