---
title: 组件化与react
date: 2018-03-19 21:14:30
tags:
---
## 什么是组件
1. 封装
对视图、数据、交互逻辑进行封装
2. 复用
通过传入props，实现功能上的复用
## jsx的本质
创建vdom的语法糖，通过babel编译成js代码
## jsx与vdom
jsx编译后就是用React.createElement()方法创建vdom
## setState
为什么是异步？
当连续重复setState的时候，数据更新没有必要每次都去更新视图，react内部将这部分操作汇总起来，只对最后的数据进行渲染