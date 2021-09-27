---
layout:     post   				        # 使用的布局（不需要改）
title:      Go 语言快速入门 				# 标题 
subtitle:   ZX觉得这个能一天看完？			# 副标题
date:       2021-09-26				    # 时间
author:     YXWang 					    # 作者
header-img: img/post-bg-keybord.jpg 	# 这篇文章的标题背景图片
catalog: true 						    # 是否归档
tags:								    # 标签
    - 施工中
    - Go语言
---

# 【施工中】Go 语言快速入门

这里现在只有标题，主要是让我记起来“这玩意有啥特点”，内容慢慢填吧，坑了（



### 并发和获取 URL



### Web 服务



### 简短变量声明 `:=` 



### 变量的生命周期和垃圾回收



### 空白标识符 `_`



### Go 的包和文件



### Slice 



### Map



### JSON



### 文本和HTML模板



### 递归



### 多返回值的函数



### 错误处理



### defer 语句



### panic 异常和 recover 捕获异常



### Go 语言的接口



## 并发

### goroutine

goroutine 是一种函数的并发执行方式；

```go
func main() {
    go spinner(100 * time.Millisecond)
    const n = 45
    fibN := fib(n) // slow
    fmt.Printf("\rFibonacci(%d) = %d\n", n, fibN)
}

func spinner(delay time.Duration) {
    for {
        for _, r := range `-\|/` {
            fmt.Printf("\r%c", r)
            time.Sleep(delay)
        }
    }
}

func fib(x int) int {
    if x < 2 {
        return x
    }
    return fib(x-1) + fib(x-2)
}
```





### channel

channel 用来在 goroutine 之间进行参数传递



### 例：创建一个线上网站的本地镜像



### 互斥锁 `sync.Mutex`

 



### 读写锁 `sync.RWMutex`

允许多个只读操作并行执行，但写操作会完全互斥。这种锁叫作“多读单写”锁（multiple readers, single writer lock）



### 延迟初始化 `sync.Once`

