---
title: windows下交叉编译 cgo for linux
date: 2021-07-04 16:57:44
tags:
- Docker
---

#### 问题：

windows开发机上编译不了linux 平台arm64，amd64的包，因为cgo无法跨平台编译

<!--more-->

#### 解决方案：

##### ARM64:

1. 通过docker容器来实现交叉编译

2. dockerfile内容如下

   ```
   FROM golang:1.13-buster
   LABEL os=linux
   LABEL arch=arm64
   
   ENV GOOS=linux
   ENV GOARCH=arm64
   ENV CGO_ENABLED=1
   ENV CC=aarch64-linux-gnu-gcc
   ENV PATH="/go/bin/${GOOS}_${GOARCH}:${PATH}"
   ENV PKG_CONFIG_PATH=/usr/lib/aarch64-linux-gnu/pkgconfig
   
   # install build & runtime dependencies
   RUN dpkg --add-architecture arm64 \
       && apt update \
       && apt install -y --no-install-recommends \
           protobuf-compiler \
           upx \
           gcc-aarch64-linux-gnu \
           libc6-dev-arm64-cross \
           pkg-config \
           libsamplerate0:arm64 \
           libsamplerate0-dev:arm64 \
           libopusfile0:arm64 \
           libopusfile-dev:arm64 \
           libopus0:arm64 \
           libopus-dev:arm64 \
           libportaudio2:arm64 \
           portaudio19-dev:arm64 \
       && rm -rf /var/lib/apt/lists/*
   
   # install build dependencies (code generators)
   RUN GOARCH=amd64 go get github.com/gogo/protobuf/protoc-gen-gofast \
       && GOARCH=amd64 go get github.com/GeertJohan/go.rice/rice \
       && GOARCH=amd64 go get github.com/micro/protoc-gen-micro
   ```

3. 构建并编译，挂载我的项目目录到容器里面

```
docker build -t gobuilder_arm64 .
docker run --rm -it -v D:/wwwroot/go/server:/usr/src/app -w /usr/src/app gobuilder_arm64 go build -o myapp_arm64
```

1. 如果arm64机器运行时提示缺少so库，需要自行拷贝容器里面的过去

##### AMD64:

1. 通过docker容器来实现交叉编译

2. dockerfile内容如下

   ```
   FROM golang:1.13-buster
   LABEL os=linux
   LABEL arch=amd64
   
   ENV GOOS=linux
   ENV GOARCH=amd64
   ENV CGO_ENABLED=1
   ENV PATH="/go/bin/${GOOS}_${GOARCH}:${PATH}"
   ```

3. 构建并编译，挂载我的项目目录到容器里面

```
docker build -t gobuilder_amd64 .
docker run --rm -it -v D:/wwwroot/go/server:/usr/src/app -w /usr/src/app gobuilder_amd64 go build -o myapp_amd64
```

1. 如果arm64机器运行时提示缺少so库，需要自行拷贝容器里面的过去
