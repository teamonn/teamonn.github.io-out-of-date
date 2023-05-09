---
title: 如何优雅的获取你点击的那个 li？
date: 2017-07-09 23:55:58
tags: 面试
---

> 如果一个 ul 中有多个 li 标签，如何快速获取到你所点击的那个 li 在整个 ul 中的顺序？

这是一道我们 web 前端程序员会经常在面试中被问到的经典考题。它看似简单却非常考察功力，因为它的解法非常多。你回答的解法，很可能会直接影响面试官对你技术水平的判定。

<!--more-->

首先，我们假设 ul 和 li 的层级关系是这样的：
``` html
<ul onClick="handleClick(event)">
  <li id="1">li-1</li>
  <li id="2">li-2</li>
  <li id="3">li-3</li>
  <li id="4">li-4</li>
  <li id="5">li-5</li>
</ul>
```

### 平常很多人的错误写法
``` javascript
var list = document.getElementsByTagName("li");
for(var i = 0; i < list.length; i++) {
  list[i].onclick = function() {
    console.log(i);
  }
}
```

为什么说这是一个最常见的错误解法呢，因为这样你怎么点击打印出来的都是"6"。原因是这个 for 循环在你点击之前就已经执行了，i 已经到了6。而这个 for 循环形成了一个闭包，i 这个变量会暂存在内存中，直到你刷新页面之后它重新加到6。

### 1：jq each
``` javascript
$('ul li').each(function(){
  $(this).click(function(){
    console.log(this.id)
  })
})
```

### 2：js this
``` javascript
$('ul li').click(function(){
  var index = $(this).index() + 1;
  console.log(index);
  return false;
})
```

上面两种使用 JQuery 的解法是最容易想出来，但是它借助了 Jquery，不适合考察面试者原生 JavaScript 的水平。所以你答出来一般面试官也并不会觉得有什么，他还会接着问你有没有其他的解法。

### 3：js setAttribute
``` javascript
var lis = document.getElementsByTagName('li');
for (var i = 0; i < lis.length; i++) {
  lis[i].setAttribute('index', i);
  lis[i].onclick = function(){
    console.log(parseInt(this.getAttribute('index')) + 1);
  } 
}
```

这个解法是我第一次碰到这种题目面试想到的解法，利用标签可以自定义属性的特性，给每个 li 事先绑定一个顺序值。然后点击的时候就可以直接打印出这个你刚定义的值即可。此法简单管用，思路也比较容易想到。

### 4：js var -> let
``` javascript
var list = document.getElementsByTagName("li");
// 仅把循环变量 i 改为用 let 声明即可
for(let i = 0; i < list.length; i++) {		
  list[i].onclick = function() {
    console.log(i + 1);
  }
}
```

这个解法是我学习 es6 之后才理解的，上次在网上再次搜这个面试题解法的时候看到了有人这么做的。思路是利用 es6 中 let 具有块级作用域的特点实现的。

### 5：js 事件委托
``` javascript
function handleClick (ev) {
  var ev = ev || window.event;
  var target = ev.target || ev.srcElement;
  console.log(target.id)
}
```

这个解法是上次别人请教我事件委托的问题，我给他讲解的过程中突然灵机一动想到了这个题目也是可以用事件委托来解答的。它主要是利用父级代理子级事件时，可以获取到子级 dom 对象。那么当然也可以获取到每个 li 的 id 属性了。

### 6：js 闭包
``` javascript
var lis = document.getElementsByTagName('li');
for (var i = 0; i < lis.length; i++) {
  (function(j){
    lis[j].addEventListener("click", function(e) {
      console.log(j + 1)
    }, false)
  })(i)
}
```

这个解法其实就算是高级写法了。是我上次想总结一下这个经典面试题所有解答方式的时候在网上看到别人是这么解的。我的理解得好像不是很透彻，有兴趣的可以研究下。

其实有很多经典的面试题，都是那种看似简单，实则暗藏玄机的考题。它不仅考察你技术上的灵活性，还考察了你知识上的系统性。我个人认为，其实都还是可以认真研究下的。

***

更多文章，可以访问: [我的csdn博客](http://blog.csdn.net/u014326381/article)