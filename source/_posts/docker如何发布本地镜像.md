---
title: docker如何发布本地镜像
date: 2023-07-28 23:18:34
tags:
- Dcoker
---

### 1. 登录到 Docker Registry

使用 `docker login` 命令登录 Registry 服务器。比如登录 Docker Hub,命令如下:

bash

Copy

```
docker login
```

会提示你输入用户名和密码。

<!--more-->

### 2. 为镜像打标签

给要推送的镜像打上仓库路径的标签:

bash

Copy

```
docker tag [Image ID] [用户名]/[镜像名]:[标签]
```

比如:

bash

Copy

```
docker tag 2e8ae03b1326 username/myimage:latest
```

### 3. 推送镜像

使用 `docker push` 命令推送镜像:

bash

Copy

```
docker push [用户名]/[镜像名]:[标签]
```

如:

bash

Copy

```
docker push username/myimage:latest
```

Docker 就会将镜像 push 到你登录的 Registry 服务器上。

### 4. 完结撤销镜像(可选)

成功推送镜像后,可以使用以下命令撤销镜像标签和本地镜像:

bash

Copy

```
docker rmi [镜像ID]
docker rmi [用户名]/[镜像名]:[标签]
```

以上是完整的推送 Docker 镜像到 Registry 的过程。



### 5. 例子

```bash
# 自行登录docker后, 在hub.docker.com里创建 jd-pro-pro 项目

# 给 {image id} 打标签
docker tag b1c4cdabffbc joy88000/jd-pro-pro:1.0.0

# 发布这个包
docker push joy88000/jd-pro-pro::latest
```

