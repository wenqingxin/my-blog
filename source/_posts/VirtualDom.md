---
title: VirtualDom
date: 2018-03-18 11:18:29
tags:
---
## 一、什么是VirtualDom
虚拟Dom，简称VDom，是用JavaScript对象来模拟真实的Dom对象。例如
```javascript
{
    tag: 'div',
    attrs: {
        style: ''
    },
    children: [
        tag: 'div',
        attrs: {
            style: ''
        },
        children: []
    ]
}
```
## 二、VirtualDom有什么用
真实Dom的操作往往代价非常的高，因为创建一个Dom元素，往往会有几十个属性，同时可能还会触发重绘。用javascript来模拟的Dom对象，是相对来说简单的，同时javascript的操作效率也比真实dom高得多，通过这种VDOM的方式就能提高浏览器渲染的性能。

## 三、Virtual的库
snabbdom库，vue就是基于这个库来实现vdom的。
api主要包括
1. h()，帮助函数(渲染函数)
2. patch(),dom拼接方法

## 四、VirtualDom的原理
1. 通过将vnode放在内存中，只有涉及到真实dom节点时才去实际渲染；
2. 当需要将vnode拼接到真实dom对象的时候，通过diff算法比对出新旧dom对象之间的差异，只渲染有差异的地方，以达到性能最优。

## 五、diff算法
