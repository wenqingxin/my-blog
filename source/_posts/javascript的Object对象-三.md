---
layout: posts
title: javascript的Object对象--属性遍历相关(三)
date: 2018-02-16 21:41:49
tags:
---
## 一、Object.keys()
返回对象自身属性key的数组，不包括原型链上的属性,不包括不可枚举属性
``` javascript
var obj = {
    name: 'xiaoming',
    age: 18
};
var keys = Object.keys(obj);
console.log(keys);// ["name", "age"]
```
## 二、Object.values()
返回自身属性value的数组，不包括原型链上的属性,不包括不可枚举属性
``` javascript
var obj = {
    name: 'xiaoming',
    age: 18
};
var values = Object.values(obj);
console.log(values);// ["xiaoming", 18]
```
<!-- more -->

## 二、Object.entries()
返回自身属性键值对的二位数组，不包括原型链上的属性,不包括不可枚举属性
``` javascript
var obj = {
    name: 'xiaoming',
    age: 18
};
var entries = Object.entries(obj);
console.log(entries);// [["name", "xiaoming"], ["age", 18]]
```
## Object.getOwnPropertyNames()
返回对象自身属性名称的数组，跟Object.keys()的区别是这个方法可以返回不可枚举属性名称
```javascript
var obj = {};
Object.defineProperty(obj, 'name', {
	value: 'xiaoming',
	enumerable: false
})
var propertyNames = Object.getOwnPropertyNames(obj);
console.log(propertyNames);// ["name"]
```