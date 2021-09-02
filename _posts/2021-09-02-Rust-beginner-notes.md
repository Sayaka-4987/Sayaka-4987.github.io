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

Rust：这里用 [IntelliJ\_IDEA 社区版](https://www.jetbrains.com/zh-cn/idea/download/#section=windows) 作为 IDE ，需要安装 `Rust`和`toml` 两个插件，中间差什么可以无脑跟着 IDE 的提示装，新建项目选 Rust-Binary (application) ；

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
