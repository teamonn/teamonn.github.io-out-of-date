---
title: 回顾盒子模型之 box-size 属性
date: 2017-02-20 10:03:59
tags: css 盒子模型
---

盒子模型是CSS中很重要也很常见的概念，它涉及到的无非就是内容(content)、填充(padding)、边框(border)、边界(margin)。但是真要你把这几个的关系说清楚，相信不少人会掉坑。因为很多人没注意到的是，盒子模型其实有两种类型：标准盒模型和IE盒模型。

<!--more-->

## 标准盒模型

``` css
box-sizing: content-box;
```

网上盗了张图：

![这里写图片描述](http://img.blog.csdn.net/20170312191531615?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDMyNjM4MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

由上图可以看出，标准盒模型的 width 组成：content（不包含 padding 和 border）

比如：你给一个div的宽度设为200px，那这个div实际的宽度其实是200px再加上padding和border的值（和是大于等于200px的）。

## IE盒模型

``` css
box-sizing: border-box;
```

网上又盗了张图：

![这里写图片描述](http://img.blog.csdn.net/20170312191552052?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDMyNjM4MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

由上图可以看出，IE盒模型的 width 组成：content + 2 \* padding + 2 \* border

比如：你给一个div的宽度设为200px，那这个div内容的宽度其实只有200px减去 padding 和 border 的值。它实际宽度是小于或等于200px的。

## 对比总结
1. box-sizing: content-box | border-box | inherit
2. 他们的主要区别其实就是 width 包不包含 border 和 padding
3. box-sizing 默认值是 content-box，即默认是标准盒子模型

***

更多文章，可以访问: [我的csdn博客](http://blog.csdn.net/u014326381/article)
