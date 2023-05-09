---
title: JavaScript 的深拷贝和浅拷贝
date: 2017-04-09 12:20:33
tags:
---

在 JavaScript 的变量赋值操作中，如果一个变量值是简单类型，直接复制没有问题。但如果是对象或者数组，直接复制后的新对象或者数组只要一修改，原对象或者数组就会同样跟着被修改。如果你不了解深拷贝和浅拷贝，你可能就会觉得这是 bug，不可理解。但是看完本文你就能理解了。

<!--more-->

## 什么是深拷贝和浅拷贝？
浅拷贝比较容易理解，所以先从浅拷贝开始说起吧！

浅拷贝
> 就是将一个对象（或数组）的内存地址『编号』复制给另一个对象（或数组）

深拷贝
> 增加一个指针，并且申请一个新的内存地址，使这个增加的指针指向这个新的内存，然后将原变量对应内存地址里的值逐个复制过去

## 深拷贝和浅拷贝怎么实现？
**数组**

eg1. 浅拷贝
``` javascript
var arr = ["One", "Two", "Three"];
console.log("原数组的值：" + arr);
var newArr = arr;
newArr[1] = "newTwo";
console.log("新数组的值：" + newArr);
console.log("浅拷贝后，原数组的值：" + arr);
```
运行结果：

![这里写图片描述](http://img.blog.csdn.net/20170409104700282?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDMyNjM4MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

因为 arr 浅拷贝赋值给 newArr 的是一个指向 arr 内存地址的引用，所以修改 newArr 的第2个元素的值，会将原数组 arr 的第二个元素值也修改掉了（其实在内存里，两个数组共用一个内存空间）。


eg2. 深拷贝
``` javascript
var arr = ["One", "Two", "Three"];
console.log("原数组的值：" + arr);
var newArr = arr.slice(0);
newArr[1] = "newTwo";
console.log("新数组的值：" + newArr);
console.log("深拷贝后，原数组的值：" + arr);
```
运行结果：

![这里写图片描述](http://img.blog.csdn.net/20170409105005209?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDMyNjM4MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

因为深拷贝是给 newArr 申请一个新的内存地址，并使 newArr 的指针指向这个新的内存。然后再把 arr 对应内存的值逐个复制到这个新内存地址。所以这时再修改 newArr 的第2个元素的值，就不会将原数组 arr 的第二个元素值也修改掉了（在内存里，两个数组不再共用一个内存空间了）。

**对象**

eg1. 浅拷贝
``` javascript
var obj = {
	name: 'xu',
	age: 24,
	sex: 'men',
}
console.log("原对象的值：");
console.log(obj);
var newObj = obj;
newObj.age = 25;
console.log("新对象的值：");
console.log(newObj);
console.log("浅拷贝后，原对象的值：");
console.log(obj);
```
运行结果：

![这里写图片描述](http://img.blog.csdn.net/20170409105542962?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDMyNjM4MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

对象的浅拷贝跟数组一样，因为赋值过去的是原对象的引用，所以修改了新对象 newObj 的属性 age 后，原对象属性 age 也被修改了。

eg2. 深拷贝
``` javascript
var obj = {
	name: 'xu',
	age: 24,
	sex: 'men',
}
console.log("原对象的值：");
console.log(obj);
var newObj = {};
for (var i in obj) {
	newObj[i] = obj[i];
}
newObj.age = 25;
console.log("新对象的值：");
console.log(newObj);
console.log("深拷贝后，原对象的值：");
console.log(obj);
```
运行结果：

![这里写图片描述](http://img.blog.csdn.net/20170409105749213?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDMyNjM4MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

这是针对一个对象中的所有属性值都是基本数据类型的情况。但是如果一个对象的属性值还是对象这种情况，就需要递归调用浅拷贝。可以如下处理：

eg. 对象深拷贝拓展
``` javascript
function deepCopy(obj, copyObj) {
	var newObj = copyObj || {};
	for (var i in obj) {
		if (typeof obj[i] === 'object') {
			newObj[i] = (obj[i].constructor === Array) ? [] : {};
			deepCopy(obj[i], newObj[i]);
		} else {
			newObj[i] = obj[i];
		}
	}
	return newObj;
}

var obj = {
	name: {
		oldName: 'pan',
		newName: 'zhengmao'
	},
	age: 24,
	sex: 'men',
}
console.log("原对象的 name 属性值：");
console.log(obj.name);
var secObj = {};
secObj = deepCopy(obj, secObj);	 // 将 obj 的所有属性都复制到 secObj 中
secObj.name = {
	oldName: 'pan2',
	newName: 'zhengmao2'
}
console.log("新对象的的 name 属性值：");
console.log(secObj.name);
console.log("深拷贝后，原对象的 name 属性值：");
console.log(obj.name);
```

运行结果：

![这里写图片描述](http://img.blog.csdn.net/20170409111121300?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDMyNjM4MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

实现原理，其实就是先新建一个空对象，在内存中新开辟一块地址，把被复制对象的所有可枚举的（注意可枚举的对象）属性方法一一复制过来，注意要用递归来复制子对象里面的所有属性和方法，直到所有子代属性都为基本数据类型。

## 结论

数组深拷贝：

1. 数组截取：`var newArr = arr.slice(0) `
2. 数组连接：`var newArr = arr.concat()`
3. 数组转字符串，再转回数组：`var newArr = arr.join(',').split(',')`
4. 无脑方法：for 循环逐个元素拷贝

对象深拷贝：

1. 对象拷贝方法：`var newObj = Object.assign({}, obj)`
2. 字符串对象互转方法结合：`var newObj = JSON.parse( JSON.stringify(obj) )`
3. 无脑方法：for 循环逐个属性拷贝

对于简单的数据类型（如 Number、String、Boolean 等），JavaScript 变量赋值即是值复制（深拷贝）。但是对象或者数组，直接赋值其实复制的只是内存地址（浅拷贝）。这样错误的操作会让父对象或父数组随时存在被篡改的可能。所以数组和对象拷贝时，我们可以按照以上方法进行深拷贝。

***

更多文章，可以访问: [我的csdn博客](http://blog.csdn.net/u014326381/article)





