---
title: javascript面向对象(一)js继承的实现方法
date: 2018-03-04 19:47:55
tags:
---
## 一、构造函数继承
```javascript
function Parent() {
  this.name = 'parent';
}

function Child() {
  Parent.call(this);
  this.age = 11;
}

var child = new Child();
console.log(child.name); //parent
```
缺点：那就是父类原型上的方法无法继承到子类
<!-- MORE -->
## 二、原型继承
```javascript
function Parent() {
  this.name = 'parent';
}

function Child() {
  this.age = 11;
}

Child.prototype = new Parent();
var child = new Child();
console.log(child.name); //parent
```
缺点：子类共享同一个原型，当修改子类属性值的时候会影响到原型，修改了原型也会影响到其他的子类，比如下面的例子
```javascript
function Animal() {
  this.name = 'animal';
  this.data = [1,2];
}

function Cat() {

}

Cat.prototype = new Animal();
var cat = new Cat();
var cat1 = new Cat();
// cat修改数据
cat.data.push(3);
console.log(cat.data);// [1, 2, 3]
// cat1也同样改变了
console.log(cat1.data);// [1, 2, 3]
```

## 三、组合继承
```javascript
function Animal() {
  this.name = 'animal';
  this.data = [1,2];
}

function Cat() {
  Animal.call(this);
}

Cat.prototype = new Animal();
var cat = new Cat();
var cat1 = new Cat();
// cat修改数据
cat.data.push(3);
console.log(cat.data);// [1, 2, 3]
// cat1没有改变
console.log(cat1.data);// [1, 2]
```
这样既能继承原型上的方法与属性，同时子类实例修改继承的属性时不会影响到其他实例。

## 四、组合继承优化
组合继承上述的方式够完美吗？事实上可以优化。
### 优化第一步
Cat.prototype = new Animal();这句代码事实上又会将父类构造器里this上的属性添加到原型链上去，而我们再子类构造器中已经通过Animal.call(this)的方式将同样的属性与方法加到了子类的实例上，再在原型上加一次实际上是不必要的，直接将父类的原型拿过来就可以了，我们可以做出如下的修改：
```
Cat.prototype = new Animal();
```
改成
```
Cat.prototype = Animal.prototype;
```
等等。。。聪明的你应该又发现了问题，如果我修改了Cat.prototype上的属性，岂不是相当于修改了 Animal.prototype的属性，相当于我修改一个子类就会影响到其他子类。看看下面是不是这样
```javascript
function Animal() {
  this.name = 'animal';
}

function Cat() {
  Animal.call(this);
}

function Dog(){
  Animal.call(this);
}

Cat.prototype = Animal.prototype;
Dog.prototype = Animal.prototype;
Cat.prototype.sex = 'm';
Dog.prototype.sex = 'f';

var cat = new Cat();
var dog = new Dog();
console.log(cat.sex);// f
console.log(dog.sex);// f
```
### 优化第二步
经过上面的尝试，我们知道不能直接让子类的prototype引用父类的prototype，不能直接引用，我们的目的其实是想让实例能够在原型链上引用父类原型上的方法，那么我们可以创建一个中间对象，让它的prototype直接引用父类的prototype，而我们的子类原型引用它的实例，这样子类修改原型时就不会直接影响到父类的原型了，上代码
```javascript
function Animal() {
  this.name = 'animal';
}

function Cat() {
  Animal.call(this);
}

function Dog(){
  Animal.call(this);
}

var tempClass = function TempClass(){};
tempClass.prototype = Animal.prototype;
Cat.prototype = new tempClass();
Dog.prototype = new tempClass();
Cat.prototype.sex = 'm';
Dog.prototype.sex = 'f';

var cat = new Cat();
var dog = new Dog();
console.log(cat.sex);// m
console.log(dog.sex);// f
```
这样写下来，创建一个临时的类总感觉有点麻烦啊，我们再来简化下吧。

### 优化第三步
我们看看这三行代码
```
var tempClass = function TempClass(){};
tempClass.prototype = Animal.prototype;
Cat.prototype = new tempClass();
```
要做的事情可以概括为，以父类的原型创建一个实例对象，等等，以xxx对象为原型创建一个对象这不就是Object.create方法做的吗？于是，你懂的，我们代码可以修改成
```javascript
function Animal() {
  this.name = 'animal';
}

function Cat() {
  Animal.call(this);
}

function Dog(){
  Animal.call(this);
}

Cat.prototype = Object.create(Animal.prototype);
Dog.prototype = Object.create(Animal.prototype);
Cat.prototype.sex = 'm';
Dog.prototype.sex = 'f';

var cat = new Cat();
var dog = new Dog();
console.log(cat.sex);// m
console.log(dog.sex);// f
```
然而然而，我们判断某个对象是哪个类实例的时候，可以用instanceof，该运算符是判断运算符右边的构造器的原型对象是否在左边对象的原型链上，于是我们可以试试以下代码
```javascript
console.log(cat instanceof Cat, cat instanceof Animal);// true true
```
都是true那么我们怎么判断某个实例对象具体是哪个类实例化出来的呢？答案就是用cat.\_\_proto\_\_.constructor

### 优化第四步
接着上述第三步，我们看看cat.\_\_proto\_\_.constructor是什么
```javascript
console.log(cat.__proto__.constructor);// ƒ Animal() {this.name = 'animal';}
```
没错，是Animal，于是我们再修改,得出最终结论
```javascript
function Animal() {
  this.name = 'animal';
}

function Cat() {
  Animal.call(this);
}

Cat.prototype = Object.create(Animal.prototype);
Cat.prototype.constructor = Cat;
var cat = new Cat();
console.log(cat.__proto__.constructor);// ƒ Cat() { Animal.call(this); }
```
