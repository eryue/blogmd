---
title: Linux下端口被占用解决
date: 2016-09-19
categories: technology
tags: linux
---

* 查找备用的端口
    * netstat -tln 查看端口使用情况，而netstat -tln \| grep 端口号 则是只查看指定端口的使用情况
* 查看端口属于哪个程序
    * lsof -i :端口号
* 杀掉占用端口的进程
    * kill -9 进程id
	

[Linux netstat命令详解](http://www.cnblogs.com/ggjucheng/archive/2012/01/08/2316661.html)
