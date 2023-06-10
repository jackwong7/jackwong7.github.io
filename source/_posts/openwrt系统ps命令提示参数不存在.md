---
title: openwrt系统ps命令提示参数不存在
date: 2021-07-06 16:58:27
tags:
- OpenWrt
---

最近使用openwrt查看进程时候，提示ps的参数不存在



```
➜  # ps axu | grep progname
ps: unrecognized option: a
BusyBox v1.31.1 () multi-call binary.
```

#### 解决方案：

安装 procps-ng-ps

```
if [ ! -x /bin/procps-ng-ps ]; then opkg update; opkg install procps-ng-ps; fi
```

<!--more-->
