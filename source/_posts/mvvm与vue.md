---
title: mvvm与vue
date: 2018-03-18 14:40:44
tags:
---
## 一、mvvm是什么
以后端mvc为原型，将设计模式移植到前端来的一种设计模式，称为mvvm（Model View ViewModel）。

## 二、vue三要素
1. 响应式
修改data属性后，vue立刻监听到
data属性被代理到vm上
2. 模板解析
模板本质就是一段字符串，最终将转换成逻辑的js代码，render函数。
3. 渲染
vue的render函数中用来with（可能带来歧义，降低代码的可阅读性），返回vnode

## 三、vue的整个实现流程
1. 初始化，钩子函数，事件初始化，响应式开始监听
2. 模板解析，生成render函数(_C返回vode)
3. 首次渲染，显示页面，收集依赖
4. data属性变化，触发update

## 四、传统jquery与vue的区别
1. 数据和视图分离
2. 数据驱动视图
