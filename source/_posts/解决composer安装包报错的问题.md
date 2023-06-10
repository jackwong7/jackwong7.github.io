---
title: 解决composer安装包报错的问题
date: 2020-09-15 16:56:56
tags:
- PHP
- Composer
---

### 今天执行compoer安装扩展时一直报下面这个错误

```
[RuntimeException]
Could not load package mews/purifier in https://packagist.phpcomposer.com: [UnexpectedValueException] Could not parse version constraint ~4.*: Invalid version string "~4.*"

[UnexpectedValueException]
Could not parse version constraint ~4.*: Invalid version string "~4.*"
```

<!--more-->

### 解决方案

Using this command solved the problem:

```
composer self-update
```

or if you don’t have the permission:

```
sudo composer self-update
```
