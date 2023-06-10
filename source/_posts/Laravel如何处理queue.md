---
title: Laravel如何处理queue
date: 2021-11-12 17:02:42
tags:
- Laravel
- PHP
---

queue内存泄漏如何处理，docker部署如何平滑关闭任务

<!--more-->

如果想要限制一个任务的内存，可以使用 –memory (默认值是128):

```
php artisan queue:work --memory=128
```

queue可以使用 docker化部署，supervisor管理，配置如下

```
[program:test-queue]
process_name=%(program_name)s_%(process_num)02d
command=php /var/www/artisan queue:work --queue=test --memory=100 --daemon
numprocs=8
autostart=true
autorestart=true
priority=3
stdout_events_enabled=true
stderr_events_enabled=true
stdout_logfile=/dev/stdout
stdout_logfile_maxbytes=0
stderr_logfile=/dev/stderr
stderr_logfile_maxbytes=0
stopsignal=SIGTERM
stopwaitsecs=600
```

docker内收到关闭信号，处理方式

```
#!/bin/sh

trap 'shutdown_all' SIGTERM

shutdown_all() {
  echo -e "\033[33m 关闭queue... \033[0m"
  
  #平滑停止supervisor管理的queue
  /usr/bin/supervisorctl -c /etc/supervisor/conf.d/supervisord.conf stop test-queue:*
  
  #这个是关闭一些没有通过supervisor管理的queue
  php artisan queue:restart
  echo -e "\033[31m queue进程已关闭... \033[0m"

}

/usr/bin/supervisord -c /etc/supervisor/conf.d/supervisord.conf &

# 等待  supervisor 进程停止
PID=$!
wait $PID
```

**需要注意的是，queue需要daemon模式运行，且php开启pcntl扩展**

### 特别说明:

```
最好同时将php的使用内存调高

假设queue设置溢出检测是 100MB

php设置的是默认值 128MB

脚本每次跑都需要 50MB

此时内存已经泄露了 80MB

那么下次执行的时候会触发OOM问题
```
