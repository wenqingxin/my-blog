---
title: DOM事件总结
date: 2018-03-02 17:38:08
tags:
---

## 一、DOM事件级别
1. DOM0标准
直接元素的属性或者标签上添加on...
Element.onclick = function(){}
2. DOM2标准
增加addEventListener方法，并且可以确定是在捕获或冒泡时执行回调
Element.addEventListener('click', function(){}, false)
3. DOM3标准
在DOM2上增加了一些事件类型，比如键盘事件、鼠标事件之类
Element.addEventListener('keyup', function(){}, false)
注：DOM1中没有事件相关的东西，所以没举例出来
<!-- more -->

## 二、DOM事件模型
捕获 从上往下
冒泡 从目标元素到上

## 三、DOM事件流
用户与浏览器的交互，当用户触发一个操作时，事件经过的流程。
完整的事件流分三个阶段
1. 捕获
2. 目标阶段
3. 冒泡阶段
{% asset_img 事件流.png 事件流向%}

## 四、event对象的应用
event.preventDefault()
event.stopPropation()
event.stopImmediatePropagation() //除了阻止冒泡外还阻止相同元素相同事件的其他绑定
event.currentTarget //当前绑定事件的元素
event.target //触发事件的元素

## 五、自定义事件
通过Event对象进行创建自定义事件
```javascript
var element = document.body;
var evt = new Event('custom',{
    bubbles: true,// 是否冒泡
    cancelable: true// 是否可取消
});
element.addEventListener('custom', function(e) {
    console.log('我被触发了');
    console.log(e.bubbles);
    console.log(e.cancelable);
});
element.dispatchEvent(evt);// 我被触发了
```
另外，CustomEvent也可自定义事件，不通的是在创建对象的时候可以多传一个detail字段来传递数据