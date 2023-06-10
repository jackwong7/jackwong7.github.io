---
title: hyperf2.0以上热更新解决方案
date: 2023-04-23 17:07:58
tags:
- Hyperf
- PHP
---

我是windows开发环境, 使用了docker环境, ide 为phpstorm, 开发hyperf2.0项目的时候,配置了watch热更新, 但是在使用过程中总是有一些莫名其妙的报错,并且cpu占用较高

<!--more-->

```
错误有很多,比如:
PHP Fatal error:  Uncaught RuntimeException: The class reflector object does not init yet in /data/project/xxx/vendor/hyperf/di/src/BetterReflectionManager.php:39
......

或者循环输出一些内容:
[WARNING] Delete files must be restarted manually to take effect.
Class reload success.
Class reload success.
Class reload success.
Class reload success.
Class reload success.
Class reload success.
[WARNING] Delete files must be restarted manually to take effect.
Class reload success.
Class reload success.
Class reload success.
Class reload success.
Class reload success.
```

我的watcher配置如下

```
return [
    'driver' => ScanFileDriver::class,
    'bin' => 'php',
    'watch' => [
        'dir' => ['app', 'config'],
        'file' => ['.env'],
        'scan_interval' => 2000,
    ],
];
```

经过长达几个小时的折腾后依旧无法解决问题, 最终搜到一个文章, 解决了我的问题,我把driver改成了 `FindNewDriver` 不仅仅cpu占用率下去了,问题也解决了 非常感谢大佬的付出

[作者: windawake]: hyperf2.0以上热更新解决方案（适用于linux系统） | PHP 技术论坛 (learnku.com)(https://learnku.com/articles/47820)
