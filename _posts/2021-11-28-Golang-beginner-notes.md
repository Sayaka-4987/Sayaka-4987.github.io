---
layout:     post                           # 使用的布局（不需要改）
title:      Go 语言快速入门                 # 标题 
subtitle:   ZX 觉得这个能一天看完，但我啥也没记住，只好再看一遍        # 副标题
date:       2021-11-28                    # 时间
author:     YXWang                         # 作者
header-img: img/post-bg-keybord.jpg     # 这篇文章的标题背景图片
catalog: true                             # 是否归档
tags:                                    # 标签
    - 施工中
    - Go语言
---

# Go 语言快速入门

这里现在很多东西只有标题，主要是让我记起来“Golang这玩意有啥特点”，内容慢慢填吧，坑了（

### 参考资料

- [Go 语言中文网](https://studygolang.com/tag/%E5%AE%98%E6%96%B9%E7%BD%91%E7%AB%99)
- [Golang 标准库文档](https://studygolang.com/pkgdoc) 
- [Go语言 Leetcode 题解](https://books.halfrost.com/leetcode/) 
- [7 天用 Go 从零实现 Web 框架 Gee 教程（7days-golang）](https://geektutu.com/post/gee.html)
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

Go 没有 `?:` 三元运算符；

在这里一个变量/包声明了不用是会报错的，所以不要浪费；

### `var` 声明变量

```go
var i, j, k int                  // int, int, int
var b, f, s = true, 2.3, "four"  // bool, float64, string
var f, err = os.Open(name)       // 多个返回值初始化，os.Open() 也许返回一个文件，也许是 error
```

### `const` 声明常量

常量的运算必须可以在编译期完成

常量可以没有显式类型，常量的形式将隐式决定变量的默认类型

```go
const (
    e  = 2.71828182845904523536028747135266249775724709369995957496696763
    pi = 3.14159265358979323846264338327950288419716939937510582097494459
)
```

#### iota 常量生成器

生成一组以相似规则初始化的常量，而不必每行都写一遍初始化表达式

```go
type Weekday int

const (
    Sunday Weekday = iota // = 0, 1, 2 ...
    Monday
    Tuesday
    Wednesday
    Thursday
    Friday
    Saturday
)

type Flags uint

const (
    FlagUp Flags = 1 << iota // is up
    FlagBroadcast            // supports broadcast access capability
    FlagLoopback             // is a loopback interface
    FlagPointToPoint         // belongs to a point-to-point link
    FlagMulticast            // supports multicast access capability
)

const (
    _ = 1 << (10 * iota)
    KiB // 1024
    MiB // 1048576
    GiB // 1073741824
    TiB // 1099511627776             (exceeds 1 << 32)
    PiB // 1125899906842624
    EiB // 1152921504606846976
    ZiB // 1180591620717411303424    (exceeds 1 << 64)
    YiB // 1208925819614629174706176
)
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

// 一般i为下标，c为内容
for i, c := range s {
    if m[int(c)] == 1 {
        return i
    }
}
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

Go语言的习惯是在 if 中处理错误然后直接返回；

```go
f, err := os.Open(fname)
if err != nil {
    return err
}
f.ReadByte()
f.Close()
```

### 变量的生命周期和垃圾回收

Go 语言有自动的内存回收机制；

## 2. 数据类型

### 数据类型注意事项

- `len()` 返回的是有符号的 int
- 浮点数有 `float32` 和 `float64`  
- 浮点数到整数的转换将丢失小数部分，向数轴零方向截断
- 布尔型有 `true` 和 `false` 
- 字符串存UTF8编码的Unicode，越界会 panic
  - 字符串的值是不可变的

<img src="https://books.studygolang.com/gopl-zh/images/ch3-04.png" style="zoom:80%;" />

- 复数有 `complex64` 和 `complex128` 

```go
var x complex128 = complex(1, 2) // 1+2i
var y complex128 = complex(3, 4) // 3+4i
fmt.Println(x*y)                 // "(-5+10i)"
fmt.Println(real(x*y))           // "-5"
fmt.Println(imag(x*y))           // "10"
```

### 类型转换写法 T(x)

```go
f := 3.141 // a float64
i := int(f)
fmt.Println(f, i) // "3.141 3"
f = 1.99
fmt.Println(int(f)) // "1"

var u uint8 = 255
fmt.Println(u, u+1, u*u) // "255 0 1"

var i int8 = 127
fmt.Println(i, i+1, i*i) // "127 -128 1"
```

### 位运算操作符

| 运算符  | 作用          |
| ---- | ----------- |
| &    | 位运算 AND     |
| \|   | 位运算 OR      |
| ^    | 位运算 XOR     |
| &^   | 位清空 AND NOT |
| <<   | 左移          |
| \>\> | 右移          |

### `fmt.Printf()` 格式化输出

- `[1]` 副词告诉Printf函数再次使用第一个操作数；
- `#` 副词告诉Printf在用 %o、%x、%X 输出时带有 0、0x、0X 前缀；
- `%c` 参数打印字符；
- `%q` 参数打印带单引号的字符：
  - 其他方面和 C/C++ 差不多

```go
o := 0666
fmt.Printf("%d %[1]o %#[1]o\n", o) 
// "438 666 0666"

x := int64(0xdeadbeef)
fmt.Printf("%d %[1]x %#[1]x %#[1]X\n", x)
// Output:
// 3735928559 deadbeef 0xdeadbeef 0XDEADBEEF

ascii := 'a'
unicode := '国'
newline := '\n'
fmt.Printf("%d %[1]c %[1]q\n", ascii)   // "97 a 'a'"
fmt.Printf("%d %[1]c %[1]q\n", unicode) // "22269 国 '国'"
fmt.Printf("%d %[1]q\n", newline)       // "10 '\n'"
```

### 字符串

#### `s[i:j]` 取子字符串

```go
s := "hello, world"
fmt.Println(s[0:5]) // "hello"
fmt.Println(s[:5])  // "hello"
fmt.Println(s[7:])  // "world"
fmt.Println(s[:])   // "hello, world"
fmt.Println("goodbye" + s[5:])  // "goodbye, world"
```

#### \`...\` 原生的字符串字面值

- 会删除回车
- 使用反引号代替双引号
- 没有转义操作，包含退格和换行
- 用于编写正则表达式、HTML模板、JSON面值、命令行提示信息
- 无法直接写 \` 字符，可以用八进制或十六进制转义或 + "`" 连接字符串常量

```go
const GoUsage = `Go is a tool for managing Go source code.
```

#### 处理字符串和字节slice

[Golang 标准库文档链接](https://studygolang.com/pkgdoc) 

```go
// import "strconv"

x := 123
y := fmt.Sprintf("%d", x)
fmt.Println(y, strconv.Itoa(x)) // "123 123"
fmt.Println(strconv.FormatInt(int64(x), 2)) // "1111011"
x, err := strconv.Atoi("123")             // x is an int
y, err := strconv.ParseInt("123", 10, 64) // base 10, up to 64 bits
```

- strings 包提供了许多如字符串的查询、替换、比较、截断、拆分和合并等功能。

- bytes 包也提供了很多类似功能的函数，但是针对和字符串有着相同结构的 []byte 类型；
  string字符串是只读的，因此逐步构建字符串会导致很多分配和复制，使用 bytes.Buffer 类型将会更有效；

- strconv 包提供了布尔型、整型数、浮点数和对应字符串的相互转换，还提供了双引号转义相关的转换。

- unicode 包提供了 IsDigit、IsLetter、IsUpper、IsLower 等类似功能，用于给字符分类

### 数组

```go
var a [3]int 
var q [3]int = [3]int{1, 2, 3}
fmt.Println(a[0])        
fmt.Println(a[len(a)-1]) 

// 遍历数组，取下标和值
for i, v := range a {
    fmt.Printf("%d %d\n", i, v)
}

// 只取值
for _, v := range a {
    fmt.Printf("%d\n", v)
}

// 也可以根据初始化值的个数计算数组长度
q := [...]int{1, 2, 3}

// 数组r含有100个元素，最后一个元素被初始化为-1，其它元素都是0
r := [...]int{99: -1}

// 数组之间有 == 和 != 运算符
a := [2]int{1, 2}
b := [...]int{1, 2}
c := [2]int{1, 3}
fmt.Println(a == b, a == c, b == c) // "true false false"
d := [3]int{1, 2}
fmt.Println(a == d) // compile error: cannot compare [2]int == [3]int
```

### Slice

应该是可以代替 C++ 的大多数顺序容器，只不过需要开脑洞合理利用一下切片功能

~~对着 LeetCode 评论区无能狂怒：我怎么就想不出来呢~~

```go
// 本质变长序列，底层是指针、长度、容量
months := [...]string{1: "January", /* ... */, 12: "December"}

// len和cap函数分别返回slice的长度和容量
len(s) == 0    // 判空

// 不同于数组，== 操作符只能和 nil 比
var s []int    // len(s) == 0, s == nil
s = nil        // len(s) == 0, s == nil
s = []int(nil) // len(s) == 0, s == nil
s = []int{}    // len(s) == 0, s != nil

// slice可以用来模拟一个stack
stack = append(stack, v)     // 入栈
top := stack[len(stack)-1]   // 取栈顶元素，记得确保len(stack)-1 >= 0
stack = stack[:len(stack)-1] // 出栈

// 删除中间的某个元素
func remove(slice []int, i int) []int {
    copy(slice[i:], slice[i+1:])
    return slice[:len(slice)-1]
}

// sort 排序，使用了 sort.Interface
sort.Slice(arr, func(i, j int) bool { return arr[i].score < arr[j].score }) // 升序排序
```

#### `make()` 函数创建 slice

创建一个指定元素类型、长度和容量的 slice，容量部分可省略

```go
make([]T, len)
make([]T, len, cap) // same as make([]T, cap)[:len]

// 开一个r行c列空数组
res := make([][]int, r)
for i := range res {
    res[i] = make([]int, c)
}
```

#### `append() ` 函数向slice追加元素

```go
var runes []rune
for _, r := range "Hello, 世界" {
    runes = append(runes, r)
}

// 第二个参数也可以是Slice，但是后面要加...
badOranges = append(badOranges, nextBadOranges...)
```

### Map

Slice 和 Map 都只能和 nil 比，不能像数组那样互相比；

```go
// map 是哈希表，可以写为 map[K]V 
ages := make(map[string]int) 

// 也可以花括号初始化
ages := map[string]int{
    "alice":   31,
    "charlie": 34,
}

// Go 没提供查询一个key在不在map中的办法，但是可以利用方便的变量赋值实现
// _, ok := map[key];语句查找成功返回两个值，ok=true，查找失败时返回零值，ok=false
// ok 的布尔值是 if 的最终结果 
if _, ok := map[key]; ok {
    // 存在
}

// 使用了fmt.Sprintf函数将字符串列表转换为一个字符串用作map的key
var m = make(map[string]int)
func k(list []string) string  { return fmt.Sprintf("%q", list) }
func Add(list []string)       { m[k(list)]++ }
func Count(list []string) int { return m[k(list)] }

// Go 语言没有 set，可以用忽略值只用键的 map 代替；
seen := make(map[string]struct{})  // a set of strings
```

#### delete() 函数删除 map 中元素

```go
delete(ages, "alice") // remove element ages["alice"]
```

### 结构体

成员顺序不同的结构体是不相同的；

**大写字母开头的成员就是 public 的；**

```go
// 定义
type Employee struct {
    ID        int
    Name, Address   string
    DoB       time.Time
    Position  string
    Salary    int
    ManagerID int
}

// 新建实例
var dilbert Employee

// 访问成员
dilbert.Salary -= 5000 // demoted, for writing too few lines of code
position := &dilbert.Position
*position = "Senior " + *position // promoted, for outsourcing to Elbonia
var employeeOfTheMonth *Employee = &dilbert
employeeOfTheMonth.Position += " (proactive team player)"

// 返回结构体的函数
func EmployeeByID(id int) *Employee { /* ... */ }
fmt.Println(EmployeeByID(dilbert.ManagerID).Position) // "Pointy-haired boss"
id := dilbert.ID
EmployeeByID(id).Salary = 0  // fired for... no real reason

// 结构体字面值表示方法1
type Point struct{ X, Y int }
p := Point{1, 2}

// 结构体字面值表示方法2，以成员名字和相应的值来初始化
anim := gif.GIF{LoopCount: nframes}

// 修改结构体内容的函数
func AwardAnnualRaise(e *Employee) {
    e.Salary = e.Salary * 105 / 100
}

// 直接返回地址
pp := &Point{1, 2}   /* 和下面两条语句是等价的
pp := new(Point)
*pp = Point{1, 2} */

// 如果所有成员都是可以比较的，那么该结构体也是可以用 == 和 != 比较的；
p := Point{1, 2}
q := Point{2, 1}
fmt.Println(p.X == q.X && p.Y == q.Y) // "false"

// 匿名成员，只声明一个成员对应的数据类型而不指名成员的名字
type Point struct {
    X, Y int
}

type Circle struct {
    Point
    Radius int
}

type Wheel struct {
    Circle
    Spokes int
}

var w Wheel
w.X = 8            // equivalent to w.Circle.Point.X = 8
w.Y = 8            // equivalent to w.Circle.Point.Y = 8
w.Radius = 5       // equivalent to w.Circle.Radius = 5
w.Spokes = 20
```

### 数据结构常用的结构体

```go
// 单链表
type ListNode struct {
    Val  int
    Next *ListNode
}

// 二叉树
type TreeNode struct {
    Val int
    Left *TreeNode
    Right *TreeNode
}

// 递归实现中序遍历二叉树
func inorderTraversal(root *TreeNode) []int {
    var res []int
    var inorder func(*TreeNode)
    inorder = func(node *TreeNode) {
        if node == nil {
            return
        }
        inorder(node.Left)
        res = append(res, node.Val)
        inorder(node.Right)
    }
    inorder(root)
    return res
}
```

### JSON 的对象类型

标准库有一些包提供支持：

- encoding/json
- encoding/xml
- encoding/asn1

```json
boolean         true
number          -273.15
string          "She said \"Hello, BF\""
array           ["gold", "silver", "bronze"]
object          {"year": 1980,
                 "event": "archery",
                 "medals": ["gold", "silver", "bronze"]}
```

```go
type Movie struct {
    Title  string
    // 结构体成员Tag, 通常是一系列用空格分隔的key:"value"键值对序列, 键名:对象名,选项 
    // omitempty选项表示当Go语言结构体成员为空或零值时，不生成该JSON对象
    Year   int  `json:"released"`         
    Color  bool `json:"color,omitempty"` 
    Actors []string
}

var movies = []Movie{
    {Title: "Casablanca", Year: 1942, Color: false,
        Actors: []string{"Humphrey Bogart", "Ingrid Bergman"}},
    {Title: "Cool Hand Luke", Year: 1967, Color: true,
        Actors: []string{"Paul Newman"}},
    {Title: "Bullitt", Year: 1968, Color: true,
        Actors: []string{"Steve McQueen", "Jacqueline Bisset"}},
    // ...
}

// 结构体slice转为JSON
data, err := json.MarshalIndent(movies, "", "    ")
if err != nil {
    log.Fatalf("JSON marshaling failed: %s", err)
}
fmt.Printf("%s\n", data)
```

### 文本和HTML模板

标准库有一些包提供支持，遇事不决查文档：

- text/template
- html/template

```bash
$ go doc html/template
```

```go
const templ = `{{.TotalCount}} issues:
{{range .Items}}----------------------------------------
Number: {{.Number}}
User:   {{.User.Login}}
Title:  {{.Title | printf "%.64s"}}
Age:    {{.CreatedAt | daysAgo}} days
{{end}}`
```

## 3. 函数

### `func` 声明函数

- 函数值之间不可比较；
- 不能用函数值作为map的key
- 函数像其他值一样，拥有类型；
- 函数可以被赋值给其他变量，传递给函数，作为函数返回值；

```go
func name(parameter-list) (result-list) {
    body
}

// 等价于func f(i int, j int, k int,  s string, t string)
func f(i, j, k int, s, t string) { /* ... */ } 

// 空白标识符依然能用，可以忽略第二个参数而不影响其函数签名
func first(x int, _ int) int { return x }

// 可变参数个数
func sum(vals ...int) int {
    total := 0
    for _, val := range vals {
        total += val
    }
    return total
}

// 函数赋值给变量
var preorder func(*TreeNode)
preorder = func(node *TreeNode) {
    if node == nil {
        return
    }
    res = append(res, node.Val)
    preorder(node.Left)
    preorder(node.Right)
}
preorder(root)    // 调用
```

### 递归

Go 语言自带可变栈，栈的大小按需增加，所以使用递归时不必考虑溢出和安全问题；

```go
/* Leetcode 21. 合并两个有序链表 */

type ListNode struct {
    Val  int
    Next *ListNode
}

func mergeTwoLists(list1 *ListNode, list2 *ListNode) *ListNode {
    if list1 == nil {
        return list2
    } else if list2 == nil {
        return list1
    } else if list1.Val < list2.Val {
        // 递归设计思路：让较小结点的 next 指针指向其余结点的合并结果
        list1.Next = mergeTwoLists(list1.Next, list2)
        return list1
    } else {
        list2.Next = mergeTwoLists(list1, list2.Next)
        return list2
    }
}
```

### 多返回值的函数

```go
// 函数直接 return res1, res2 就行
func findLinks(url string) ([]string, error) {
    resp, err := http.Get(url)
    if err != nil {
        return nil, err
    }
    if resp.StatusCode != http.StatusOK {
        resp.Body.Close()
        return nil, fmt.Errorf("getting %s: %s", url, resp.Status)
    }
    doc, err := html.Parse(resp.Body)
    resp.Body.Close()
    if err != nil {
        return nil, fmt.Errorf("parsing %s as HTML: %v", url, err)
    }
    return visit(nil, doc), nil
}

// 调用时应该显式分配这多个返回值
links, err := findLinks(url)
links1, _ := findLinks(url) 

// 按照惯例，函数的最后一个bool类型的返回值表示函数是否运行成功，error类型的返回值代表函数的错误信息
// 每一个return语句等价于：return words, images, err
func CountWordsAndImages(url string) (words, images int, err error) {
    resp, err := http.Get(url)
    if err != nil {
        return
    }
    doc, err := html.Parse(resp.Body)
    resp.Body.Close()
    if err != nil {
        err = fmt.Errorf("parsing HTML: %s", err)
        return
    }
    words, images = countWordsAndImages(doc)
    return
}

func countWordsAndImages(n *html.Node) (words, images int) { /* ... */ }
```

### 错误处理

方法1：返回错误信息

```go
doc, err := html.Parse(resp.Body)
resp.Body.Close()
if err != nil {
    // 注意错误信息中应避免大写和换行符
    return nil, fmt.Errorf("parsing %s as HTML: %v", url,err) 
}

// 输出函数获得字符串类型的错误信息
fmt.Println(err)
fmt.Printf("%v", err)
```

方法2：重复尝试操作

```go
for tries := 0; time.Now().Before(deadline); tries++ {
    _, err := http.Head(url)
    if err == nil {
        return nil // success
    }
    log.Printf("server not responding (%s);retrying…", err)
    time.Sleep(time.Second << uint(tries)) // exponential back-off
}
```

方法3：输出错误信息并结束程序

* 应该只在main中执行这种操作，库函数一般只应该传播错误，除非遇到bug

```go
// (In function main.)
if err := WaitForServer(url); err != nil {
    fmt.Fprintf(os.Stderr, "Site is down: %v\n", err)
    os.Exit(1)
}

// 更标准的写法
if err := WaitForServer(url); err != nil {
    log.Fatalf("Site is down: %v\n", err)
}

// 也可以只输出错误信息
if err := Ping(); err != nil {
    log.Printf("ping failed: %v; networking disabled",err)
}
```

### 匿名函数

优点在于可以访问它所在作用域里的全部变量

```go
// squares返回一个匿名函数, 该匿名函数每次被调用时都会返回下一个数的平方。
func squares() func() int {
    var x int
    return func() int {
        x++
        return x * x
    }
}
```

### defer 语句

当执行到含有关键字 `defer` 的语句时，函数和参数表达式得到计算，但直到包含该defer语句的函数执行完毕时，defer后的函数才会被执行

```go
// 例：互斥锁
var mu sync.Mutex
var m = make(map[string]int)
func lookup(key string) int {
    mu.Lock()
    defer mu.Unlock()
    return m[key]
}
```

### panic 异常和 recover 捕获异常

当panic异常发生时：

- 程序会中断运行，并立即执行在该 goroutine 中的 `defer` 函数
- 随后程序崩溃并输出日志信息
  - 日志信息包括panic value和函数调用的堆栈跟踪信息

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

