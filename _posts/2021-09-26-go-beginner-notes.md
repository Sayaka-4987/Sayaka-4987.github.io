---
layout:     post   				        # 使用的布局（不需要改）
title:      Go 语言快速入门 				# 标题 
subtitle:   ZX 觉得这个能一天看完，但我啥也没记住		# 副标题
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



### 参考资料

- [Golang 标准库文档](https://studygolang.com/pkgdoc) 
- [Go语言 Leetcode 题解](https://books.halfrost.com/leetcode/) 
- [Go语言圣经《The Go Programming Language》](https://books.studygolang.com/gopl-zh/)  
- [Go语言高级编程《Advanced Go Programming》](https://chai2010.cn/advanced-go-programming-book/) 



## 0. 入门例程

### 并发和获取 URL

```go
// Fetchall fetches URLs in parallel and reports their times and sizes.
package main

import (
    "fmt"
    "io"
    "io/ioutil"
    "net/http"
    "os"
    "time"
)

func main() {
    start := time.Now()
    ch := make(chan string)
    for _, url := range os.Args[1:] {
        go fetch(url, ch) // start a goroutine
    }
    for range os.Args[1:] {
        fmt.Println(<-ch) // receive from channel ch
    }
    fmt.Printf("%.2fs elapsed\n", time.Since(start).Seconds())
}

func fetch(url string, ch chan<- string) {
    start := time.Now()
    resp, err := http.Get(url)
    if err != nil {
        ch <- fmt.Sprint(err) // send to channel ch
        return
    }
    nbytes, err := io.Copy(ioutil.Discard, resp.Body)
    resp.Body.Close() // don't leak resources
    if err != nil {
        ch <- fmt.Sprintf("while reading %s: %v", url, err)
        return
    }
    secs := time.Since(start).Seconds()
    ch <- fmt.Sprintf("%.2fs  %7d  %s", secs, nbytes, url)
}
```



### Web 服务

```go
// Server2 is a minimal "echo" and counter server.
package main

import (
    "fmt"
    "log"
    "net/http"
    "sync"
)

var mu sync.Mutex
var count int

func main() {
    http.HandleFunc("/", handler)
    http.HandleFunc("/count", counter)
    log.Fatal(http.ListenAndServe("localhost:8000", nil))
}

// handler echoes the Path component of the requested URL.
func handler(w http.ResponseWriter, r *http.Request) {
    mu.Lock()
    count++
    mu.Unlock()
    fmt.Fprintf(w, "URL.Path = %q\n", r.URL.Path)
}

// counter echoes the number of calls so far.
func counter(w http.ResponseWriter, r *http.Request) {
    mu.Lock()
    fmt.Fprintf(w, "Count %d\n", count)
    mu.Unlock()
}
```



## 1. 基本写法、结构

Go 语言推荐驼峰命名；



### `var` 声明变量

### `const` 声明常量

### `func` 声明函数实体对象

```go
var i, j, k int                  // int, int, int
var b, f, s = true, 2.3, "four"  // bool, float64, string
var f, err = os.Open(name)       // 多个返回值初始化，os.Open() 也许返回一个文件，也许是 error
```



### 简短变量声明 `:=` 

简短变量声明语句中必须 **至少要声明一个新的变量**  

```go
f, err := os.Open(name)
if err != nil {
    return err
}
// ...use f...
f.Close()
```



### 指针

和 C/C++ 差不多；

任何类型的指针的零值都是 `nil`；

```go
x := 1
p := &x         // p, of type *int, points to x
fmt.Println(*p) // "1"
*p = 2          // equivalent to x = 2
fmt.Println(x)  // "2"
```



#### `new(T)` 函数

表达式 new(T) 创建一个T类型的匿名变量，初始化为T类型的**零值**，返回变量地址，指针类型为`*T` 

```go
p := new(int)    // p, *int 类型, 指向匿名的 int 变量
fmt.Println(*p)  // "0"
*p = 2           // 设置 int 匿名变量的值为 2
fmt.Println(*p)  // "2"
```



### 变量赋值

```go
x = 1                       // 命名变量的赋值
*p = true                   // 通过指针间接赋值
person.name = "bob"         // 结构体字段赋值
count[x] = count[x] * scale // 数组、slice或map的元素赋值，也可以 count[x] *= scale
i, j, k = 2, 3, 5
a[i], a[j] = a[j], a[i]     // 元组赋值语句，交换变量的值

// 求最大公约数
func gcd(x, y int) int {
    for y != 0 {
        x, y = y, x%y
    }
    return x
}

// 计算斐波纳契数列的第N个数
func fib(n int) int {
    x, y := 0, 1
    for i := 0; i < n; i++ {
        x, y = y, x+y
    }
    return x
}
```



Go 的自增和自减是语句，而不是表达式

```go
v++      // 等价方式 v = v + 1；v 变成 2
v--      // 等价方式 v = v - 1；v 变成 1
x = i++  // 错误！
```



### 会产生多个值的表达式

map查找、类型断言、或通道接收出现在赋值语句的右边时，并不一定产生两个结果，也可能只产生一个结果；

对于只产生一个结果的情形：

- map查找失败时会返回零值；
- 类型断言失败时会发生运行时panic异常；
- 通道接收失败时会返回零值（阻塞不算是失败）

```go
v, ok = m[key]             // map lookup
v, ok = x.(T)              // type assertion
v, ok = <-ch               // channel receive

v = m[key]                // map查找，失败时返回零值
v = x.(T)                 // type断言，失败时panic异常
v = <-ch                  // 管道接收，失败时返回零值（阻塞不算是失败）

_, ok = m[key]            // map返回2个值
_, ok = mm[""], false     // map返回1个值
_ = mm[""]                // map返回1个值
```



### 下划线空白标识符 `_` 

可以丢弃不需要的值

```go
_, err = io.Copy(dst, src) // 丢弃字节数
_, ok = x.(T)              // 只检测类型，忽略具体值
```



### `type` 声明类型

```go
package tempconv

import "fmt"

type Celsius float64    // 摄氏温度
type Fahrenheit float64 // 华氏温度

const (
    AbsoluteZeroC Celsius = -273.15 // 绝对零度
    FreezingC     Celsius = 0       // 结冰点温度
    BoilingC      Celsius = 100     // 沸水温度
)

func CToF(c Celsius) Fahrenheit { return Fahrenheit(c*9/5 + 32) }

func FToC(f Fahrenheit) Celsius { return Celsius((f - 32) * 5 / 9) }
```



### 作用域

声明语句的作用域：

- 对应的是一个源代码的文本区域；
- 是一个编译时的属性

变量的生命周期：

- 指程序运行时变量存在的有效时间段，在此时间区域内它可以被程序的其他部分引用；
- 是一个运行时的概念。



Go语言的习惯是在if中处理错误然后直接返回；



### 变量的生命周期和垃圾回收

Go 语言有自动的内存回收机制；





## 数据类型



### Go 的包和文件



### Slice 



### Map

Go 没提供查询一个key在不在map中的办法，但是可以利用方便的变量赋值写成： 

```go
// _, ok := map[key];语句查找成功返回两个值，ok=true，查找失败时返回零值，ok=false
// ok 的布尔值是 if 的最终结果 
if _, ok := map[key]; ok {
	// 存在
}
```



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

