---
title: linxu系统运行程序提示_GLIBC_2.2_not_found
date: 2021-12-04 17:03:59
tags:
- Linux
---

./server_amd64: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.28’ not found (required by ./server_amd64)

<!--more-->

原因是系统的glibc版本太低，软件编译时使用了较高版本的glibc引起的：

查看系统glibc支持的版本：

```
strings /lib/x86_64-linux-gnu/libc.so.6 |grep GLIBC_
```

glibc是GNU发布的libc库，即c运行库。glibc是linux系统中最底层的api，几乎其它任何运行库都会依赖于glibc。glibc除了封装linux操作系统所提供的系统服务外，它本身也提供了许多其它一些必要功能服务的实现。由于 glibc 囊括了几乎所有的 UNIX 通行的标准，可以想见其内容包罗万象

```
sudo apt-get install libc6

sudo apt-get install libc6-dev

Ubuntu18.04查看glibc的版本号
```

jiang@jiang-Ubuntu:~~$ ls -l /lib/x86_64-linux-gnu/libc.so.6
lrwxrwxrwx 1 root root 12 8月 18 13:01 /lib/x86_64-linux-gnu/libc.so.6 -> libc-2.27.so
jiang@jiang-Ubuntu:~~$ ldd –version
ldd (Ubuntu GLIBC 2.27-3ubuntu1.2) 2.27
Copyright (C) 2018 自由软件基金会。
这是一个自由软件；请见源代码的授权条款。本软件不含任何没有担保；甚至不保证适销性
或者适合某些特殊目的。
由 Roland McGrath 和 Ulrich Drepper 编写。

编译安装glibc
首先从gnu官网下载最新版的glibc，地址http://www.gnu.org/software/libc/
http://ftp.gnu.org/gnu/libc/

```
sudo apt-get install gawk

tar -zxvf glibc-2.32.tar.gz

cd glibc-2.32

mkdir build

cd build


../configure --prefix=/usr/local
```

编译 glibc 过程中报错:These critical programs are missing or too old: bison

首先查看bison 版本 bison –version sudo apt install bison

```
../configure --prefix=/usr/local --disable-sanity-checks

make -j8

make clean

make install
```
