---
layout:     post   				        # 使用的布局（不需要改）
title:      折腾一个 manjaro XFCE 操作系统环境 		# 标题 
subtitle:   建议不要乱动显示设置			# 副标题
date:       2022-03-06 				    # 时间
author:     WYX 					    # 作者
header-img: img/post-bg-os-metro.jpg 	    # 这篇文章的标题背景图片
catalog: true 						    # 是否归档
tags:								    # 标签
    - 操作系统
    - VMware
---

# 折腾一个 manjaro XFCE 操作系统环境

物理机 Windows 10 系统，环境使用 VMware Workstation

目前已知问题1是 VMtool 不会自适应分辨率，已知问题2是~~我不太会用 Linux~~

## manjaro 官网下载 ISO 文件

[https://manjaro.org/get-manjaro/](https://manjaro.org/get-manjaro/)

## 创建虚拟机

创建新的虚拟机 -> 手动选择刚才下载的iso文件 -> 客户机操作系统选 Linux，版本选“其他 Linux 5.x 内核 64 位” 

## 进入 livecd 模式安装系统 

安装完进去看到桌面，此时系统还没装上，双击桌面“Install Manjaro Linux”的图标

时区、语言、键盘、密码按需设置，差点选了个维吾尔语键盘……

设置分区选择“交换到文件”

## 更改显示效果

窗口特别小的可以重装一下 VMtool，用 pacman 卸载原来的再 clone一份即可

不过我这里重装了 VMtool 还是窗口特别小，最后妥协成在“显示”设置改了分辨率……

字体可以一会儿 `sudo pacman -S #找自己喜欢的` 

不要把缩放的1x改成2x。。。窗口会大的操作不了，费半天劲才挪回去

## pacman 包管理器基本命令

可以 `pacman -h` 看帮助；应该不会有人还不知道这个 `$` 符号不要复制吧

```bash
$ pacman {-h --help}
$ pacman {-V --version}
$ pacman {-D --database} <选项> <软件包>
$ pacman {-F --files}    [选项] [文件]
$ pacman {-Q --query}    [选项] [软件包]
$ pacman {-R --remove}   [选项] <软件包>
$ pacman {-S --sync}     [选项] [软件包]
$ pacman {-T --deptest}  [选项] [软件包]
$ pacman {-U --upgrade}  [选项] <文件>
```

常用的：

```bash
$ pacman -Ss [软件包] # 搜索
$ pacman -S  [软件包] # 安装
$ pacman -Rs [软件包] # 卸载
$ pacman -Syu        # 更新
```

## pacman 换成国内镜像，初始化

~~真希望哪天再也不用折腾这些~~

```bash
$ sudo pacman-mirrors -i -c China -m rank  # 会列出中国地区镜像列表，选自己喜欢的
$ sudo pacman -Syyu
```

## 安装常用工具

因为还有物理机，所以这里就随便装点

```bash
$ sudo pacman -S code # VSCode，问就是 Vim 不会

$ sudo pacman -S fcitx-im  # fcitx 框架，默认全部安装
$ sudo pacman -S fcitx-configtool
$ sudo pacman -S fcitx-googlepinyin  # 安装谷歌拼音输入法，装完找搜索引擎配置一下需要重启
```

到这里 manjaro 装差不多了



## 后续

经过两天的折腾和查资料，发现 VMware 的硬件辅助虚拟化并不能很好的解决 64 位物理机搭虚拟机里用 gcc 跑 linux0.11远古版本的 32 位汇编 .S 代码这个任务

所以操作系统作业还是用 bochs 和 qemu 这种纯模拟器做吧，这个 manjaro 就留着当玩具了（

