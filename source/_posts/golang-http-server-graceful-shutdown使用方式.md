---
title: golang http server graceful shutdown使用方式
date: 2022-07-27 17:06:16
tags:
- Golang
---

### goalng 1.8后支持平滑关闭http server,使用方法如下

<!--more-->

```
package main

import (
	"context"
	"log"
	"net/http"
	"os"
	"os/signal"
	"syscall"
	"time"
)

func main() {
	mux := http.NewServeMux()
	mux.HandleFunc("/hello", hello)
	server := &http.Server{
		Addr:    ":8080",
		Handler: http.TimeoutHandler(mux, 20*time.Second, "Timeout!"),
		//ReadTimeout:  2 * time.Second,
		//WriteTimeout: 2 * time.Second,
	}

	// http服务
	go func() {
		log.Println("HTTP服务启动", "http://localhost"+server.Addr)
		if err := server.ListenAndServe(); err != nil && err != http.ErrServerClosed {
			log.Println(err)
			os.Exit(0)
		}
		log.Println("HTTP服务关闭请求")
	}()

	// 监听信号 优雅退出http服务
	Watch(func() error {
		ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
		defer cancel()
		return server.Shutdown(ctx)
	})

	log.Println("程序退出")
}

// 监听信号
func Watch(fns ...func() error) {
	// 程序无法捕获信号 SIGKILL 和 SIGSTOP （终止和暂停进程），因此 os/signal 包对这两个信号无效。
	ch := make(chan os.Signal, 1)
	signal.Notify(ch, syscall.SIGHUP, syscall.SIGINT, syscall.SIGTERM, syscall.SIGQUIT, syscall.SIGUSR1, syscall.SIGUSR2)

	// 阻塞
	s := <-ch
	close(ch)
	log.Println("接收到信号", s.String(), "执行关闭函数")
	for i := range fns {
		if err := fns[i](); err != nil {
			log.Println(err)
		}
	}
	//time.Sleep(30 * time.Second)
	log.Println("关闭函数执行完成")
}

func hello(w http.ResponseWriter, r *http.Request) {
	time.Sleep(time.Second * 5)
	log.Println("http请求执行完成")
	w.Write([]byte("hello"))
}
```
