---
title: promise
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
### promise封装Ajax请求
需了解XMLHttpRequest
```js
function promiseForAjax(methods, path, body, headers) {
  return new Promise((resolve, reject) => {
    let xml = new XMLHttpRequest();
    xml.open(methods, path);
    for(let key in headers) {
      let value = headers[key]
      xml.setRequestHeader(key, value)
    }
    xml.send(body)
    xml.onreadystatechange = () => {
      if(xml.readyState === 4) {
        if(xml.status >= 200 && xml.status <= 400){
          resolve.call(undefined, xml.responseText)
        } else if(xml.status >= 400) {
          reject.call(undefined, xml)
        }
      }
    }
  })
}
```