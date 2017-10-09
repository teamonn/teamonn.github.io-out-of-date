---
title: for...in 与 for...of 你千万别混淆
date: 2017-04-19 21:18:43
tags: javascript 
---

遇到不少人其实是分不太清 for...in 和 for...of 的。大都是用的时候试下，哪个能解决问题还不报错，就选用哪个了。

> for...in 循环会遍历一个对象所有的可枚举属性。

> for...of 会循环迭代出有序对象（或集合）的每一个属性值。

这是 MDN 上的官方解释。下面，用两个例子说明一切。

<!--more-->

## 遍历的是数组

等等，按照刚在开头的解释，这两个不都是用来遍历对象的么？怎么也可以用来遍历数组？

大哥啊，数组不就是一种对象吗？在 JavaScript 里，万物皆为对象啊 : ) 所以，你可以把数组看成是一种元素有序的对象。

既然如此，先声明一个数组 iterable：


``` javascript
let iterable = [3, 5, 7];
```

**使用 for...in 遍历:**


``` javascript
for (let i in iterable) {
  console.log(i); 		// logs 0, 1, 2
}
```

既然数组是一种特殊的对象，那 iterable 的属性就是其下标了。for...in 遍历的是对象属性，那打印出的自然就是 0，1，2 了。

**使用 for...of 遍历:**


``` javascript
for (let i of iterable) {
  console.log(i); 		// logs 3, 5, 7
}
```

数组是一种元素有序的对象，iterable 的每个属性其实就是数组的每个元素。for...of 遍历的是对象属性值，所以打印出 3，5，7 了。

## 遍历的是对象

没错，这两者都是可以用来遍历对象的方法。

但 for...of 是为各种有序集合专门定制的，并不适用于所有的对象。
 
如果对象含有有序集合，它会以这种方式迭代出该集合的每个元素。

那如果不含有有序集合呢？那当然就不灵了，这超越了本文的讨论范围，暂不讨论。

（前方高能，没听过 JavaScript 原型的请绕行）

先使用原型继承的方法，给 Object 对象和 Array 对象拓展两个属性，属性值均为函数。


``` javascript
Object.prototype.objCustom = function () {}; 
Array.prototype.arrCustom = function () {};
```

接着还是跟上一个例子一样，定义一个 iterable，然后添加一个属性 foo:


``` javascript
let iterable = [3, 5, 7];
iterable.foo = "hello";
```

这个时候 iterable 变得有点小复杂了。

首先，它声明的就是一个数组，所以它应该包含 arrCustom 属性；

其次，数组本身又是一种对象，那它又应该包含刚给 Object 对象拓展的 objCustom 属性；

再次，它声明时给的是一个数组 [3, 5, 7]，那它肯定包含 3，5，7 这样一个有序集合；

最后 `iterable.foo = "hello` 又给它增加了一个属性 foo。

那总结一下就是，iterable 包含一个有序集合 3，5，7，然后还包含 arrCustom、 objCustom、foo 三个属性（无序集合）

此时，使用 **for...in 遍历** iterable:


``` javascript
for (let i in iterable) {
  console.log(i); 			// logs 0, 1, 2, "foo", "arrCustom", "objCustom"
}
```

如果你记住了开篇的一句话 —— for...in 循环会遍历一个对象所有的可枚举属性，那你应该很容易明白打印出来的就是 iterable 所有的属性（其中有序集合是个数组，跟第一个例子一样会打印出数组下标）。

如果使用 **for...of 遍历** iterable:

``` javascript
for (let i of iterable) {
  console.log(i); 			// logs 3, 5, 7
}
```

有了第一个例子做支撑，这里也好理解了。for...of 只能遍历对象中有序集合部分，iterable 中有序的部分就是数组 [3, 5, 7]，那必然只会打印出 3, 5, 7 了。

## 对比总结

* for...in 是循环遍历对象属性。它会遍历一个对象所有的可枚举属性(如果是数组则打印其对应下标)。

* for...of 是循环遍历对象属性值。它是为各种有序集合专门定制的，并不适用于所有的对象。它会以这种方式迭代出有序对象每个元素（如果该有序对象是一个数组，则是迭代出它每一个元素）。

我也是每次使用依然每次都要查文档，关键是每次查了下次用的时候还是迷迷糊糊的。所以决定用这两个例子来辅助记忆，希望我下次使用不会再查文档了。呵~

***

更多文章，可以访问: [我的csdn博客](http://blog.csdn.net/u014326381/article)



