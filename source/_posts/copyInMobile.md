---
title: Js点击复制文本
date: 2019-02-19 15:41:46
tags: [前端,Js]
categories: 随笔
---
  Js适用于移动端的点击复制（Android/IOS）
<!--more-->
```HTML
<div>
  <div id="content">
    <p> copycopycopycopy </p>
  </div>
  <button id="copyBtn">复制</button>
</div>
```
```Js
function copyArticle(event) {
    const range = document.createRange();
    range.selectNode(document.getElementById('content'));
    const selection = window.getSelection();
    if (selection.rangeCount > 0) selection.removeAllRanges();
    selection.addRange(range);
    document.execCommand('copy');
    alert("复制成功！");
}
document.getElementById('copyBT').addEventListener('click', this.copyArticle, false);
```
如果需要隐藏复制内容，添加`display：none;`是获取不到内容的，无法复制，可以通过设置字体颜色透明的方式`color：transparent`，然后添加定位，定位到可视区域之外。