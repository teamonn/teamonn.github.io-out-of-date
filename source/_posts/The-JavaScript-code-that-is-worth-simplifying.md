---
title: 那些值得被简化的 JavaScript 代码
date: 2017-07-11 15:21:33
tags: JavaScript
---

最近这段时间写 JavaScript 的代码感觉略有心得，所以就想把自己平时积累的一些知识点整理分享出来，跟大家粗浅的谈一谈 JavaScript 中有哪些代码可以简化，让它变得更优雅。

<!--more-->

### 1. 非空校验 / 短路运算
我们经常会遇到把一个变量 a 赋值给变量 b 的情况。但是值得注意的是，一般都要进行非空的判断。不然把 null 或者 undefined 赋值给了 b，当后面使用 b 的时候就会报错。

简化前：

``` javascript
if (a !== null && a !== undefined) {
	let b = a;
}
```

简化后：

``` javascript
let b = a || '';
```

巧妙运用逻辑短路运算，当 a 值为 null 或者 undefined 的时候，「或」的前半部分都是假，而此时「或」的后半部分值是真，则 b 被赋值为空字符串。

### 2. 三目运算符的使用
简化前：

``` javascript
let big;
if (x > 10) {
    big = true;
} else {
    big = false;
}
```

简化后：

``` javascript
let big = x > 10 ? true : flase;
```

这是我们开发中很常见的情景，if...else 大家都很容易想到，但用上三目运算符后代码既简洁，又高效。

### 3. 优化 if...else
if...else 既要尽量使用三目运算符替代，也要优化它本身。

简化前：

``` javascript
// 当 c 是假值的时候，执行某些操作
if (c === false) {
    // do something...
}
```

简化后：

``` javascript
if (!c) {
    // do something...
}
```

### 4. 自增自减等
假设 count1、count2、count3、count4、x 均已被赋值

``` javascript
count1 = count1 + 1;
count2 = count2 - 1;
```

这种很多人都知道可以简化成 

``` javascript
count1++;
count2--;
```

但是如果自增、自减的值不是 1，而是一个变量 x 时又该怎么写？

简化前：

``` javascript
count1 = count1 + x;
count2 = count2 - x;
count3 = count3 * x;
count4 = count4 / x;
```

简化后：

``` javascript
count1 += x;
count2 -= x;
count3 *= x;
count4 /= x;
```

### 5. 多用 RegExp 隐式声明
如，用正则匹配3个连续的数字

简化前：

``` javascript
let reg = new RegExp("[0-9]{3}","ig");
let result = reg.exec("padding666padding");
console.log(result[0]); 		// 666
```

简化后：

``` javascript
let result = /[0-9]{3}/ig.exec("padding666padding");
console.log(result[0]); 		// 666
```

这样写的好处不仅在于代码更简洁，而且少使用一个变量，减小了内存的开销。

### 6. 字符串取值当成数组处理
例如 `let str = 'myString'`，怎么快速取到字符串 str 的首字母？

简化前：

``` javascript
str.charAt(0);			// m
```

简化后：

``` javascript
str[0];					// m
```

既然字符串可以当成数组来用下标取值，为什么我们不选择这种更高的姿势呢?


### 7. 箭头函数
假设有一个数组 `let nums = [4, 8, 1, 9, 0];`

现在需要将它的每个元素都变成之前2倍

简化前：

``` javascript
for (let i in nums) {
	nums[i] *= 2;
}
console.log(nums)			//输出 [8, 16, 2, 18, 0]
```

简化后：

``` javascript
nums.map(i => 2 * i)
console.log(nums)			//输出 [8, 16, 2, 18, 0]

```

箭头函数是 ES6 的内容，ES6 已经发展多年，兼容性也比较好了。既然它就是为了解放我们开发者劳动力的，那么我们还有什么理由不去使用更好、更快、更简洁的写法呢？

除了箭头函数，这个例子还使用了 Array 的 map 方法。它是比用 for 循环来遍历数组更佳的写法，不熟悉的同学可以自行查阅。

### 8. 优雅的表示大数
平常开发中偶尔也会遇到大数赋值，怎样才是表示大数的正确姿势呢？

简化前：

``` javascript
let longNum = 1000000;
```

简化后：

``` javascript
let longNum = 1e7;

```


### 9. 连等
简化前：

``` javascript
let a = 5;
let b = a + 3;
let c = b;
console.log(c);			// 8
```

简化后：

``` javascript
let a = 5;
let c = b = a + 3;
console.log(c);			// 8
```

### 10. class 的使用
简化前：

``` javascript
function Person (name, age) {
    this.name = name;
    this.age = age;
}

Person.prototype.addAge = function () {
    this.age++;
}

Person.prototype.setName = function (name) {
    this.name = name;
}
```

简化后：

``` javascript
class Person {
    constructor (name, age){
        this.name = name;
        this.age = age;
    }
    addAge () {
        this.age++;
    }
    setName (name) {
        this.name = name;
    }
}
```
简化前是函数式编程的写法，各个方法写在外面格式显得很凌乱。用 ES6 的 class 简化后是面向对象的写法，代码显得更整洁紧凑，可以更方便的实现继承。 

### 11. 字符串拼接
`let name = 'torain';`

简化前：

``` javascript
let str = 'My name is ' + name;
console.log(str)				// My name is torain
```

简化后：

``` javascript
let str = `My name is ${name}`;
console.log(str)				// My name is torain
```


### 12. 提取对象属性或者属性值
``` javascript
let obj = {
	a: 1,
	b: 2
}
```

简化前：

``` javascript
// 提取 obj 的所有属性
for (let i in obj) {
	console.log(i);					// 依次打印出：a b
}

// 提取 obj 的所有属性值
for (let i in obj) {
	console.log(obj[i]);			// 依次打印出：1 2
}
```

简化后：

``` javascript
// 提取 obj 的所有属性
keys(obj)				// ["a", "b"]

// 提取 obj 的所有属性值
values(obj)			// [1, 2]
```

### 13. !! 更快转化成逻辑值
简化前：

``` javascript
let num = 2;
let bln = Boolean(num);
console.log(bln);				// true
```

简化后：

``` javascript
let num = 2;
let bln = !!num;
console.log(bln);				// true
```

转换成逻辑值的函数 Boolean() 可能不是人人都知道，所以可以通过两次逻辑求反得到一个变量的逻辑值。既快速，有方便记忆。

### 14. 对象解构
简化前：

``` javascript
let obj = {
	a: 3, 
	b: 4 
}
console.log(obj.a, obj.b);		// 3 4
```

简化后：

``` javascript
let obj = {
	a: 3, 
	b: 4 
}
let { a, b } = obj;
console.log(a, b);				// 3 4
```

这种 ES6 的新写法看似简洁不了多少，但在 obj 的属性有非常多的时候优势就比较明显了。而且这样把需要用到的属性都放在同一行统一解构，对于使用与没有使用的对象属性可以一览无余。

### 15. ... 数组展开运算符
假如要列举一个数组的所有的元素，可能大多数人想到的是用 for 循环遍历。

简化前：

``` javascript
let arr = [1, 2, 3];
for (let i of arr) {
	console.log(i);			// 依次打印出 1 2 3
}
```

简化后：

``` javascript
let arr = [1, 2, 3];
console.log(...arr);		// 1 2 3
```

***

更多文章，可以访问: [我的csdn博客](http://blog.csdn.net/u014326381/article)

