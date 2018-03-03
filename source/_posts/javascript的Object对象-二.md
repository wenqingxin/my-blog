---
layout: posts
title: javascript的Object对象--配置限制相关(二)
date: 2018-02-16 21:25:42
tags:
---
## 一、Object.seal()
 禁止对象配置，描述符种的configurable设置为flase
 ```javascript
 var obj = {
     name: 'xiaoming'
 };
 Object.seal(obj);
 console.log(Object.getOwnPropertyDescriptor(obj, 'name'));// {value: "xiaoming", writable: true, enumerable: true, configurable: false}
 ```
 <!-- more -->

 ## 二、Object.isSealed()
 判断对象是否被禁止配置操作
```javascript
 var obj = {
     name: 'xiaoming'
 };
 Object.seal(obj);
 console.log(Object.isSealed(obj));// true
```
## 三、Object.freeze()
冻结对象，将描述符种的writeable、configurable设置为false;
```javascript
 var obj = {
     name: 'xiaoming'
 };
 Object.freeze(obj);
 console.log(Object.getOwnPropertyDescriptor(obj, 'name'));// {value: "xiaoming", writable: false, enumerable: true, configurable: false}
```
## 四、Object.isFrozen()
判断对象是否被冻结
```javascript
 var obj = {
     name: 'xiaoming'
 };
 Object.freeze(obj);
 console.log(Object.isFrozen(obj));// true
```
## 五、Object.preventExtensions()
禁止对象添加扩展，不能添加新的属性，但是可以删除属性
```javascript
 var obj = {
     name: 'xiaoming'
 };
 Object.preventExtensions(obj);
 obj.age = 18;
 console.log(obj);// {name: "xiaoming"}
 delete obj.name;
 console.log(obj);// {}
```
## 六、Object.isExtensible()
判断对象是否可扩展
```javascript
 var obj = {};
 console.log(Object.isExtensible(obj));// true
 Object.preventExtensions(obj);
 console.log(Object.isExtensible(obj));// false
```