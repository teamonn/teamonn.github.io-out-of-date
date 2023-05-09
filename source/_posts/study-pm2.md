---
title: PM2 从入门到“放弃”
date: 2017-9-18 16:23:31
tags: 后端
---

> “世上无难事，只要肯放弃”。 ：）

本文主要是记录笔者学习PM2后的一种知识回顾和梳理，而不是入门教程。（入门资料见文章底部）

### 简介
pm2 是一个Node应用的进程管理器。

<!--more-->

你可以把它理解成是一种命令行管理工具，利用它可以简化很多node应用管理的繁琐任务。

### 特性
1. 内建负载均衡（使用 Node cluster 集群模块，可参考朴灵的《深入浅出node.js》第九章）
2. 后台运行
3. 0秒停机重载，即 允许你重新载入代码而不用失去请求连接
4. 跨平台支持，具有 Ubuntu 和 CentOS 的启动脚本
5. 停止不稳定的进程（避免无限循环）
6. 控制台性能检测
7. 提供 HTTP API
8. 远程控制和实时的接口 API ( Nodejs 模块，允许和 PM2 进程管理器交互 )

### 常用用法
``` bash
$ npm install pm2 -g     # 命令行安装 pm2 
$ pm2 start app.js -i 4  # 后台运行pm2，启动4个app.js (负载均衡)
                         # 也可以把'max' 参数传递给 start
                         # 正确的进程数目依赖于Cpu的核心数目
$ pm2 start app.js --name my-api    # 命名进程
$ pm2 start app.json     # 启动进程, 在app.json里设置选项（启动的参数说明下面会提到）
$ pm2 list               # 显示所有进程状态
$ pm2 monit              # 监视所有进程的CPU和内存的实时使用情况
$ pm2 logs               # 显示所有进程日志
$ pm2 stop all           # 停止所有进程
$ pm2 restart all        # 重启所有进程
$ pm2 reload all         # 0秒停机重载进程 (用于 NETWORKED 进程)
$ pm2 describe 0         # 常看指定应用详细的运行状态
$ pm2 stop 0             # 停止指定的进程
$ pm2 restart 0          # 重启指定的进程
$ pm2 startup            # 产生 init 脚本 保持进程活着
$ pm2 web                # 开启api访问 (浏览器访问 http://localhost:9615)
$ pm2 delete 0           # 杀死指定的进程
$ pm2 delete all         # 杀死全部进程
```

### 启动参数
``` bash
$ pm2 start app.js <参数>
```
参数列表
* ---watch：监听应用目录的变化，一旦发生变化，自动重启。如果要精确监听、不见听的目录，最好通过配置文件。
* -i ---instances：启用多少个实例，可用于负载均衡。如果-i 0或者-i max，则根据当前机器核数确定实例数目。
* ---ignore-watch：排除监听的目录/文件，可以是特定的文件名，也可以是正则。比如--ignore-watch="test node_modules "some scripts""
* -n ---name：应用的名称。查看应用信息的时候可以用到。
* -o --output <path>：标准输出日志文件的路径。
* -e --error <path>：错误输出日志文件的路径。
* ---interpreter <interpreter>：the interpreter pm2 should use for executing app (bash, python...)。比如你用的coffee script来编写应用。

### 结语
学习，是一场孤独的旅行。
但我相信，每个忠于自己爱好和兴趣的人，一定能走到最后。

***

参考资料:
1- [PM2介绍 - 怪石羽笺](https://www.douban.com/note/314200231/)
2- [PM2实用入门指南 - IMWeb社区](http://imweb.io/topic/57c8cbb27f226f687b365636)
3- [使用pm2躺着实现负载均衡](http://blog.csdn.net/qq_17475155/article/details/53823862)

