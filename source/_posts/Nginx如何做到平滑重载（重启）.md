---
title: Nginx如何做到平滑重载（重启）
date: 2020-07-14 16:40:18
tags:
- Nginx
---

检查nginx配置是否有语法 错误

```
/usr/local/tengine/sbin/nginx -t
```

平滑重启

```
nginx /usr/local/tengine/sbin/nginx -s reload
```

<!--more-->
