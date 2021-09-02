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

主要内容来自 [Rust 教程\|菜鸟教程](https://www.runoob.com/rust/rust-tutorial.html) 和 [Rust 程序设计语言 简体中文版](http://120.78.128.153/rustbook/)



## 环境配置

Rust：这里用 [IntelliJ\_IDEA 社区版](https://www.jetbrains.com/zh-cn/idea/download/#section=windows) 作为 IDE ，需要安装 `Rust` 和 `toml` 两个插件，中间差什么可以无脑跟着 IDE 的提示装，新建项目选 Rust-Binary (application) ；

Cargo： 是 Rust 的构建系统和包管理器，用 Cargo 新建的项目会自动生成两个文件和一个目录：一个 Cargo.toml 配置文件，一个 src 目录，以及位于 src 目录中的 main.rs 文件；同时初始化一个配备 .gitignore 文件的 git 仓库；

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

可以在项目目录下，运行 `cargo doc --open` 命令，打开所有本地依赖提供的文档；

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

此处用一个 match 表达式 比较猜测的数字和秘密数字， match 由 **分支** 构成，一个分支包含：一个模式 ，表达式开头的值与分支模式相匹配时应该被执行的一块代码；Rust 获取提供给 `match` 的值并挨个检查每个分支的模式

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

Rust 是 **具有自动判断变量类型的能力** 的 **强类型** 语言

1. 声明变量后，变量类型就被确定了，不能再赋值其它类型的值；
2. Rust 不允许精度有损失的自动数据类型转换；
3. Rust 变量默认为 **不可变变量**； 

> Rust 语言设计这种机制的原因：若程序的一部分在**假设值永远不会改变**的情况下运行，另一部分在改变该值，那么前者可能就不会按照设计的意图去运转，且这种原因造成的错误难以事后发现。



```rust
let a = 123;	// 声明不可变变量，注意：不允许用 a = 456; 这样的语句私自修改不可变变量
let a = 456;	// 但允许重新绑定不可变变量

let b: u64 = 123;	// 声明指定类型的变量 
const c: i32 = 123;	// 声明完全不允许改变的常量

let mut d = 123;	// 声明可变变量
d = 456;
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
