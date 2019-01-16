---
title: Vue手动清空keepalive
date: 2019-01-16 10:55:48
tags: [前端,Vue]
categories: 随笔
---
  Vue如何手动清空keepAlive.
<!--more-->
### 运用场景
入职新公司，领导安排一个项目：重构控制台。
大致需求如下：此控制台的一切配置，全部通过后台传入的json数据类型控制。譬如身份验证、导航栏、主导航栏、页面组件、小组件、大组件...
### 技术选型
在确认需求之后，想一下需要做哪些事。首先我需要实现将所有能运用到的前端应用、组件封装好。然后通过一套规则，读取后台传入的配置：路由、身份、组件、接口等等。
Vue很符合这个需求，首先Vue提供addRouter可以用来动态生成路由、其次keepalive可以模拟传统的iframe，并且Vue非常轻便，相比react所有事情都需要手写，工作量大大缩短。
### 实现思路
1、封装组件：将小组件譬如input、button、table、select等经常会在控制台运用到的。
2、使用缓存组件模拟iframe。
3、制定规则，获取自个儿需要的参数，用来生成自己想要的路由或者组件。
4、配置信息告诉路由名、物理路径等信息之后，需要手动创建组件，撰写Vue组件。这一点暂时没想到好的方法代替。
### keepAlive如何手动清空
在Vue官网文档内并未提供手动清空keepalive的方法，我也是网上搜了一些资料，并不能确定这样是真正意义上的从内存中清空。以下贴入清空keepalive关键代码：
```Js
//入口文件 Main.js 通过路由守卫判断。
Vue.mixin({
  beforeRouteLeave: function (to, from, next) { 
    if (from && from.meta.remove) { // remove是用来判断需要清空的路由。
      if (this.$vnode && this.$vnode.data.keepAlive) { // 一层层获取获取keepalive
        if (this.$vnode.parent && this.$vnode.parent.componentInstance && this.$vnode.parent.componentInstance.cache) {
          if (this.$vnode.componentOptions) {
            var key = this.$vnode.key == null
              ? this.$vnode.componentOptions.Ctor.cid + (this.$vnode.componentOptions.tag ? `::${this.$vnode.componentOptions.tag}` : '')
              : this.$vnode.key
            var cache = this.$vnode.parent.componentInstance.cache
            var keys = this.$vnode.parent.componentInstance.keys
            if (cache[key]) {
              if (keys.length) {
                var index = keys.indexOf(key)
                if (index > -1) {
                  keys.splice(index, 1)
                }
              }
              delete cache[key] // 删除内存
            }
            from.meta.remove = false // 这里恢复初始值.
            this.$destroy() // 摧毁组件
          }
        }
      }
    }
    next()
  }
})

```