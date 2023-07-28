---
title: docker如何本地生成且信任该证书
date: 2023-07-28 23:16:19
tags:
- Docker
---

我这里以Dockerfile举例

```dockerfile
# 创建一个 apis.imooc.com 的, 10年证书
RUN mkdir /app/ssl \
    && openssl req -x509 -newkey rsa:4096 -nodes -keyout /app/ssl/server.key -out /app/ssl/server.crt \
    -subj "/C=US/ST=California/L=San Francisco/O=Example Org/OU=Example/CN=apis.imooc.com" \
    -days 3650

# 将自签名证书添加到 CA 列表中
RUN cp /app/ssl/server.crt /usr/local/share/ca-certificates/ && \
    update-ca-certificates
```

