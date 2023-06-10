---
title: Docker清理
date: 2021-08-11 17:01:24
tags:
- Docker
---

随着业务量越来越多，docker占用的空间也越来越多了，可以用下面的命令清理一下空间

<!--more-->

```
docker命令
清理日志
sudo sh -c "du -ch /var/lib/docker/containers/*/*-json.log"
sudo sh -c "truncate -s 0 /var/lib/docker/containers/*/*-json.log"

清理镜像
docker image prune -f -a

清理容器
docker container prune -f 
```
