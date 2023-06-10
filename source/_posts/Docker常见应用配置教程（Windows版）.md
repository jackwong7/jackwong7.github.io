---
title: Docker常见应用配置教程（Windows版）
date: 2020-07-14 16:41:57
tags:
- Docker
---

##### 创建网络

```
docker network create somenetwork
```

##### 使用网络

```
docker run --net somework
```

<!--more-->

##### RabbitMQ

```
docker run -d  --name rabbitmq --restart always rabbitmq:3
```



##### mysql

```
docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d --restart always mysql:latest
```

##### redis

```
docker run --name redis -d -p 6379:6379 --restart always redis
```

##### Dnsmasq

```
docker run -d --name dnsmasq -p 53:53/udp -p 5380:8080 -v D:/environment/dns/dnsmasq/dnsmasq.conf:/etc/dnsmasq.conf --log-opt "max-size=100m" -e "HTTP_USER=foo" -e "HTTP_PASS=bar" --restart always jpillora/dnsmasq
```

##### elasticsearch

```
docker run -d -p 9200:9200 -p 9300:9300 --net default --name elasticsearch -e "discovery.type=single-node" -e "ES_JAVA_OPTS=-Xms512m -Xmx512m" -v D:/environment/es/data:/usr/share/elasticsearch/data --restart always elasticsearch:7.8.0
```

##### kibana

```
docker run -d -p 5601:5601 -e "I18N_LOCALE=zh-CN" --name kibana --link elasticsearch:elasticsearch --restart always kibana:7.8.0
```

