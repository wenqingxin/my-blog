---
layout: posts
title: javascript的Object对象--常用静态方法(四)
date: 2018-02-16 21:55:52
tags:
---
## 一、Object.assign()
对象浅拷贝(不能拷贝原型链上的属性，不能拷贝不可枚举属性)
```javascript
var obj = {};
var newObj = {
    name: 'xiaoming',
    age: 18
};
Object.assign(obj, newObj);
console.log(obj);// {name: "xiaoming", age: 18}
```
<!-- more -->

## 二、Object.create()
指定某个对象作为原型对象后创建一个新对象并返回。
```javascript
var obj = {
    name: 'xiaoming'
};
var newObj = Object.create(obj);
console.log(newObj.__proto__ === obj);// true
```
## 三、Object.is()
确定两个值是否是相同的值。可以解决+0、-0、NaN比较的问题
```javascript
// 本应该不等的，实际却相等
console.log(+0 === -0);// true
// 本应该相等的实际却不等
console.log(NaN === NaN);// false
``` 
用Object.is()更严谨的判断
```javascript
console.log(Object.is(+0, -0));// false
console.log(Object.is(NaN, NaN));// true
``` 
## 四、Object.getPrototypeOf()
获取对象的原型对象,通过这个来获取对象的原型对象，\_\_proto\_\_作为对象的内部属性，尽量少用，建议是用该方法代替。
```javascript
function Foo(){
}
var foo = new Foo();
console.log(Object.getPrototypeOf(foo) === foo.__proto__);// true
``` 
## 五、Object.setPrototypeOf()
给对象设置原型对象
```javascript
function Foo(){
}
var foo = new Foo();
var newProto = {name: 'xiaoming'};
Object.setPrototypeOf(foo, newProto);
console.log(foo.name);// xiaoming
console.log(Object.getPrototypeOf(foo) === newProto)// true
``` 