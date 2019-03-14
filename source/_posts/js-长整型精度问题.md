---
title: js 长整型精度问题
date: 2019-03-14 11:52:59
tags:
---
  js 长整型精度解决方案
<!--more-->
### JS 长整型精度问题
JS接收长整型的值大于15位之后，会出现混乱问题。通常不会遇到这种问题，除非后台搞事情。
#### 解决方案（比较消耗性能）：通过正则对response中大于15位数字做处理。
```JS
function langForString(data) { // lang类型转换string
  return JSON.parse(data.replace(/(\\?["])[ ]*[:][ ]*(\d{15,})/g, '$1:$1$2$1'))
}
```
以fetch为例
```Js
// 代码片段
  const responseData =  fetch(url, initObj).then((data)=>{
    return data.text()
  }).then((dataStr)=>{
    console.log(langForString(dataStr)) // 最终json数据
  })
```
