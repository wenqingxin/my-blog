---
layout: posts
title: ES6中的面向对象
date: 2018-02-17 23:06:35
tags:
---

## 一、ES6中类的声明方式
```javascript
class Foo {
    // 构造函数
    constructor() {
        this.name = 'xiaoming';
    }
    // 原型上的方法
    sayName() {
        console.log(this.name);
    }
    // 静态方法
    static showName() {
        console.log('static showName');
    }
}
// 直接调用静态方法
Foo.showName();
let foo = new Foo();
// 实例方法
foo.sayName();
```
<!-- more -->
注意点
1. 类的声明并没有变量提升
2. 类的声明里只能有方法，没有属性
2. 支持表达式方式 var Foo = class { ... };
3. 类声明里的方法是不可枚举的，不能被遍历

## 二、类的继承
ES6中用关键字extend实现类的继承
```javascript
class Father {
    constructor() {
        this.name = 'Father';
    }
    sayName() {
        console.log(this.name);
    }
    static showName() {
        console.log('Father static showName');
    }
}
class Son extends Father{
    // 省略的constructor会默认添加上
}
Son.showName();// Father static showName
let son = new Son();
son.sayName();// Father
```
### super关键字的意义
1. 表示父类的构造方法且只能在构造器第一行以函数方式调用的方式出现，否则将报错。
在上面的例子基础上修改子类的声明
```javascript
class Son extends Father {
    constructor() {
        super();// 继承后必须手动调用弗雷构造器函数，否则会报错
        this.name = 'son';
    }
}
let son = new Son;
son.sayName();// son
```
2. 表示父类的原型对象且只能以调用父类原型方法的方式出现，否则将报错。
```javascript
class Son extends Father {
    constructor() {
        super();
        this.name = 'son';
    }
    saySonName() {
        console.log(super.sayName());// super关键字出现且只能调用父类原型上的方法，否则将报错
        console.log(this.name);
    }
}
let son = new Son;
son.saySonName();// son son
```
3. 表示父类且只能以调用静态方法的方式出现，否则将报错。
```javascript
class Son extends Father {
    static showName() {
        console.log(super.showName());// super关键字出现且只能调用父类的静态方法，否则将报错
    }
}
Son.showName();// Father static showName
```
