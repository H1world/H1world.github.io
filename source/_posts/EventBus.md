---
title: 发布订阅
categories: 随笔
tags: [Js,算法]
---
实现eventBus
<!--more-->
```js
class EventBus {
  constructor() {
    this.list = new Map()
  }

  on(name, fn) {
    let haves = this.list.get(name)
    if(!haves) {
      this.list.set(name, [fn])
    } else {
      haves.push(fn)
    }
  }

  off(name, fn) {
    let haves = this.list.get(name)
    haves.splice(haves.findIndex(e => e === fn), 1)
  }

  emit(name, ...args) {
    let haves = this.list.get(name)
    for(let i in haves) {
      haves[i].apply(this, args)
    }
  }

  once(name, fn) {
    let _this = this
    function haves() {
      _this.off(name, haves)
      fn.apply(null, arguments)
    }
    this.on(name, haves)
  }
}
// test
let demo1 = (...obj) => {
    console.log(...obj)
}

let demo2 = (...obj) => {
    console.log(...obj)
}
let demo = new EventBus()
demo.on('demo1', demo1)
demo.emit('demo1', '第一次')
demo.off('demo1', demo1)
demo.emit('demo1', '第二次')

demo.once('demo2', demo2)
demo.emit('demo2', '第一次', 111)
demo.emit('demo2', '第二次', 2222)
```