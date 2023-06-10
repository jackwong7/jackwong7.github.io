---
title: 'beego提示 gob: name not registered for interface'
date: 2020-07-14 16:48:25
tags:
- Golang
- Beego
---

很多初学者在学习beego涉及到更改session时，很容易犯下面这个错误

```
gob: name not registered for interface: "github.com/jackwong7/beego_blog/models.User"
```



解决方案也很简单，需要在 main.go 中注册一下 models.User (上面报错的这个类型)

```
gob.Register(models.User{})
```

<!--more-->
