---
title: 圣杯布局 VS 双飞翼布局
date: 2017-03-30 09:54:50
tags: CSS 布局
---

作为前端开发者，布局都应该已经写了不少了，但很多时候我们实现布局的思路可能都不一定正确或者不是最合适的。要想自己的布局水平有所提高，两个经典的布局： 圣杯布局 和 双飞翼布局，我个人觉得还是很有必要掌握的。

<!--more-->

圣杯布局和双飞翼布局都是为了实现一种非常常见的布局：即两侧定宽，中间自适应的三列布局，特征是中间列要放在文档流前面以优先渲染。

既然都是为了解决同一个问题，那么有哪些不同呢？我们先来稍微回顾一下这两种布局，再加以比较。

## 圣杯布局
圣杯布局的来历是2006年发在a list part上的这篇文章：
 
[点击原文链接](http://alistapart.com/article/holygrail) 

html 部分：
``` html
<div id="container">
	<div id="main" class="col"> #main </div>
	<div id="left" class="col"> #left </div>
	<div id="right" class="col"> #right </div>
</div>
```
css 部分：
``` css
#container {
	padding-left: 200px;
	padding-right: 250px;
}
.col { 
	position: relative; 
	float: left; 
	height: 300px; 
}
#main {
	width: 100%;
	background-color: rgba(255, 0, 0, .5);
}
#left {
	width: 200px;
	margin-left: -100%;
	left: -200px;
	background-color: rgba(0, 255, 0, .5);
}
#right {
	width: 250px;
	margin-left: -250px;
	left: 250px;
	background-color: rgba(0, 0, 255, .5);
}
```
**大致实现思路：**三个 col 的容器 #container 一开始先空出左右两个地方，准备待会儿放左列和右列。三个 col 都设置 `float: left` 和 相对定位，然后左列 `margin-left: -100%`; 则左列跟中间主列左对齐了。这时左列相对定位加上 left 值是负的左列宽度，就飘到最左边了；右列同理，margin-left 值是负的自身宽度，这时右列跟中间主列右对齐了。右列也是相对定位的，left 值是自身宽度时，右列就飘到最右侧了。

## 双飞翼布局
双飞翼布局的概念始于淘宝UED，据说是玉伯提出的（网上看到的，未求证）。意思是：如果把三列布局比作一只大鸟，可以把中间的主列看成是鸟的身体，左右则是鸟的翅膀。这个布局的实现思路是，先把最重要的身体部分放好，然后再将翅膀移动到适当的地方。

html 部分：
``` html
<div id="main-wrapper" class="col">
  <div id="main"> #main </div>
</div>
<div id="left" class="col"> #left </div>
<div id="right" class="col"> #right </div>
```

css 部分：
``` css
.col { 
	float: left; 
	height: 300px; 
}
#main-wrapper {
	width: 100%;
}
#main {
	height: 300px;
	margin-left: 200px;
	margin-right: 250px;
	background-color: rgba(255, 0, 0, .5);
}
#left {
	width: 200px;
	margin-left: -100%;
	background-color: rgba(0, 255, 0, .5);
}
#right {
	width: 250px;
	margin-left: -250px;
	background-color: rgba(0, 0, 255, .5);
}
```
**大致实现思路：**在圣杯布局的 dom 基础上，给 中间列加了个容器，然后在容器里面左右各空出一个地方，准备待会儿放左列和右列。三个 col 都设置 `float: left`，然后左列 `margin-left: -100%`， 则左列就飘到最左边了；右列稍有不同，margin-left 值是负的自身宽度，这时右列就飘到最右侧了。

## 对比

圣杯布局
* 特征：主列先加载，统一浮动，统一相对定位，设置负边距
* 优点：不需要额外的 dom 节点
* 缺点：需要清除浮动，需要设置相对定位

双飞翼布局
* 特征：主列先加载，仅需统一浮动、负边距
* 优点：兼容性非常好，IE5.5 以上都支持
* 缺点：需要额外的 dom 节点

其实，实现这种经典的三列布局，还有另外两种思路：一种是圣杯布局的变种 —— 改用绝对定位来实现，一种是使用 flex 布局来实现。有兴趣的可以自己尝试下，这里我也简单的做出对比：

绝对定位
* 特征：按先后顺序加载，需要设置浮动、绝对定位、负边距
* 优点：无高度坍塌，没有破坏文档流，不需要清除浮动
* 缺点：浮动、绝对定位、负边距齐齐上阵，容易混淆

flex 布局
* 特征：容器 display 指为 flex，主列 flex 值为1，两侧宽度固定
* 优点：写法简洁，易理解（还可以改写成一个响应式的三列布局，分辨率是移动设备时两侧都出现在主列下边）
* 缺点：兼容性不好，需要加上各类浏览器前缀

总的来看，网页布局技巧不外乎：浮动、绝对定位 / 相对定位、负边距、flex 布局。能把这几个灵活的搭配用到一起，大部分的布局一般就都能实现了。选用最合适的方法来布局，不仅能帮你节省很多时间，还能提升不少页面的加载性能。

***

更多文章，可以访问: [我的csdn博客](http://blog.csdn.net/u014326381/article)