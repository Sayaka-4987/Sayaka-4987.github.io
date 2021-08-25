---
layout:     post   				        # 使用的布局（不需要改）
title:      WPF：初探 MVVM 架构 			# 标题 
subtitle:   啥也没学会但是硬着头皮写了       # 副标题
date:       2021-08-25 				    # 时间
author:     YXWang 					    # 作者
header-img: img/post-bg-debug.png 	    # 这篇文章的标题背景图片
catalog: true 						    # 是否归档
tags:								    # 标签
    - WPF	
    - 施工中
---

# WPF：初探 MVVM 架构

前文：[WPF学习笔记 - 一些无用的想法 (Blog by WYX)](https://sayaka-4987.github.io/2021/08/20/WPF-beginner-notes/) 

使用 Visual Studio 作为 IDE，课件主要来自 [WPF入门基础教程合集__p9](https://www.bilibili.com/video/BV1mJ411F7zG?p=9) 

本文向您介绍怎么在**完全不懂 MVVM** 的情况下，用 WPF 做前端界面，C# 作为后端语言，写一个能跑的大创界面。感谢 ZX 和西鹏飞两位神仙队友的支持！



## 引入 MVVM Light 包

首先在 VS 中打开（或新建）一个 WPF 项目，在界面右侧的 “解决方案资源管理器” 中右击 “引用” ，选择 “管理NuGet程序包” ，然后在弹出的 `Nuget 包管理器` 窗口中搜索 `MVVM Light ` 安装。

喜欢敲命令行的朋友也可以选择在 `程序包管理器控制台` 里敲个 Install 命令；

![image-20210825125434458](.\media\image-20210825125434458.png)



## MVVM 架构的交互方式

完成安装 MVVM Light 框架后，WPF 项目中会自动生成一个 ViewModel 文件夹，里面有 MainViewModel.cs 这个文件。MainViewModel.cs 文件只包括一个 MainViewModel 类，顾名思义，该类是 MainWindow 窗口对应的ViewModel 。

![](https://img2018.cnblogs.com/blog/1463878/201811/1463878-20181122153357654-1043270966.png)

一些结论：

所有 ViewModel 都需要继承 ViewModelBase 类

所有 View 都需要继承 Window 类

所有 Model 都需要继承 ObservableObject 类



