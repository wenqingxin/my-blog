---
title: 浏览器js运行机制
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
1. 宏任务队列（macro task 每个异步模块任务源都有对应的一个异步队列）
任务源：script（全局任务）, （setTimeout, setInterval, setImmediate）, I/O, UI rendering.
队列执行规则：
 相同队列中的任务按照先进先出的顺序, 不同的队列按照提前设置的队列优先级来调用. 例如，用户代理可以有一个用于鼠标和键盘事件的任务队列（用户交互任务源），另一个用于其他任务。然后，用户代理75%概率调用键盘和鼠标事件任务队列，25%调用其他队列, 这样的话就保持界面响应而且不会饿死其他任务队列. 但是相同队列中的任务要按照先进先出的顺序。也就是说单独的任务队列中的任务总是按先进先出的顺序执行，但是不保证多个任务队列中的任务优先级，具体实现可能会交叉执行
2. 微任务队列（micro task 只有一个）
任务源： Promise, Object.observer, MutationObserver.

## 四、事件循环
1. 执行完主执行线程中的任务。
2. 取出Microtask Queue中任务执行直到清空。
3. 取出Macrotask Queue中一个任务执行。
4. 取出Microtask Queue中任务执行直到清空。
5. 重复3和4。
比如下面的例子
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