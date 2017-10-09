---
title: 闲话 git merge 与 git rebase 的区别
date: 2017-03-12 17:35:08
tags: git
---

最近换工作，面试碰到一次笔试考这两个的区别。开始学习git的时候，这两个的区别是有了解过的。但是时间长了加上之前公司应用得少了，所以当时记得很模糊。笔试完就赶紧回家整理了下，希望可以借此加深印象。

<!--more--> 

## merge
如果一开始我们的分支情况如下图，有一个主分支 master 及一个开发分支 deve：

![（SourceTree中的分支截图）](http://img.blog.csdn.net/20170312161607281?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDMyNjM4MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

假定我们目前处在 master 主分支上，在该项目根目录路径下输入命令：

``` bash
$ git merge deve
```

Git 系统会以两个分支的共同祖先 e381a81 为基础，将两个分支的最新提交 8ab7cff 和 696398a 进行三方合并（分支路径没有合并），然后将合并中修改的内容生成一个新的 commit，即下图的 78941cb：

![这里写图片描述](http://img.blog.csdn.net/20170312163257132?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDMyNjM4MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## rebase
假如我们初始的分支情况如下图：

![这里写图片描述](http://img.blog.csdn.net/20170312163606713?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDMyNjM4MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

我们还是处在 master 分支上，我们在该项目根目录路径下输入命令：

``` bash
$ git rebase deve
```

这时 Git 系统会将 master 中没有 deve 的部分 bcde65c 和 35b6708 都合并进 master，这个过程是不会修改 deve 分支的。此时两个分支暂时合并成一根线，如下图所示：

![这里写图片描述](http://img.blog.csdn.net/20170312163801560?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDMyNjM4MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## merge 和 rebase 的区别对比：

对比项    | merge                | rebase
-------- | -------------------- | ----
操作过程 | 将两个分支的修改信息合并 | 将目标分支中有而当前没有的提交都加进当前分支，但不修改目标分支
分支路径 | 多条，各自显示，此时交叉了 | 单条，合在一起显示
历史版本信息 | 忠实反映各分支实际发生过什么 | 只反映项目过程中发生过什么
冲突处理 | 遇见冲突后会直接停止，等待手动解决冲突并重新提交 commit 后，才能再次 merge | 遇见冲突后会暂停当前操作，开发者可以选择手动解决冲突，然后 git rebase --continue 继续，或者 --skip 跳过（注意此操作中当前分支的修改会直接覆盖目标分支的冲突部分），亦或者 --abort 直接停止该次 rebase 操作

## 辅助记忆

可以类比同学 a 和同学 b 考试对答案的场景，来区别 merge 和 rebase。

* **a merge b** => 等同 a 和 b 相互对答案，觉得对方对或者对方做了自己没做的就修改过来。最终和他们各自开始的答案相比，他们的答案都修改了（不考虑开始就答案相同的情况）。

* **a rebase b** => 等同 a 抄 b 的答案，觉得 b 对或者 b 做了自己没做的就按照他的抄过来。最终只有 a 的答案修改了，b 的答案不会被修改。

参考文档：[点击进入 git 官网](https://git-scm.com/docs)

***

更多文章，可以访问: [我的csdn博客](http://blog.csdn.net/u014326381/article)
