---

title: This is my first blog — — hexo
categories: 随笔
tags: [hexo,常见问题]
---
  搭建Hexo时的常见问题.
<!--more-->
测试>
  报错: $ hexo d
        ERROR Deployer not found: Git
  解决方法：npm install --save hexo-deployer-git
  hexo常用命令：
  启动本地服务`hexo server` => `hexo s`;
  生成静态文件`hexo generate` => `hexo g`;
  同步到github`hexo deploy` => `hexo d`;
