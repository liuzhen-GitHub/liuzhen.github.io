---
title: tengine通过cygwin编译出windows版本安装包
tags: [tengine,cygwin]
categories: tengine
---

cygwin下载
https://cygwin.com/setup-x86_64.exe

tengine下载地址
http://tengine.taobao.org/download_cn.html

tengine编译windows版本时编译参数
./configure --builddir=objs --prefix=/cygdrive/c/nginx --conf-path=conf/nginx.conf --pid-path=logs/nginx.pid --http-log-path=logs/access.log --error-log-path=logs/error.log --sbin-path=nginx.exe --with-cc-opt="-D FD_SETSIZE=4096" && make && make install

对了，参数里的路径要用相对路径，否则编译出来的nginx在windows上运行路径要与编译环境的路径一致
我之前用的是tengine2.2.0的源码，这个参数对版本应该没有要求， 都是一些路径
编译是在cygwin上操作的

tengine2.2.1版本源码有问题，用最新代码编译没问题

Nginx源码学习之编译、构建与安装(cygwin环境)  包含注意事项
http://blog.csdn.net/cpucooler2011/article/details/21995595
