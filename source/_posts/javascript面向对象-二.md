---
title: javascript面向对象(二)-new运算符的模拟实现
date: 2018-03-04 22:12:53
tags:
---
## new运算符做了什么
1. 创建一个新对象，使该对象的__proto__指向构造函数的prototype
2. 让this指向该对象
3. 执行构造函数
4. 判断构造函数的返回值是引用类型，如果是就返回该值，如果不是就返回新创建的对象
让我们按照以上步骤模拟实现一下
```javascript
function new1(fn) {
  var obj = Object.create(fn.prototype);
  var res = fn.call(obj);
  if(typeof res === 'object' || typeof res === 'function') {
    return res;
  }else {
    return obj;
  }
}
function Foo() {
  this.name = "foo";
}
var foo = new1(Foo);
console.log(foo.name);// foo
console.log(foo instanceof Foo);// true
console.log(foo.__proto__.constructor === Foo);// true
```
<!-- more -->
### 完善实现
兼容构造函数需要传参的情况
```javascript
function new1(fn) {
  var obj = Object.create(fn.prototype);
  var res = fn.apply(obj, [].splice.call(arguments, 1));
  if(typeof res === 'object' || typeof res === 'function') {
    return res;
  }else {
    return obj;
  }
}
function Foo(name, sex) {
  this.name = name;
  this.sex = sex;
}
var foo = new1(Foo, 'foo', 'male');
console.log(foo.name);// foo
console.log(foo.sex);// male
console.log(foo instanceof Foo);// true
console.log(foo.__proto__.constructor === Foo);// true
```
