---
title: 全面学习JS对象
date: 2017-08-03 19:26:14
tags: javascript
---

有一天，前端菜鸟问老鸟问题。

> 菜鸟：什么是对象啊？
老鸟：有句话不知当讲不当讲？
菜鸟：请讲。
老鸟：佛说，万物皆为对象！
菜鸟：...#@&%!

<!--more-->

开个玩笑。Web前端开发，JavaScript是核心。而JavaScript中，对象相关的知识又是核心。

所以，对于一个前端开发者来说，学习好JS对象知识是必不可少的。

### 对象的基本结构
**对象**，是一组由若干个无序键值对（key: value）组成的数据集合。

在 javascript 中，所有的数据都可以被视作对象。在某个对象中，每一个键值对叫做`成员`，键名叫做`属性`或者`方法`。

### 如何构造对象、复制对象（深拷贝）、访问或者删除对象属性
*常见几种构造对象方法：*
``` javascript
// 方法1，创建字面量
var person = {
  name: 'rain',
  age: 25
}

// 方法2，定义对象实例
var person = new Object();
person.name = 'rain';
person.age = 25;
// 方法3，工厂模式
function createPerson(name, age){
  var o = new Object();
  o.name = name;
  o.age = age;
  return o;
}
var person = createPerson('rain', 25);
// 方法4，构造函数模式
function Person(name, age){
  this.name = name;
  this.age = age;
}
var person = new Person('rain', 25);
// 方法5，原型模式
var person = Object.create(Object.prototype, {
  name: {
    value: 'rain'
  },
  age: {
    value: 25
  }
})
```
*复制对象*
JavaScript中对象（或数组）与其他基础数据类型不同，它的引用是**按址引用**。的。如果直接使用等号简单复制，会造成不同的变量引用同一个对象，即它们会指向同一个内存地址。修改其中一个变量，其他的变量就会跟着一起改变。
解决办法就是**对象深拷贝**。`var newObj = Object.assign({}, obj)`方法是伪深拷贝，它只能深拷贝对象第一层子属性。`var newObj = JSON.parse( JSON.stringify(obj) )`方法是深拷贝，但是只能深拷贝对象的属性，对象的方法在拷贝过程中会被丢掉。
所以真正的深拷贝是需要自己实现的，可以通过**递归调用浅拷贝**，一层层复制一个对象的子代属性来实现。
参见：[JavaScript 的深拷贝和浅拷贝](https://teamonn.github.io/2017/04/09/deep-copy-and-shallow-copy/#more)

*访问对象属性*
读取属性有两种方法，一种是**点运算符**（对象点属性/方法即可拿到一个对象的某个属性值或方法），一种是**方括号运算符**。方括号运算符可以使用表达式，也可以读取键名为数字、不符合标识符条件的、关键字和保留字的属性。对象的属性本质是一个字符串，加不加引号都可以。
`Object.keys(obj)`可以遍历 obj 所有可以枚举的属性，返回所有可枚举自身属性的属性名组成的数组。

*删除对象属性*
`delete person.name`即删除掉person对象的name属性。

### 面向OOP编程
JavaScript 是一门彻底的面向对象的语言。它在ES6之前没有类（class）的概念，它是基于原型来面向对象的。

**基于类的面向对象 VS 基于原型的面向对象**
> 在 *基于类的面向对象* 方式中，对象（object）依靠 类（class）来产生。而在 *基于原型的面向对象* 方式中，对象（object）则是依靠 构造器（constructor）利用 原型（prototype）构造出来的。就像工厂要造一辆车，一方面，工人必须参照一张工程图纸，设计规定这辆车应该如何制造。这里的工程图纸就好比是语言中的 类 (class)，而车就是按照这个 类（class）制造出来的；另一方面，工人和机器 ( 相当于 constructor) 利用各种零部件如发动机，轮胎，方向盘 ( 相当于 prototype 的各个属性 ) 将汽车组装出来。

**JavaScript如何实现对象的继承？**
JavaScript无法实现接口继承，只支持实现继承，所以它主要是依靠原型链来实现继承的。
1. **原型方法**（将父类的实例作为子类的原型，Cat.prototype = new Animal()）
2. **借用构造函数**（借父类的构造函数来增强子类实例，子类构造函数中通过call/apply方法把this传到父类）
3. **原型链方式/实例继承**（子类通过prototype将所有在父类中通过prototype追加的属性和方法都追加到Child）
4. **拷贝继承**（子类构造函数里面逐个拷贝父类属性）
5. **组合继承**（把实例函数都放在原型对象上，以实现函数复用。同时还要保留借用构造函数方式的优点）
6. **寄生组合继承**（切掉了原型对象上多余的那份父类实例属性）
（*见 《JavaScript高级程序设计 第3版》第6章 继承*）

![js实现继承方式对比](/assets/obj_extend.jpg)

### 值引用和对象引用
程序中变量分为两种，一种计算机存储的是这个变量的值（值引用），一种存储的是这个变量在计算机内存中的地址（对象引用）。
与其他简单数据类型不同，数组和对象的引用是**按址引用**的。如果不同的变量引用同一个对象，它们会指向同一个内存地址，修改其中一个变量，会影响到其它的变量。

*上面讲到的 浅拷贝 和 深拷贝 就是 值引用 和 对象引用 区别的具体体现。*

### 原型对象（prototype和\_\_proto\_\_）、原型链、作用域链、闭包
*构造函数、原型和实例的关系:*
每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针。

![prototype和_proto_的关系](/assets/obj_prototype.png)

``` javascript
function Person (n) {  // 构造函数
  this.name = n;
}
var me = new Person('rain');  // me 是实例对象
me.__proto__ === Person.prototype;  // true
```

简单来讲**原型对象**是用来指导某个构造函数来构造对应对象的父对象。这些原型对象一层层嵌套，形成一个链条，就叫**原型链**。JavaScript 主要通过原型链实现继承。

所有函数的默认原型都是 Object 的实例，因此默认原型都会包含一个内部指针，指向 Object.prototype。但原型链的顶点是null，而不是Object.prototype。**原型链上只能有对象**。

**作用域：**程序中函数或者变量的作用范围，可以是局部，也可以是全局。
**作用域链：**各个函数作用域的相互嵌套会形成函数作用域链。当在自身作用域内找不到该变量的时候，会沿着作用域链逐步向上查找，若在全局作用域内部仍找不到该变量，则会抛出异常。
**闭包：**在一个函数的内部创建另外一个函数。当外部函数被调用的时候，内部函数也就随着创建，这样就形成了闭包。作用是 —— 用来实现在一个函数作用域中访问另一个函数作用域的变量或方法。

### this指向问题(apply/call/bind)
this用法很多，不同场合this指向不同。
1. **普通函数调用：**浏览器宿主的全局环境中，this指的是window对象
2. **作为对象的方法来调用：**this就指向这个被调用方法的父对象
3. **作为构造函数来调用：**this指向这个新构造出来的新对象
4. **使用apply/call方法来调用：**this指的就是第一个参数（func.call(this, arg1, arg2)，func.apply(this, [arg1, arg2])）
5. **bind方法：**创建一个新的函数, 当被调用时，将其this关键字设置为提供的值
6. **es6箭头函数：**绑定this为定义时所在的作用域，而不是运行时所在的作用域(因为箭头函数根本没有自己的this)
``` javascript
// call方法(apply类似)
function cat(){
  this.food = "fish";
  this.say = function(){
    console.log("I love " + this.food);
  }
}
var blackCat = new cat();
blackCat.say();  // I love fish
var whiteDog = {
  food: "bone"
}
blackCat.say.call(whiteDog);  // I love bone 这样让whiteDog也能调用blackCat的say方法
// bind方法
var x = 1;
var obj = {
  x: 2,
  getX: function(){
    console.log(this.x);
  }
}
console.log(obj.getX());  // 2
var oldObj = obj.getX;
oldObj();  // 1  oldObj暴露在全局作用域，oldObj的this指向的是window
var newObj = obj.getX.bind(obj);  // 用bind将obj.getX的this指向绑定为obj的this指向
newObj();  // 2
```

### 最后的小结
由于公司业务也比较忙，这边文章（其实就是我个人的学习笔记）我差不多整理了3个晚上。之前初学 JavaScript 太过走马观花，没有脚踏实地好好学习原生JS的基础知识。导致工作一年来，越来越觉得基础的重要性。趁现在还能亡羊补牢，赶紧抽时间把JS的基础知识好好系统过一遍吧！

毕竟，磨刀不误砍柴工。

***

推荐参考和学习的资料：
1. [《JavaScript高级程序设计 第3版》](https://book.douban.com/subject/10546125/)
2. [《JavaScript语言精粹》](https://book.douban.com/subject/3590768/)
3. [《你不知道的JavaScript 上》](https://book.douban.com/subject/26351021/)
4. [mozilla开发者文档 MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript)
5. [《阮一峰 ES6教程》](http://es6.ruanyifeng.com/)



