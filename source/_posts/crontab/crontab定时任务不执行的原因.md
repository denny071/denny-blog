---
title: crontab定时任务不执行的原因
date: 2020-11-12 14:41:28
tags: crontab
categories: tools
---
### 1 crond服务未启动

> crontab不是Linux内核的功能，而是依赖一个crond服务，这个服务可以启动当然也可以停止。如果停止了就无法执行任何定时任务了，解决的方法是打开它:

`crond`或`service crond start`

> 如果提示crond命令不存在，可能被误删除了，CentOS下可以通过这个命令重新安装：

`yum -y install crontabs`

### 2 权限问题

> 比如：脚本没有x执行权限，解决方法：
增加执行权限，或者用bash abc.sh的方法执行
也有可能crontab任务所属的用户对某个目录没有写权限，也会失败

### 3 路径问题

> 有的命令在shell中执行正常，但是在crontab执行却总是失败。有可能是因为crontab使用的sh未正确识别路径，比如：以root身份登录shell后执行一个/root/test.sh，只要执行

`./test.sh`

> 就可以了。但是在crontab中，就会找不到这个脚本，比如写完整：

`/root/test.sh`

### 4 时差问题

> 因为服务器与客户端时差问题，所以crontab的时间以服务器时间为准。
时差这个问题还真是搞人，这个我亲身体验了，现象如下：

(1) 我设置了一个定时脚本，用date命令观察服务器的时间到了脚本执行的时间点，发现没有执行

(2) 但是我把脚本设置成每分钟执行一次，就是OK的

> 见鬼了，服务器时间是对的啊？莫非是要加个什么时区？于是把脚本的时间减10或者12或者8个小时都尝试了下，发现都不行。
但是很明显是时间不一致导致的不执行。

### 5 变量问题
> 有时候命令中含有变量，但crontab执行时却没有，也会造成执行失败。