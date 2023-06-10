---
title: go语言ide不能提示第三方包的解决方案
date: 2020-07-14 16:26:31
tags:
- Goland
---

#### 非go mod模式的解决方案:

1. 把包放到go目录

2. 首先确认项目的包与第三方的包都在 $GOPATH/src 目录下

3. 然后 Goland Ide 打开设置，在GO >> GOPATH >> Index entire GOPATH打上勾

   <!--more-->
