---
title: alpine无法使用go
date: 2023-07-28 23:13:08
tags:
- GO
- Linux
---

我的alpine版本是3.17

装好了go,一直无法启动,提示命令不存在, 其实是, go在alpine系统采用的so不同导致的

执行下面命令即可

```bash
echo "https://dl-cdn.alpinelinux.org/alpine/edge/main" >> /etc/apk/repositories && apk add gcompat
```

