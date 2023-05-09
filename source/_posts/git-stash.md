---
title: git stash 和 git stash pop
date: 2017-06-17 11:54:18
tags: git
---

`git stash` 可以用来暂存当前正在进行的工作。

比如，你想 pull 最新代码，又不想加新 commit 的时候。

又或者另外一种情况，为了 fix 一个紧急的 bug，先 `git stash`, 使返回到自己上一个 commit，改完 bug 之后再 `git stash pop`，继续原来的工作。

<!--more--> 

基础命令：

``` bash
git stash
# do some work
git stash pop
```

进阶：

``` bash
git stash save "work in progress for foo feature"
```

当你多次使用 `git stash` 命令后，你的栈里将充满了未提交的代码，这时候你会对将哪个版本应用回来有些困惑，你可以输入：

``` bash
git stash list
```

将当前的 git 栈信息全都打印出来，你只需要将找到对应的版本号，例如使用

``` bash
git stash apply stash@{1}
```

就可以将你指定版本号为 stash@{1} 的工作取出来。当你将所有的栈都应用回来的时候，可以使用

``` bash
git stash clear
```

来将栈清空。

### git stash apply 和 git stash pop 区别

两者都可以把 git 栈里面的内容取出来，但 `git stash apply` 恢复后，stash 内容并不会被删除，除非你再用 git stash drop 来删除；

而用 git stash pop，它在恢复的同时把 stash 内容也删了。

### 相关命令

``` bash
git stash           # save uncommitted changes
git stash list      # list stashed changes in this git
git show stash@{0}  # see the last stash 
git stash pop       # apply last stash and remove it from the list
git stash apply     # apply last stash
git stash drop      # remove it from the list
git stash clear     # remove the whole git stash list
git stash --help    # for more info
```

参考资料：

* [廖雪峰 git 教程 - Bug分支](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137602359178794d966923e5c4134bc8bf98dfb03aea3000)
* [git 官方文档](https://git-scm.com/docs)

***

更多文章，可以访问: [我的csdn博客](http://blog.csdn.net/u014326381/article)



