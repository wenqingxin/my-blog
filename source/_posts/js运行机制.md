---
title: js运行机制
date: 2018-03-10 15:54:36
tags:
---
大家都知道js引擎是单线程的，那它是怎么实现单线程的呢？那就要提到事件循环了，在讲事件循环之前我们首先得搞清楚一下概念。
## 一、函数栈
栈是一种数据结构，特点是数据先进后出，js在执行函数过程中其实也是将函数放进栈结构中，我们成为函数栈，函数执行完成后就拿出函数，当函数内部继续在调用其他函数的时候，我们也继续压入函数。现在，我们写了下面一段代码
```javascript
function bar() {
    console.log(1);
}

function foo() {
    console.log(2);
    far();
}

setTimeout(() => {
    console.log(3)
});

foo();
```
<!-- more -->
这段代码的出栈入栈过程如下
{% asset_img 出栈入栈过程.png 出栈入栈过程 %}

## 二、异步处理模块
浏览器的许多异步功能的实现，并不是js引擎完成的，而是依赖与其他异步处理模块。通常有以下几个
1. 定时器
2. 网络通信
3. dom事件
4. promise
5. I/O
6. UI渲染

## 三、任务队列
任务队列存储回掉的函数，调用异步模块，异步模块完成执行达到回调函数触发条件的时候，会讲回调处理放到任务队列。
任务队列分为两类
1. 宏任务队列（macro task 每个异步模块都有对应的一个异步队列）
2. 微任务队列（micro task 只有一个）
那么哪个任务会被分配到哪个队列呢？
宏任务：script（全局任务）, setTimeout, setInterval, setImmediate, I/O, UI rendering.
微任务：process.nextTick, Promise, Object.observer, MutationObserver.

## 四、事件循环
当函数栈里已经没有函数了，会在任务队列里取任务来执行，执行完毕后，发现没有函数了，又会在队列里拿出一个任务来执行。
宏任务与微任务的执行顺序
当一个宏任务队列执行完毕后就会执行微任务队列，比如下面的例子
```javascript
console.log('script start');
setTimeout(function() {
    console.log('setTimeout');
});
Promise.resolve().then(function(){
    console.log('promise 1');
});
Promise.resolve().then(function(){
    console.log('promise 2');
});
console.log('script end');
```
结果是：
script start
script end
promise 1
promise 2
setTimeout

## 五、总结
{%asset_img 栈、任务队列、事件循环.png 概览%}