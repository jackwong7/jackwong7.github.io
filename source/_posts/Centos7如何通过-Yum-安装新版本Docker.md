---
title: Centos7如何通过 Yum 安装新版本Docker
date: 2020-07-14 16:46:43
tags:
- Centos
- Docker
---

#### 首先设置yum源

```
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

<!--more-->

![img](Centos7如何通过 Yum 安装新版本Docker/47a25840c7895ff923a72f29487ecc0d.png)

#### 检查是否更新成功

```
yum list docker-ce --showduplicates | sort -r
```

![img](Centos7如何通过 Yum 安装新版本Docker/d237c67d50235cb738bf799f8875f1cb.png)

#### 安装docker，我这里就不指定版本了

```
yum install docker-ce -y
```

![img](Centos7如何通过 Yum 安装新版本Docker/6ee12e604f64bd852d53ac9e82bd6071.png)

#### 运行且设置开机启动

```
systemctl start docker && systemctl enable docker
```
