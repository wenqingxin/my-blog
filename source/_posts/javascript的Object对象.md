---
layout: posts
title: javascript的Object对象--属性定义相关(一)
date: 2018-02-16 19:30:42
tags:
---
## 一、Object.defineProperty()
我们默认情况下给一个对象定义属性时，该属性的配置都是系统预定义好的，当我们需要手动定义配置的时候可以用该方法定义新的属性或者修改现有的属性。
### 语法：
``` javascript
Object.defineProperty(obj, prop, descriper)
```
### 参数：
obj 要在其上定义属性的对象。
prop 要定义或修改的属性的名称。
descriptor 将被定义或修改的属性描述符。
<!-- more -->

### 描述符：
描述符是一个对象，可以配置属性的相关性质，分为以下两类:
1. __数据描述符__,配置项有
value 属性对应的值
writable 值是否可以被修改,该属性值为true时才能对属性进行赋值操作
configurable 属性是否可以被修改, 比如该属性值为true时，delete才能删除属性值
enumerable 属性是否可以杯枚举,比如该属性值为true时，for...in才能遍历到属性值
举个栗子🌰
```javascript
var obj = {};
Object.defineProperty(obj, 'name', {
    value: 'xiaoming',
    writable: false,
    configurable: false,
    enumerable: false
});
console.log(obj.name);// xiaoming
obj.name = 'xiaohong';
console.log(obj.name);// xiaoming
delete obj.name;
console.log(obj.name);// xiaoming
for(var name in obj) {
    console.log(name)// undefined
}
```
2. __存取描述符__,配置项有
get 提供getter方法，读取属性值的时候调用该方法
set 提供setter方法，设置属性值的时候调用该方法
configurable 属性是否可以被修改
enumerable 属性是否可以枚举
举个栗子🌰
```javascript
var obj = {};
var name = 'xiaoming';
Object.defineProperty(obj, 'name', {
    get: function() {
        console.log('name的值被读取了');
        return name;
    },
    set: function(val) {
        console.log('name的值被修改了',val);
        name = val;
    },
    configurable: true,
    enumerable: true
});
var getName = obj.name;// name的值被读取了
obj.name = 'xiaohong';// name的值被修改
```
### 注意点
数据描述符(value、writable)与存取描述符(get、set)方法<font color=red>不能同时存在</font>，如果同时存在，将会产生异常。
一旦指定了描述符后，没有具体指定的属性值默认都为false。
## 二、Object.defineProperties()
在一个对象上定义或者修改多个属性，直接上代码
```javascript
var obj = {};
Object.defineProperties(obj,{
    'name': {
        value: 'xiaoming',
        writable: true,
        configurable: true,
        emuerable: true
    },
    'age': {
        value: 12,
        writable: true,
        configurable: true,
        emuerable: true
    }
});
console.log(obj);// {name: "xiaoming", age: 12}
```
## 三、Object.getOwnPropertyDescriptor()
获取一个对象上属性的描述符
```javascript
var obj = {};
obj.name = 'xiaoming';
console.log(Object.getOwnPropertyDescriptor(obj, 'name'));// {value: "xiaoming", writable: true, enumerable: true, configurable: true}
```

## 四、Object.getOwnPropertyDescrptors()
获取一个对象上所有属性的描述符
```javascript
var obj = {
    name: 'xiaoming',
    age: 18
};
var descriptors = Object.getOwnPropertyDescriptors(obj);
console.log(descriptors);// {name: {…}, age: {…}}
```
## 五、Object.getOwmPropertySymbols()
返回一个数组，该数组包含了指定对象所有为symbol类型的属性key
 ``` javascript
let name = Symbol('name');
let age = Symbol('age');
let obj = {
    [name]: 'xiaoming',
    [age]: 18
};
let symbolKeys = Object.getOwnPropertySymbols(obj);
console.log(symbolKeys);// [Symbol(name), Symbol(age)]
 ```