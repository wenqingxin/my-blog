---
title: requestAnimationFrame写动画
date: 2018-03-02 17:38:21
tags:
---

## 一、requestAnimationFrame 是什么
requestAnimationFrame是window对象的一个函数，目的是请求浏览器进行重绘，这个函数接收一个回掉函数；回掉函数执行的时机是在页面重绘之前。
## 二、与setTimeout或者setInterval动画的区别
### 1.setTimeout或者setInterval的缺点
大多数显示器以及w3c建议的浏览器刷新频率是每秒60次，也就是浏览器每次重绘的间隔时间是 1000ms/6于等于16.67ms。
__缺点一__. setTimeout动画的时候时间间隔往往就需要讲究了，太长动画不流畅，太短的话由于刷新频率的限制，小于16.67往往会掉帧,不通的浏览器有可能刷新频率不一样；
__缺点二__. setTimeout由于js的运行机制，并不是严格按照间隔16.67ms执行一次，setTimeout只是在规定的时间后，将回掉任务加到执行队列里去，需要等主线程的任务以及队列里排在之前的任务执行完毕后才会执行回掉。所以动画肯定不是特别流畅
<!-- more -->
### 2.requestAnimationFrame的优点
requestAnimationFrame就不会存在以上问题了。
__优点一__. requestAnimationFrame会把每一帧中的所有DOM操作集中起来，在一次重绘或回流中就完成，并且重绘或回流的时间间隔紧紧跟随浏览器的刷新频率
__优点二__. 在隐藏或不可见的元素中，requestAnimationFrame将不会进行重绘或回流，这当然就意味着更少的CPU、GPU和内存使用量
__优点三__. requestAnimationFrame是由浏览器专门为动画提供的API，在运行时浏览器会自动优化方法的调用，并且如果页面不是激活状态下的话，动画会自动暂停，有效节省了CPU开销
举个例子
```javascript
var i = 0;
function move() {
	i++;
	if(i>=10){
		return;
	}
 	console.log('move');
	requestAnimationFrame(move);
}
requestAnimationFrame(move);
```
