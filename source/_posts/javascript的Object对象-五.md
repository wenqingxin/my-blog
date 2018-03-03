---
layout: posts
title: javascript的Object对象--实例方法(五)
date: 2018-02-16 22:20:48
tags:
---
## 一、hasOwnProperty()
判断对象的某个属性是自身属性还是原型链上的属性。
```javascript
function Foo() {
    this.name = 'xiaoming';
}
Foo.prototype.age = 18;
var foo = new Foo();
console.log(foo.hasOwnProperty('name'));// true
console.log(foo.hasOwnProperty('age'));// false
```
<!-- more -->

## 二、isPrototypeOf()
判断某个原型对象是否在某个对象的原型链上，跟instanceof的功能相同，只是调用的方向不一样。
```javascript
function Foo() {
    this.name = 'xiaoming';
}
var foo = new Foo();
console.log(Foo.prototype.isPrototypeOf(foo));// true
```
## 三、propertyIsEbumerable()
判断自身属性是否是可枚举的,是hasOwnProperty的加强版
```javascript
function Foo() {
}
Foo.prototype.name = 'xiaoming';
var foo = new Foo();
Object.defineProperties(foo, {
    age: {
        value: 18,
        writable: true,
        configurable: true,
        enumerable: false
    },
    gender: {
        value: 'male',
        writable: true,
        configurable: true,
        enumerable: true
    }
});
// 非自身属性
console.log(foo.propertyIsEnumerable('name'));// false
// 非可枚举属性
console.log(foo.propertyIsEnumerable('age'));// false
// 自身的可枚举属性
console.log(foo.propertyIsEnumerable('gender'));// true
```
## 四、toString()
返回当前对象的字符串形式
```javascript
var obj = {name: 'xiaoming'};
console.log(obj.toString());// [object Object]
```
比typeof判断变量类型更加具体的方式
```javascript
var toStr = Object.prototype.toString;
console.log(toStr.call(1))// [object Number]
console.log(toStr.call([]));// [object Array]
console.log(toStr.call(new Date()));// [object Date]
```
## 五、valueOf()
返回当前对象对应的值
```javascript
var obj = {name: 'xiaoming'};
console.log(obj.valueOf());// {name: 'xiaoming'}
```