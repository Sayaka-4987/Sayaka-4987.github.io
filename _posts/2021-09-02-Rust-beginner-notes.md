---
layout:     post   				        # 使用的布局（不需要改）
title:      Rust 学习笔记 				# 标题 
subtitle:   先在这里挖个大坑		        # 副标题
date:       2021-09-02 				    # 时间
author:     YXWang 					    # 作者
header-img: img/post-bg-keybord.jpg 	# 这篇文章的标题背景图片
catalog: true 						    # 是否归档
tags:								    # 标签
    - Rust
    - 施工中


---

# Rust 学习笔记

主要内容来自 [Rust 程序设计语言 简体中文版](http://120.78.128.153/rustbook/) 和 [Rust 教程 \| 菜鸟教程](https://www.runoob.com/rust/rust-tutorial.html) 

建议有 C++ / Java 基础再阅读



## 环境配置

Rust：这里用 [IntelliJ\_IDEA 社区版](https://www.jetbrains.com/zh-cn/idea/download/#section=windows) 作为 IDE ，需要安装 `Rust` 和 `toml` 两个插件，中间差什么可以无脑跟着 IDE 的提示装，新建项目选 `Rust-Binary (application) `；

Cargo：是 Rust 的构建系统和包管理器，用 Cargo 新建的项目会自动生成两个文件和一个目录：一个 Cargo.toml 配置文件，一个 src 目录，以及位于 src 目录中的 main.rs 文件；同时初始化一个配备 .gitignore 文件的 git 仓库；

```toml
# Cargo.toml 的内容

[package]
name = "rust_application_1"
version = "0.1.0"
edition = "2018"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]

```



## 例：猜数字

~~（小时候QQ群语c老爱玩这个了，怀念ing）~~

### 第一部分：获取输入和命令行输出

```rust
use std::io;	// 将输入输出库引入当前作用域

fn main() {		// fn 语法声明了一个新函数，() 表明没有参数， main 是程序的入口
    println!("Guess the number!");
    println!("Please input your guess.");	// println 是命令行输出的宏

    let mut guess = String::new();	// 创建变量 guess 储存用户输入， new 是 String 类型的关联函数，创建了一个新的空字符串

    io::stdin().read_line(&mut guess)	// 调用 read_line 方法从标准输入句柄获取用户输入到 guess 变量中，写成 &mut guess 使其成为一个可变的引用
        .expect("Failed to read line");	// 编写错误处理代码，否则 Rust 将报错 note: this `Result` may be an `Err` variant, which should be handled 

    println!("You guessed: {}", guess);	// 使用占位符 {} 
}

```



### 第二部分：生成随机数字

需要添加一个 rand 库的依赖，打开 https://crates.io/crates/rand ，复制网页右侧 Install 版块的 `rand = "0.8.4"` 加到 Cargo.toml 配置文件中：

```toml
[dependencies]
rand = "0.8.4"
```

<img src="https://s3.bmp.ovh/imgs/2021/09/8e051a963a2abd0e.png" style="zoom:50%;" />



然后开头引入 `Rng` 这个 trait：

```rust
use rand::Rng;
```



可以在项目目录下，用 cmd / Powershell 运行 `cargo doc --open` 命令，打开所有本地依赖提供的文档；

<img src="https://i.bmp.ovh/imgs/2021/09/8fff9d766298e77c.png" style="zoom: 33%;" />



`rand::thread_rng` 函数从操作系统获取 seed 的随机数生成器，调用随机数生成器的 `gen_range` 方法，接受一个范围表达式作为参数，该方法会在参数范围内生成一个随机数；范围表达式此处采用 `start..end` 形式，它是一个左闭右开区间，包含下限但不包含上限，需要指定 `1..101` 来获取一个 1 和 100 之间的数；等价写法还有 `1..=100` ；

```rust
let secret_number = rand::thread_rng().gen_range(1..101);
```



### 第三部分：比较猜测的数字

需要引入 `Ordering` 这个枚举类型：

```rust
use std::cmp::Ordering;
```

此处用一个 match 表达式 比较猜测的数字和秘密数字， match 由 **分支** 构成，一个分支包含：一个模式 ，表达式 **开头的值与分支模式相匹配** 时应该被执行的一块代码；Rust 获取提供给 `match` 的值，并挨个检查每个分支的模式：

```rust
match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win!"),
    }
}
```

为了让 secret 和 guess 类型匹配，需要添加一行：

```rust
let guess: u32 = guess
	.trim()		// 去除空白字符
	.parse()	// 将字符串解析成数字，需要指定数字类型
	.expect("Please type a number!");
```



### 第四部分：循环和控制条件

`loop` 关键字创建无限循环：

```rust
loop {
    println!("Please input your guess.");

     ……

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win!"),
    }
}
```

退出循环还是用 break; 语句



### 猜数字的最终成品

```rust
use std::io;
use rand::Rng;
use std::cmp::Ordering;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..101);

    // println!("The secret number is: {}", secret_number);

    loop {
        println!("Please input your guess.");

        
        let mut guess = String::new();

        io::stdin().read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };  // 忽略非数字，让用户可以继续猜测
        
        // 这一段还有一种更精简的处理方式：let guess: u32 = guess.trim().parse()?; 向上层抛错误

        println!("You guessed: {}", guess);

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!"); break;
            }
        }
    }
}
```



## 变量和可变性

~~摘录一些名言警句：~~

> 虽然编译错误令人沮丧，但那只是表示程序不能安全的完成你想让它完成的工作；并**不能**说明你不是一个好程序员！

Rust 的变量默认是不可改变的；声明变量后，变量类型就被确定了，不能再赋值其它类型的值；另外，Rust 也不允许精度有损失的自动数据类型转换；

Rust 语言设计这种机制的原因是，若一部分代码假设一个值永远也不会改变，而另一部分代码改变了这个值，前者就有可能以不可预料的方式运行，且这种原因造成的错误难以事后发现。因此，Rust 编译器保证，如果声明一个值不会变，它就真的不会变。

```rust
let a = 123;	// 默认声明不可变变量
// 注意：不允许用 a = 456; 这样的语句私自修改不可变变量
// 一旦值被绑定在一个名称上，你就不能改变这个值
let a = 456;	// 但允许重新绑定不可变变量

let b: u64 = 123;	// 声明指定类型的变量 
```



### `mut` 关键字使变量可变

```rust
let mut d = 123;	// 声明可变变量
d = 456;			// 可以合法的改变这个值
```



### `const` 关键字声明常量

Rust 对常量的命名约定是在单词之间使用全大写，中间用下划线连接

```rust
const c: i32 = 123;		// 去掉 let，用 const 声明完全不允许改变的常量
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;	// 常量只能被设置为常量表达式
```



### Rust 的隐藏（Shadowing）机制

Rust 允许定义一个与之前变量 **同名的新变量** ，称之为第一个变量被第二个 **隐藏** 了，这意味着程序使用这个变量时会看到第二个值；

可以用相同变量名称来隐藏一个变量；

可以重复使用 `let` 关键字来多次隐藏；

```rust
fn main() {
    let x = 5;	// 将 x 绑定到值 5 上

    let x = x + 1;	// 通过 let x = 隐藏 x，获取初始值并加 1， x 的值变成 6 
    
    {
        let x = x * 2;
        println!("The value of x in the inner scope is: {}", x);	// 隐藏了 x，将之前的值乘以 2，x 的值是 12
    }

    println!("The value of x is: {}", x);	// 内部 shadowing 的作用域结束，x 返回到 6
}
```



### 隐藏和可变变量的区别

隐藏：

1. 显式使用了 `let` 对变量重新赋值，赋值完得到的依然是一个不可变变量；
2. 本质是创建了一个新变量，我们可以改变值的类型，但复用这个名字；

```rust
let spaces = "   ";
let spaces = spaces.len();
```

可变变量：和其他强类型语言差不多，值可变，但不能改变变量的类型；



## 数据类型

Rust 是 **静态类型** 语言，在编译阶段必须知道所有变量的类型；

编译器通常可以推断出我们想要用的类型，但当有多种推断可能时，就必须增加类型注解，否则编译器将报错： ^^^^^ consider giving \`variable_name\` a type ；



### 标量类型

标量类型代表一个单独的值；

Rust 有四种基本的标量类型：整型、浮点型、布尔类型和字符类型；



#### Rust 整型

以 `u32` 整数类型为例，该类型声明表明，它关联的值应该是一个占据 32 比特位的无符号整数，有符号整数类型以 `i` 开头，无符号以 `u` 开头；

| 长度    | 有符号  | 无符号  | 备注                                                         |
| ------- | ------- | ------- | ------------------------------------------------------------ |
| 8-bit   | `i8`    | `u8`    |                                                              |
| 16-bit  | `i16`   | `u16`   |                                                              |
| 32-bit  | `i32`   | `u32`   |                                                              |
| 64-bit  | `i64`   | `u64`   |                                                              |
| 128-bit | `i128`  | `u128`  |                                                              |
| arch    | `isize` | `usize` | `isize` 和 `usize` 类型依赖运行程序的计算机架构<br>64 位架构上它们是 64 位的， 32 位架构上它们是 32 位的 |



#### 整型字面值

Rust 允许使用类型后缀，例如 `57u8` 来指定类型，同时也允许使用 `_` 作为分隔符方便读数，例如`1_000`，它的值与 `1000` 相同；

| 数字字面值                    | 例子          |
| ----------------------------- | ------------- |
| Decimal (十进制)              | `98_222`      |
| Hex (十六进制)                | `0xff`        |
| Octal (八进制)                | `0o77`        |
| Binary (二进制)               | `0b1111_0000` |
| Byte (单字节字符)(仅限于`u8`) | `b'A'`        |



#### 拓展：整型溢出

当发生 “整型溢出” 时，Rust 有一些有趣的规则：

1. 在 **debug 模式**编译时，Rust 会检查这类问题，并使程序 panic，这个术语被 Rust 用来表明程序因错误而退出。
2. 在 **release 构建**中，Rust 不检测溢出，相反会进行一种被称为二进制补码包装（two’s complement wrapping）的操作。简而言之，以 `u8` 类型（范围 0-255 ）为例，值 256 变成 0，值 257 变成 1，依此类推



注意：依赖整型溢出被认为是一种错误，即便可能出现这种行为

如果确实需要这种行为，标准库中有一个类型显式提供此功能，Wrapping。 为了显式地处理溢出的可能性，你可以使用标准库在原生数值类型上提供的以下方法：

- 所有模式下都可以使用 `wrapping_*` 方法进行包装，如 `wrapping_add`
- 如果 `check_*` 方法出现溢出，则返回 `None`值
- 用 `overflowing_*` 方法返回值和一个布尔值，表示是否出现溢出
- 用 `saturating_*` 方法在值的最小值或最大值处进行饱和处理



#### Rust 浮点数

Rust 也有两个原生的 **浮点数** 类型，它们是带小数点的数字

Rust 的浮点数类型是 `f32` 和 `f64`，分别占 32 位和 64 位；

默认类型是 `f64`，因为在现代 CPU 中，它与 `f32` 速度几乎一样，不过精度更高；

浮点数采用 IEEE-754 标准表示。`f32` 是单精度浮点数，`f64` 是双精度浮点数；



> Rust 中的所有数字类型都支持基本数学运算：加法、减法、乘法、除法和取余；
>
> 整数除法会向下舍入到最接近的整数；



#### Rust 布尔型

有两个可能的值：`true` 和 `false`；和其他高级语言应该区别不大；



#### Rust 字符型

Rust 的 `char` 类型的大小为四个字节，并代表了一个 Unicode 标量值，这意味着它可以比 ASCII 表示更多内容：

```rust
fn main() {
    let c = 'z';
    let z = 'ℤ';
    let heart_eyed_cat = '😻';
}
```

拼音字母，中文、日文、韩文等字符，emoji（绘文字），以及零长度的空白字符都是有效的 `char` 值，Unicode 标量值包含从 `U+0000` 到 `U+D7FF` 和 `U+E000` 到 `U+10FFFF` 在内的值；



### 复合类型

Rust 有两个原生的复合类型：元组和数组



#### 元组（tuple）

1. 元组是一个将多个其他类型的值组合进一个复合类型的主要方式；
2. 元组长度固定，一旦声明，其长度不会增大或缩小；
3. 使用包含在圆括号中的逗号分隔的值列表来创建一个元组；
4. 元组中的每一个位置都有一个类型，而且这些不同值的类型也不必是相同的；



可以使用模式匹配来解构元组值，代码示例如下：

```rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);	// 创建了一个元组，绑定到 tup 变量上

    let (x, y, z) = tup;	// 使用了 let 和一个模式，将 tup 分成了三个不同的变量x, y, z，称为解构

    println!("The value of y is: {}", y);
}
```



也可以使用点号`.` 后跟值的索引来直接访问它们：

```rust
fn main() {    let x: (i32, f64, u8) = (500, 6.4, 1);    let five_hundred = x.0;    let six_point_four = x.1;    let one = x.2;}
```



#### 数组（array）

与元组不同，数组中的每个元素的类型必须相同；

Rust 的数组是固定长度的，一旦声明，它们的长度不能增长或缩小；

数组中的值位于中括号内的逗号分隔的列表中；

数组在栈（stack）上为数据分配空间；

```rust
fn main() {    let a = [1, 2, 3, 4, 5];        let b: [i32; 5] = [1, 2, 3, 4, 5];        let c = [3; 5];	// 等效于 c = [3, 3, 3, 3, 3]        let months = ["January", "February", "March", "April", "May", "June", "July",              "August", "September", "October", "November", "December"];}
```



和其他高级语言一样，直接用下标访问数组，如 a[0] = 1；

当尝试用索引访问一个元素时，Rust 会检查指定的索引是否小于数组的长度，如果索引超出了数组长度，Rust 会 panic，带着错误信息直接退出；



vector 类型是标准库提供的一个 **允许增长和缩小长度** 、类似数组的集合类型；

当不确定是应该使用数组还是 vector 的时候，尽量使用 vector；



## 函数

用 `fn` 关键字+函数名称+一对圆括号包裹的含参列表，声明新函数；

**必须声明每个参数的类型**；

Rust 的函数和变量名应当使用 *snake case* 规范风格（全小写，用下划线连接）；

```rust
fn another_function(x: i32) {    println!("The value of x is: {}", x);}
```



### 包含语句和表达式的函数体

语句（Statements）：执行一些操作但不返回值的指令

表达式（Expressions）：计算并产生一个**返回值**，**结尾没有分号**

Rust 是一门基于表达式（expression-based）的语言；

```rust
fn main() {    let x = 5;	// let 创建变量并绑定值是一个语句    let y = {        let x = 3;        x + 1	// 这是个表达式，它返回值为 4，末尾没有分号    };    println!("The value of y is: {}", y);}
```



### 函数返回值

这里 five 函数的返回值是 5，返回值类型是 i32 ：

```rust
fn five() -> i32 {    5	// 这里加个分号将会报错 implicitly returns `()` as its body has no tail or `return` expression  - help: consider removing this semicolon}
```



## 控制流

### if 表达式

和其他高级语言似乎区别不大，以 `if` 关键字开头，后跟一个条件，条件必须是 bool 值：

```rust
fn main() {    let number = 3;    if number < 5 {        println!("condition was true");    } else {        println!("condition was false");    }}
```



#### 结合 `let` 语句使用 `if` 

```rust
fn main() {    let condition = true;    let number = if condition {	// 将 if 表达式的返回值赋给一个变量        5    } else {        6    };        println!("The value of number is: {}", number);}
```



### `loop` 循环 

1. loop 循环会反复地执行其中的代码块，直到遇 break; 语句或按 `Ctrl+C` 才退出； 
2. 可以用来执行需要重复的工作，也可以用来重试可能会失败的工作；
3. `break` 表达式后面可以接返回值：

```rust
fn main() {    let mut counter = 0;    let result = loop {        counter += 1;        if counter == 10 {            break counter * 2;	// 使用 break 关键字返回 counter * 2 的值        }    };	// 通过分号结束赋值给 result 的语句    println!("The result is {}", result);}
```



#### 指定循环标签

若存在嵌套循环，`break` 和 `continue` 会默认应用于此时最内层的循环；

可以在一个循环上指定一个 **循环标签**，然后将标签与 `break` 或 `continue` 一起使用：

```rust
fn main() {
    let mut count = 0;
    'counting_up: loop {	// 外层循环有一个标签 counting_ up
        println!("count = {}", count);
        let mut remaining = 10;

        loop {
            println!("remaining = {}", remaining);
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'counting_up;		// 将退出外层循环
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {}", count);
}
```



### `while` 循环 

和其它高级语言比较类似：

```rust
while number != 0 {
    println!("{}!", number);
    number = number - 1;
}
```



### `for` - `in`循环

Rust 中最常用的循环；

能保证在循环的每次迭代中，索引都在数组的边界内；

```rust
fn main() {    let a = [10, 20, 30, 40, 50];    for element in a.iter() {        println!("the value is: {}", element);    }        for number in (1..4).rev() {	// rev 方法用于反转 [1,4) 这个 Range        println!("{} ", number);	// 输出结果为 3 2 1     }}
```



### 练习：打印圣诞颂歌 “The Twelve Days of Christmas” 的歌词

~~我写了一个最老实的版本发出来丢人~~（想看骚操作可以自行搜索引擎一下

```rust
fn main() {    println!("The Twelve days of Christmas. ");    let days = ["first", "second", "third", "fourth", "fifth", "sixth", "seventh", "eighth", "ninth", "tenth", "eleventh", "twelfth"];    let gifts = ["And a partridge in a pear tree.", "Two turtle doves, ", "Three French hens, ", "Four calling birds, ", "Five golden rings, ", "Six geese a-laying, ", "Seven swans a-swimming, ", "Eight maids a-milking, ", "Nine ladies dancing, ", "Ten lords a-leaping, ", "Eleven pipers piping, ", "Twelve drummers drumming, "];    for i in 0..12 {        print!("On the {} day of Christmas, my true love sent to me: ", days[i]);        if i == 0 { println!("A partridge in a pear tree."); continue; }        else {            let mut j = i;            loop {                print!("{}", gifts[j]);                if j == 0 { println!(); break; }                else { j = j-1; }            }        }    }}
```



## 所有权

Rust 不采用垃圾回收机制，而是通过 **所有权系统** 管理内存，编译器在编译时会根据一系列的规则进行检查，该功能并不影响运行速度；

所有权系统要处理的问题包括：跟踪哪部分代码正在使用堆上的哪些数据，最大限度的减少堆上的重复数据的数量，以及清理堆上不再使用的数据确保不会耗尽空间。



栈和堆都是代码在运行时可供使用的内存，但是它们的结构不同；

| 栈（Stack）                                                  | 堆（Heap）                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 栈以放入值的顺序存储值并以相反顺序取出值。这也被称作 **后进先出**。<br>栈中的所有数据都必须占用 **已知且固定的大小**；<br>入栈比在堆上分配内存要快，因为入栈时操作系统无需为新数据去搜索内存空间，其位置总是在栈顶；<br>调用一个函数时，传递给函数的值（包括可能指向堆上数据的指针）和函数的局部变量会被压入栈中；当函数结束时，这些值被移出栈。<br>指针的大小是已知并且固定的，所以将指向堆上数据的指针推入栈中并不是分配；<br> | 编译时大小未知、或大小可能变化的数据，要改为存储在堆上；<br>当向堆放入数据时，要请求一定大小的空间，由操作系统在堆中找到一块足够大的空位，把它标记为已使用，并返回一个 **表示该位置地址的指针** ，这个过程称作 **在堆上分配内存**；<br>访问堆上的数据比访问栈上的数据慢，因为必须通过指针来访问堆中的数据，而现代处理器 **在内存中跳转越少就越快**；<br>出于同样原因，处理器在处理的数据彼此较近的时候（比如在栈上）比较远的时候（比如可能在堆上）能更好的工作；<br> |



### 所有权的规则 

1. Rust 中的每一个值都有一个被称为其 **所有者** 的变量；
2. 值在任一时刻 **有且只有** 一个所有者；
3. 当所有者（变量）**离开作用域**，这个值将被丢弃；



### 变量作用域

Rust 变量是否有效与作用域的关系类似其他编程语言：

```rust
{	// s 在这里无效, 它尚未声明    let s = "hello";   // 从此处起，s 是有效的        ……	// 使用 s    }	// 此作用域已结束，s 不再有效
```



#### 以储存在堆上的 String 类型为例

在 Rust 中 `String` 类型被分配到堆上，所以能够存储在编译时未知大小的文本。可以使用 `from` 函数基于字符串字面值来创建 `String`；

此处两个连用的冒号 `::` 是运算符，允许将特定的 `from` 函数置于 `String` 类型的命名空间下：

```rust
let s = String::from("hello");s.push_str(", world!");		// 允许用 push_str() 在字符串后追加字面值println!("{}", s);			// 将打印 `hello, world!`
```

Rust 采取的内存管理策略是：**内存在拥有它的变量离开作用域后，就被自动释放**。



### 变量与数据交互的方式

#### 移动

Rust 中的多个变量可以采用一种独特的方式与同一数据交互：

```rust
let s1 = String::from("hello");let s2 = s1;println!("{}, world!", s1);
```

以上代码会得到这样的报错信息，Rust 禁止使用无效的引用：

```c
/* 套这个注释是为了 Rouge 能正常高亮

error[E0382]: borrow of moved value: `s1`
 --> src\main.rs:7:28
  |
4 |     let s1 = String::from("hello");
  |         -- move occurs because `s1` has type `String`, which does not implement the `Copy` trait
5 |     let s2 = s1;
  |              -- value moved here
6 | 
7 |     println!("{}, world!", s1);
  |                            ^^ value borrowed here after move
  
*/  
```

Rust **永远也不会自动创建数据的 “深拷贝”**，这里拷贝指针、长度和容量而不拷贝数据，有些像浅拷贝，但 Rust 同时使第一个变量 `s1` 无效了，因此 `s2 = s1` 这个操作被称为 **移动**，这时只有 `s2` 是有效的变量，此时变量 `s2` 的内存表现如图所示：

<img src="http://120.78.128.153/rustbook/img/trpl04-04.svg" style="zoom: 25%;" />



#### 克隆

如果我们**确实需要深度复制 `String` 中堆上的数据**，可以使用一个叫做 `clone` 的通用函数：

```rust
let s1 = String::from("hello");let s2 = s1.clone();println!("s1 = {}, s2 = {}", s1, s2);
```

当看到 `clone` 调用时，应当充分理解有一些特定的代码被执行，而且这些代码可能相当消耗资源；



#### 拷贝只在栈上的数据

这里没有调用 `clone`，不过 `x` 依然有效且没有被移动到 `y` 中：

```rust
let x = 5;let y = x;println!("x = {}, y = {}", x, y);
```

原因是，像整型这样的 **在编译时已知大小的类型** 被整个存储在 **栈** 上，所以拷贝其实际的值是快速的。这意味着没有理由在创建变量 `y` 后使 `x` 无效；



#### 补充：`Copy` trait

Rust 有一个叫做 `Copy` trait 的特殊注解，性质如下：

1. 可以用在类似整型这样的存储在栈上的类型上（第十章再详细讲解，待补充）；

2. 若一个类型拥有 `Copy` trait，旧的变量在将其赋值给其他变量后将仍然可用；

3. Rust 不允许实现了 `Drop` trait 的类型使用 `Copy` trait，如果我们对其值离开作用域时需要特殊处理的类型使用 `Copy` 注解，将会出现一个编译时错误；

4. 以下是一些 `Copy` 的类型：

   所有整数类型

   所有浮点数类型

   布尔类型 `bool` 

   字符类型 `char`

   **包含的类型都是 `Copy` 的元组**；

   （举例：`(i32, i32)` 是 `Copy` 的，`(i32, String)` 不是）



### 所有权与函数

函数可以转移值的所有权：

```rust
fn main() {
    let s = String::from("hello");  // s 进入作用域

    println!("{}", s);

    takes_ownership(s);             // s 的值移动到函数里 ...

    // println!("{}", s);   		非法，s 到这里不再有效

    let x = 5;                      // x 进入作用域

    println!("{}", x);

    makes_copy(x);                  // x 应该移动函数里到
    
    println!("{}", x);   			// 但 i32 是 Copy 的，所以此处可继续使用 x

} // 这里, x 先移出了作用域，然后是 s。但因为 s 的值已被移走，所以不会有特殊操作

fn takes_ownership(some_string: String) { // some_string 进入作用域
    println!("{}", some_string);
} // 这里，some_string 移出作用域并调用 `drop` 方法。占用的内存被释放

fn makes_copy(some_integer: i32) { // some_integer 进入作用域
    println!("{}", some_integer);
} // 这里，some_integer 移出作用域。不会有特殊操作
```



### 所有权与返回值

返回值也可以转移所有权：

```rust
fn main() {
    let s1 = gives_ownership();         // gives_ownership 将返回值移给 s1

    let s2 = String::from("hello");     // s2 进入作用域

    let s3 = takes_and_gives_back(s2);  // s2 被移动到 takes_and_gives_back 中，takes_and_gives_back 也将返回值移给 s3
} // 这里 s3 移出作用域并被丢弃; s2 也移出作用域，但已被移走，所以什么也不会发生。s1 移出作用域并被丢弃

// gives_ownership 将返回值移动给调用它的函数
fn gives_ownership() -> String {             
    let some_string = String::from("hello"); // some_string 进入作用域.
    some_string                              // 返回 some_string 并移出给调用的函数
}

// takes_and_gives_back 将传入字符串并返回该值
fn takes_and_gives_back(a_string: String) -> String {	// a_string 进入作用域
    a_string  											// 返回 a_string 并移出给调用的函数
}
```

默认操作是：变量的所有权会在值赋给另一个变量时移动过去，如果只想用不想转移所有权，请使用引用；



### 引用与借用

引用：允许你使用一个值，但不获取其所有权，默认情况下**不允许修改引用的值**；

借用：将获取的引用作为函数参数的行为；

#### `&` 运算符引用 

`&s1` 创建一个**指向值 `s1` 的引用**，但是并不拥有它；

因为 `calculate_length` 函数并不拥有这个值，所以引用离开作用域时，其指向的值 s 也不会被丢弃：

```rust
fn main() {    let s1 = String::from("hello");    let len = calculate_length(&s1);    println!("The length of '{}' is {}.", s1, len);}fn calculate_length(s: &String) -> usize {	// 以 s 的引用作为参数，而不是获取值的所有权    s.len()}
```



#### 可变引用

注意：特定作用域中的特定数据 **只能有一个可变引用**；

不能 **同时** 拥有同一值的多个可变引用；

也不能在拥有不可变引用的 **同时**，拥有可变引用；

写法上，把 `&s` 换成 `&mut s`，函数参数列表处也从 `&String` 换成 `&mut String` 即可：

```rust
fn main() {    let mut s = String::from("hello");    change(&mut s);}fn change(some_string: &mut String) {    some_string.push_str(", world");}
```



> #### 数据竞争
>
> 数据竞争（*data race*）类似于竞态条件，它可由这三个行为造成：
>
> - 两个或更多指针同时访问同一数据。
> - 至少有一个指针被用来写入数据。
> - 没有同步数据访问的机制。
>
> 数据竞争会导致未定义行为，难以在运行时追踪，并且难以诊断和修复；
>
> Rust 完全不会编译存在数据竞争的代码；



### 切片（Slice）类型

slice 类型没有所有权，该类型允许你引用集合中一段连续的元素序列，而不用引用整个集合；



这里有一个习题：编写一个函数，该函数接收一个字符串，并返回在该字符串中找到的第一个单词。如果函数在该字符串中并未找到空格，则整个字符串就是一个单词，返回整个字符串：

```rust
fn first_word(s: &String) -> &str {    let bytes = s.as_bytes();	// 用 as_bytes 方法将 String 转化为字节数组    for (i, &item) in bytes.iter().enumerate() {        // 用 iter 方法在字节数组上创建一个迭代器        // 而 enumerate 包装了 iter 的结果        // 元组中的 i 是索引，而元组中的 &item 是单个字节        if item == b' ' {            return &s[0..i];	// 如果找到了一个空格，返回从开头到它的切片        }    }    &s[..]}
```



#### 字符串切片

以 `String` 为例，取其中一部分值的引用：

```rust
let s = String::from("hello world");

let hello = &s[0..5];
let world = &s[6..11];
```

可以省略上下界：

```rust
let s = String::from("hello");

let slice1 = &s[..2];	// 等效于 &s[0..2];
let slice2 = &s[3..];	// = &s[3..len];
let slice3 = &s[..];	// = &s[0..len];
```



字符串字面值就是一个切片；

```rust
let s = "Hello, world!";
```

这里 `s` 的类型是 `&str`，`&str` 是一个不可变引用，因此字符串字面值是不可变的；



## 结构体

一个存储用户账号信息的结构体：

```rust
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}
```

创建结构体实例和使用 `.` 运算符修改字段的值：

```rust
let mut user1 = User {    email: String::from("someone@example.com"),    username: String::from("someusername123"),    active: true,    sign_in_count: 1,};user1.email = String::from("anotheremail@example.com");	
```



### 字段初始化简写语法

变量与字段同名时，可以使用 **字段初始化简写语法**：

```rust
fn build_user(email: String, username: String) -> User {    User {        email,		// 设置为 build_user 函数 email 参数的值        username,	// 设置为 build_user 函数 username 参数的值        active: true,        sign_in_count: 1,    }}
```



### 从其他实例创建实例

适用场景：想要创建一个新的结构体实例，需要使用旧实例的大部分值，但要改变其中一部分；

```rust
let user2 = User {    email: String::from("another@example.com"),    username: String::from("anotherusername567"),    active: user1.active,    sign_in_count: user1.sign_in_count,};
```

还可以使用 `..` 语法表示剩下的从 user1 中取得：

```rust
let user2 = User {    email: String::from("another@example.com"),    username: String::from("anotherusername567"),    ..user1};
```



### 元组结构体

每一个元组结构体有其自己的类型，即使结构体中的字段有着相同的类型；

以此处代码为例，获取 `Color` 类型参数的函数不能接受 `Point` 作为参数：

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```



### 类单元结构体：没有任何字段

**类单元结构体**（*unit-like structs*）类似于 `()`，即 unit 类型；

通常在你想要在某个类型上实现 trait ，但不需要在类型中存储数据时使用；



#### 通过派生 trait 为结构体增加实用功能

```rust
#[derive(Debug)]	
// 跟着编译器报错提示，加上 #[derive(Debug)] 注解显式选择打印调试信息
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle { width: 30, height: 50 };
    // 跟着编译器报错提示，在 {} 中加入 :? 指示符告诉 println! 我们想要使用叫做 Debug 的输出格式
    println!("rect1 is {:?}", rect1);		
}
```



### 方法语法

Rust 中 **方法** 和 **函数** 是两个不同的概念：

| 相同点                                                       | 不同点                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 使用 `fn` 关键字和名称声明；<br>可以拥有参数和返回值，同时包含在某处调用该函数 / 方法时会执行的代码；<br> | 方法定义在结构体 / 枚举 / trait 的上下文中；<br>方法的第一个参数总是 `self`，代表调用该方法的实例本身；<br> |



#### `impl` 块

还是以之前的 `Rectangle` 结构体为例，为它增加一个求面积的方法 `area` 、和一个比较两 `Rectangle` 大小的方法 `can_hold`：

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    // 因为不需要获取所有权，只需要读结构体内数据，所以采用只读引用
    fn area(&self) -> u32 {		
        self.width * self.height
    }
    
    //实现 can_hold 方法，它获取另一个 Rectangle 实例作为参数
    fn can_hold(&self, other: &Rectangle) -> bool {		
        self.width > other.width && self.height > other.height
    }
}

fn main() {
    let rect1 = Rectangle { width: 30, height: 50 };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );
}
```



#### Rust 的自动引用和解引用功能

当使用 `object.something()` 调用方法时，Rust 会自动为 `object` 添加 `&`、`&mut` 或 `*` ，以便使 `object` 与方法签名匹配；



### 关联函数

关联函数即在 `impl` 块中定义的、**不以 `self` 作为参数的函数**；

经常被用作构造函数；

```rust
impl Rectangle {
    fn square(size: u32) -> Rectangle {
        Rectangle { width: size, height: size }
    }
}

// 调用时
let sq = Rectangle::square(3);
```



## 枚举 

### 定义枚举类型

任何一个 IP 地址，要么 IPv4 的、要么是 IPv6 的，而且不能两者都是，因此枚举数据结构非常适合这个场景，可以定义一个 `IpAddrKind` 枚举来表现这个概念并列出可能的 IP 地址类型，`V4` 和 `V6`：

```rust
enum IpAddrKind {
    V4,
    V6,
}

// 创建对象
let four = IpAddrKind::V4;
```



可以将任意类型的数据放入枚举成员，这样之后就可以把它们放在一起处理：

```rust
struct Ipv4Addr {
    // ... 
}

enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
    V4(Ipv4Addr)
}
```



### `Option` 枚举

Rust 为限制空值的泛滥，不设 `null` ，而是用 `Option` 枚举类型表达一个值存在或不存在的概念：

```rust
// <T> 是泛型类型参数
enum Option<T> {
    Some(T),	
    None,
}

let some_number = Some(5);
let some_string = Some("a string");

let absent_number: Option<i32> = None;	// 这里需要显式声明这个 Some 成员的类型
```



## 模式匹配

### `match` 运算流控制符

功能是将一个值与一系列的 **模式** 相比较，并根据相匹配的模式执行相应代码；

1. 模式可由字面值、变量、通配符和许多其他内容构成；
2. match 确保**所有可能的情况**都要得到处理，否则将报错 ^ pattern \`...\` not covered；
3. 每个分支相关联的代码是一个 lambda 表达式；

```rust
fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => {
            println!("Lucky penny!");
            1
        },
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```



#### 用 match 匹配分支绑定值

```rust
fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {	// 可以获取 Coin 枚举的 Quarter 成员中内部的州的值。
            println!("State quarter from {:?}!", state);
            25
        },
    }
}
```



#### 用 match 匹配 Option\<T\>

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    }
}

let five = Some(5);			
let six = plus_one(five);	// 6
let none = plus_one(None);	// None
```



#### `_` 通配符

`_` 模式会匹配所有的值，所以应该将其放置于其他分支之后（类似 C 的 default）

```rust
let some_u8_value = 0u8;
match some_u8_value {
    1 => println!("one"),
    3 => println!("three"),
    5 => println!("five"),
    7 => println!("seven"),
    _ => (),
}
```



### `if let` 控制流

适用情景：只匹配一个模式的值，忽略其他模式不用处理；

1. 可以认为 `if let` 是 `match` 的一个语法糖；
2. 可以在 `if let` 后接一个 `else` ，和 `match` 中的 `_` 通配符效果类似；

```rust
let some_u8_value = Some(0u8);

// 用 match 控制：
match some_u8_value {
    Some(3) => println!("three"),
    _ => (),
}

// 用 if let 控制：
if let Some(3) = some_u8_value {
    println!("three");
}
```









## 输入输出

### 命令行输出

`println!()` 和 `print!()` 都是命令行输出字符串的方法，第一个参数是格式字符串，后面是一串可变参数；

Rust 中格式字符串中的占位符是一对 `{}` ，在 `{}` 之间还可以放一个数字，意义是将把之后的一串可变参数当作一个数组访问，填入的这个数字就是数组下标，从 0 开始；

```rust
fn main() {
    println!("a is {0}, a again is {0}", a); 
}
```

想用原来的 `{` 和 `}` 需要打成 `{{` 和 `}}` ，其他转义字符还是前面加一个 `\` ；



## 其它

Rust 语言代码文件后缀名为 `.rs` ；

Rust 是一种 **预编译静态类型** 语言，这意味着你可以编译程序并将可执行文件送给其他人，而接受者不需要安装 Rust 就可以运行；
