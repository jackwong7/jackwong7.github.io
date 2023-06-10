---
title: Laravel 队列执行频率限制
date: 2020-07-14 16:56:10
tags:
- Laravel
- PHP
---

说明：这是 laravel 官方提供的办法，我写出来是因为我在百度和必应都搜不到正确答案
官方文档：队列《Laravel 5.8 中文文档》
laravel 5.5+

<!--more-->

#### 每分钟限制执行 10 次 JOB（注意是 JOB，而不是整个队列）

```
use Illuminate\Support\Facades\Redis;
$reisThrottle = Redis::throttle('key')->allow(10)->every(60);
$reisThrottle->timeout = 0;//默认超时时间是3s 会影响业务处理速度
$reisThrottle->then(function () {
    // 任务逻辑...
}, function () {
    // 无法获得锁...

    return $this->release(10);
});
```

#### 并发，限制同一时间只执行一个 JOB

```
Redis::funnel('key')->limit(1)->then(function () {
    // 任务逻辑...
}, function () {
    // 无法获得锁...

    return $this->release(10);
});
```

**方法里的参数 key 是自定义的 redis key，如果需要多个 job 共用一个限制，则可以使用同一个 key**
