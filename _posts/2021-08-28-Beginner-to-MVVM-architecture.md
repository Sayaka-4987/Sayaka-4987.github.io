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
---

# WPF：初探 MVVM 架构

前文：[WPF学习笔记 - 一些无用的想法 (Blog by WYX)](https://sayaka-4987.github.io/2021/08/20/WPF-beginner-notes/) 

使用 Visual Studio 作为 IDE，课件来自 [WPF入门基础教程合集__p9](https://www.bilibili.com/video/BV1mJ411F7zG?p=9) 

本文向您介绍怎么在**完全不懂 MVVM** 的情况下，用 WPF 做前端界面，C# 作为后端语言，写一个能跑的大创界面。感谢 ZX 和西鹏飞两位神仙队友的支持！



## 引入 MVVM Light 包

首先在 VS 中打开（或新建）一个 WPF 项目，在界面右侧的 “解决方案资源管理器” 中右击 “引用” ，选择 “管理 NuGet 程序包” ，然后在弹出的 `Nuget 包管理器` 窗口中搜索 `MvvmLight`  安装。

喜欢敲命令行的朋友也可以选择在 `程序包管理器控制台` 里敲个 Install 命令；

![fqM6t.png](https://ss.im5i.com/2021/08/25/fqM6t.png)



## MVVM 架构的交互方式

完成安装 MVVM Light 框架后，WPF 项目中会自动生成一个 ViewModel 文件夹，里面有 MainViewModel.cs 这个文件。MainViewModel.cs 文件只包括一个 MainViewModel 类，顾名思义，该类是 MainWindow 窗口对应的ViewModel，MVVM 类间的交互方式请看下图：

![fqpeL.png](https://ss.im5i.com/2021/08/25/fqpeL.png)

（此处略去大概 500 字，有兴趣自己搜索引擎看看 MVVM 的百科啥的）

在 C# 中使用 MvvmLight 的类需要在开头引入包：

```c++
using GalaSoft.MvvmLight;
```

因为~~我也说不明白原理~~，所以我们**快进到结论**：

1. 所有 `ViewModel` 都需要继承 <u>GalaSoft.MvvmLight.ViewModelBase</u> 类；
2. 所有 `View` 都需要继承 <u>GalaSoft.MvvmLight.Window</u> 类；
3. 所有 `Model` 都需要继承 <u>GalaSoft.MvvmLight.ObservableObject</u> 类；
4. 使 `ViewModel` 拥有所有它需要的 `Model` 成员，  `ViewModel` 需要完成初始化/获取、修改 `Model` 的值，提供一些 public 的方法以供 `View` 绑定和命令 `ViewModel` 进行操作这几项重要任务；
5. 使 `View` 拥有它相对应的 `ViewModel` 成员，然后在 `View` 的构造函数中初始化该 `ViewModel` 成员；



## 数据绑定和命令

这里的代码我主要是参考 [WPF入门基础教程合集__p9](https://www.bilibili.com/video/BV1mJ411F7zG?p=9) 写的，如果看例子看不明白可以去看原视频；

### 如何在 View 中显示和修改数据

这里的例子是我们要在界面中显示一个可供用户修改的路径变量 targetPath。

`ViewModel` 层代码如下：

```c#
public class MainViewModel : ViewModelBase
{
    public MainViewModel()  // MainViewModel的构造函数
    {
    	……
    }

    private string targetPath = @"D:\";		// 私有的成员
    public string TargetPath				// 公开的获取和修改方法
    {
        get { return targetPath; }
        set
        {
            targetPath = value;
            RaisePropertyChanged();
        }
    }
    ……
}
```

再来看 `View` 层的 MainWindow.xaml 文件，首先设定此处引用数据的上下文是 MainViewModel 这个 `ViewModel` ，就可以在此后访问 MainViewModel 内的公开属性/方法：

```xml
<Window x:Class="项目名.MainWindow"
        d:DataContext="{d:DesignInstance Type=local:MainViewModel}">
    ……
</Window>
```

在界面中，我们可以用一个  TextBox 显示该路径，并为它的 Text 属性绑定能获取和修改 targetPath 变量的 TargetPath 方法：

```xml
<TextBox Text="{Binding TargetPath}" Style="{DynamicResource StyleTextLine}" Margin="10,0,10,10"></TextBox>
```

这时点击运行，我们就可以在界面中随意修改 targetPath 的值了。

<img src="https://ss.im5i.com/2021/08/25/fvvTR.png" alt="fvvTR.png" style="zoom: 50%;" />

### 按钮绑定事件

在 `View` 的 .xaml 文件的设计页面中双击按钮，IDE 会在对应的 .xaml.cs 文件中自动生成一个相应的事件函数；

```c#
private async void SearchButton_Click(object sender, RoutedEventArgs e)
{
	
}
```

之前的结论写了 `ViewModel` 应当作为 `View` 的成员被初始化，这样我们即可在 `View` 的 .xaml.cs 文件中调用 `ViewModel` 中的函数：

```c#
public partial class MainWindow : Window
{
    MainViewModel viewModel;	// 定义 ViewModel 成员

    public MainWindow()
    {
        InitializeComponent();
        viewModel = new MainViewModel();	// 初始化 ViewModel 成员
        DataContext = viewModel;			// 设定数据上下文为 ViewModel 
    }

    private async void SearchButton_Click(object sender, RoutedEventArgs e)
    {
        viewModel.SearchKeyword();			// 调用 ViewModel 提供搜索的方法
    }
}
```



## 附赠一点实际应用中的多线程知识

这里遇到的问题实际上是 ZX 解决的，但他懒得写博客……

### 加锁

在为大创编写的程序中，需要维护一个 ObservableCollection 动态数据集合；当我们对这个集合进行增删查改的时候，整个界面都会卡住，这是因为在 “什么额外的工作也没做” 时，主线程被维护集合的操作占用了，就无法再接收 `View` 界面的放大缩小命令；

这里我们需要给这个集合加锁：

```c#
_itemsLock = new object();
Items = new ObservableCollection<Item>();
```

然后在 `ViewModel` 的构造函数中用 System.Windows.Data 包提供的 BindingOperations.EnableCollectionSynchronization 方法设定他们的绑定关系：

```c#
BindingOperations.EnableCollectionSynchronization(Items, _itemsLock);
```

之后，用 lock() { ... } 代码块包裹任何对动态集合的增删查改操作：

```c#
lock (_itemsLock)
{
    // Once locked, you can manipulate the collection safely from another thread
    Items.Add(new Item());
    Items.RemoveAt(0);
}
```

### async / await 自动等待耗时操作完成

用 async 关键字声明的函数就会成为异步函数， async 关键字需要配合 await 关键字使用；

async 这种简单的异步函数的具体行为是，程序跑到标志了 await 的区域时，会隔一会儿就检测它等待的值是不是可用状态，若可用，才会执行此后操作；若不可用，程序可以先进行其他不需要等待这个返回值的部分，此处我们的 “耗时操作” 是关键字查找，需要等待它完成：

```c#
private async void SearchButton_Click(object sender, RoutedEventArgs e)
{
    await Task.Run(viewModel.SearchKeyword);
    // 等到完成查找过程，再显示查找结果
    SearchResultWindow resultWindow = new SearchResultWindow();	
    resultWindow.DataContext = viewModel;
    resultWindow.ShowDialog();
}
```

由 async 声明的异步方法，其返回类型只能是 `void`、`Task`、`Task<TResult>` 之一，此处示例的返回值类型是 void ，主要难点还是需要判断使用这两个关键字来防止耗时操作阻塞当前线程的场合。



当天晚上更新：费时间的可以全加上 async / await，加就完事了

