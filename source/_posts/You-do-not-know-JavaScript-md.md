---
title: 《你不知道的JavaScript》上、中卷阅后感
date: 2018-02-22 13:19:58
tags: JavaScript
---

今年春节期间，利用放假在家的时间看完了《你不知道的JavaScript》上、中卷。

我个人感觉，它与《JavaScript高级程序设计》最为不同的一点就是，JS高程3 侧重的是JS这门语言的使用规则和规范，而《你不知道的JavaScript》侧重的是JS中那些让人难以理解甚至是误解的机制，以及这些机制背后深层次的原理。

<!--more-->

*对我个人有很大启示作用的知识点：*

1. **LHS**（赋值操作的目标） & **RHS**（赋值操作的源头）
2. **with(obj)**（更加方便的访问和操作obj对象）
3. **不是值的值**（undefined、null）、**不是数字的数字**（NaN）
4. **new Array(..)** 参数是集合的时候表示给数组赋初值，只有1个参数时表示新创建数组的长度
5. `var a = 3; var b = '2'; a - b // 1   a + b // '32'`
6. `Date.now() === (new Date()).getTime() === +new Date()`
7. **eval(...)** 字符串当做 JavaScript 代码进行执行。将Json字符串转化为JavaScript对象，但eval()还要用一对圆括号将字符串包起来
8. **slice** 和 **substring** 方法接收的是起始位置和结束位置(不包括结束位置)，而 **substr** 接收的则是起始位置和所要返回的字符串长度
9. **JSON.stringify()** 其实是有3个参数
10. **具有假值的6个值**：undefined、null、NaN、''(空字符串)、0、false
11. （~ 位非操作）**原码**、**反码**、**补码**
12. `a || b`  等价  `a ? a : b`; `a && b` 等价 `a ? b : a`
13. **事件循环、事件并发、纸牌屋心理**
14. **生成器**、**迭代器**
15. **Web Worker**
16. **异步** & **并行**


### LHS & RHS
LHS 是赋值操作的目标，RHS 是赋值操作的源头。

``` javascript
function foo(a) { 
	var b = a;
	return a + b; 
}
var c = foo( 2 );
```
一共是3处 **LHS**（`c = ..;`、`a = 2`(隐式变量分配)、`b = ..`），4次 **RHS**（`foo(2..`、`= a;`、`a ..`、`.. b`）。


### with(obj)
with语句 用来扩展一个语句的作用域链。

``` javascript
with (Math) {
  console.log(PI);	// 等价于 Math.PI
  console.log(cos(PI));	// 等价于 Math.cos(PI)
  console.log(sin(PI / 2));	// 等价于 Math.sin(PI / 2)
}
```

### JS中一些比较特殊的值类型
* **不是值的值**：undefined、null
* **不是数字的数字**：NaN
* **具有假值的6个值**：undefined、null、NaN、''(空字符串)、0、false

### 一些比较特殊的JS代码
``` javascript
var a = 3; 
var b = '2'; 
a - b // 1   
a + b // '32'
```

``` javascript
Date.now() === (new Date()).getTime() === +new Date()
```

``` javascript
a || b  // 等价  a ? a : b；
a && b // 等价 a ? b : a
```

### new Array(...)
比较让人忽略的一点是，当参数是集合的时候表示给数组赋初值，只有1个参数时表示新创建数组的长度。

``` javascript
var arr = new Array(2);
console.log(arr);	// [empty × 2]
```

``` javascript
var arr2 = new Array(1, 2);
console.log(arr2);	// [1, 2]
```

了解了这个之后，如果我们要创建只有一个元素的数组，就只能用字面量初始化赋值的方法了。如：`var arr3 = [1];`

### eval(...) 和 JSON.stringify()
eval() 是将一个字符串当做 JavaScript 代码进行执行的内建函数。

但它还能像JSON.stringify()一样，将Json字符串转化为JavaScript对象。不过它要用一对圆括号将要当成代码执行的字符串包起来。

**由于json是以”{}”的方式来开始以及结束的，在JS中，它会被当成一个语句块来处理，所以必须强制性的将它转换成一种表达式。**

``` javascript
var str = JSON.stringify({ky: 'val'});
console.log(eval('(' + str + ')'));	// {ky: "val"}
```

JSON.stringify() 方法是将一个JavaScript值(对象或者数组)转换为一个JSON字符串的内建函数。

**值得注意的是，它有3个参数，既可以在序列化过程中逐个执行函数操作，还可以指定序列化之后JSON对象字符串的缩进对齐格式。**

语法 `JSON.stringify(value[, replacer [, space]])`

``` javascript
var str2 = JSON.stringify({a: '34', b: 5}, null, 4);
console.log(str2);
```
输出如下：

``` json
"{
    "a": "34",
    "b": 5
}"
```

### slice()、substring()、substr()
**slice** 和 **substring** 接收的是起始位置和结束位置(不包括结束位置)，而 **substr** 接收的则是起始位置和所要返回的字符串长度。

``` javascript
var test = 'hello world';
console.log(test.slice(4, 7));             //o w
console.log(test.substring(4, 7));         //o w
console.log(test.substr(4, 7));            //o world
```

### 原码、补码、反码
毕业一年多了，由于长时间没用到这块儿，原码、补码、反码等计算机组成原理的知识已经忘了很多了。看这两本书的时候涉及到了~（位非操作），我就把这块儿重新温习了一遍，毕竟计算机底层技术是我们编程的内功。

**原码**：最高位表示符号位，剩下的位数是这个数的绝对值的二进制；

**反码**：正数的反码就是其原码，负数的反码就是在其原码的基础之上，符号位不变，其他位取反；

**补码**：正数的补码就是其原码，负数的补码就是在其反码的基础之上 +1；

### 事件循环、事件并发、纸牌屋心理
事件循环：是一个类似于 while 实现的持续运行的循环。事件一旦被触发，就会被立即加入到事件循环队列的尾部，等待被执行。
事件并发：并行地执行多个事件，一般需要是多线程。
纸牌屋心理：对于某段代码，因为不知道为什么这样写，所以一点都不敢碰。

### 生成器、迭代器
``` javascript
// 生成器
function *createIterator() {
    yield 1;
    yield 2;
    yield 3;
}
// 生成器能像正规函数那样被调用，但会返回一个迭代器
let iterator = createIterator();
console.log(iterator.next().value); // 1
console.log(iterator.next().value); // 2
console.log(iterator.next().value); // 3
```
对于已有数据，需要构造成生成器，可以这样:

``` javascript
var as = ['a','b','c'];
function *createIterator() {
	for(var i = 0; i < as.length; i++){
		yield as[i];
	}
}
let iterator = createIterator();
console.log(iterator.next().value); // a
console.log(iterator.next().value); // b
console.log(iterator.next().value); // c
```

### Web Worker
Web Worker 是 HTML5 的一个新特性，用来在浏览器环境中实现多线程。
你可以这样来实例化一个 Web Worker：

```var w1 = new Worker( "http://some.url.1/mycoolworker.js" );```

这个 URL 应该指向一个 JavaScript 文件的位置(而不是一个 HTML 页面!)，这个文件将 被加载到一个 Worker 中。然后浏览器启动一个独立的线程，让这个文件在这个线程中作 为独立的程序运行。

### 异步 & 并行
**异步** 和 **并行** 常常被混为一谈，但实际上它们的意义完全不同。异步，是
关于现在和将来的时间间隙，而并行是关于能够同时发生的事情。

异步：假如你打电话点买卖，问老板有没有辣椒炒肉。老板说，我要去厨房看看，您稍等。你让老板看好了再打电话告诉你，你先挂掉电话这种方式是**异步**。如果老板让你别挂电话，等他去看完了告诉你这就是**同步**。

并行：就是你同时做多件事情，比如边吃辣椒炒肉，边看电影。

多线程：指某个程序可以并行执行多个任务。人是单线程的，人的并行可以看作是你做多件事情是在很短的时间内相互切换的。

这两本书对于JavaScript进阶编程确实有很大的作用。里面讲到的很多东西可能会让你在阅读的时候迷失了自己，甚至怀疑自己的JS白学了。但是不要紧，这正说明你的技术功底得到了空前的提升 :)

***

参考资料:
1- 《你不知道的JavaScript（上卷）》
2- 《你不知道的JavaScript（中卷）》


