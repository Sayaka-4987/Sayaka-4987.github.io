---
layout:     post                         # 使用的布局（不需要改）
title:      踩坑：从家用电脑登录 Microsoft 账户到重装系统                  # 标题 
subtitle:   从前车马很慢，书信很远，一张番茄花园光盘解决一辈子的 Windows XP # 副标题
date:       2023-10-09                   # 时间
author:     WYX                          # 作者
header-img: img/post-bg-keybord.jpg      # 这篇文章的标题背景图片
catalog: true                            # 是否归档
tags:                                    # 标签
    - 操作系统
    - Windows 10
---

# 踩坑：从家用电脑登录 Microsoft 账户到重装系统

## 故事的背景

- 家里的台式机不是我攒的（华硕主板预装 Windows 10，没有 Office）
- 天下苦 WPS 久矣，我妈还只能用 WPS 办公
- 我上大学用的联想小新笔记本（预装 Windows 10 + Office 2019）坏掉了
- 为了生活便利，我打算将自己笔记本对应的 Office 2019 终身订阅改绑定到家里台式机上

## 触发原因

1. 家里台式机只有一个 **无密码，未设置 PIN** 的 **本地管理员账户** 
2. 在该条件下用 Office **登录了 Microsoft 账户**（P.S. 此时本地管理员账户已经自动被 Microsoft 账户吞并，但用户是没有感知的）
3. 电脑一切正常，我们没有设置 PIN 和密码就把电脑休眠了
4. 再次解锁时发现陷入恶性 BUG，无限循环登录界面，点了登录没有反应

## 如何确认是这个 BUG

1. 用辅助功能读屏幕，听到“链接：登录”而不是“登录”

## 为什么会这样呢？

前提：只有本地管理员账户的时候，系统权限由本地管理员说了算；登录过 Microsoft 账户以后，系统权限由 Microsoft 更信得过的 Microsoft 账户做主。

卡 Bug 的情况：本地管理员已经被合并了，以前本地管理员没有密码，新来的 Microsoft 账户又没有设置本机密码。系统出于安全考虑，不敢让你像以前一样继承本地管理员的无密码认证，也没有 PIN 和密码证明你确实是这个能做主的 Microsoft 账户，逻辑上也没想到有人能卡到这个情况下来……

> 表现测试：本地管理员的账户会在登录 Microsoft 账户的时候被强制的、用户无感知、无提示的吞并，Windows 10 系统对本地管理员和新 Microsoft 账户的合并是各取一部分，主要权限和身份取后者（Microsoft 账户），但本地管理员的用户名、注册表 ProfileImageName、和用户文件夹路径会保留

> 搜索发现 Windows 11 还存在这个情况，每周都有新受害者，希望尽快修复，或者强制登录 Microsoft 账户后必须立刻至少设置一个密码也行

## 解决方案

### 方法 0: 预防

1. 在“触发原因”的第 2 步，登录 Microsoft 账户后，立刻设置密码和 Windows Hello PIN，不要锁屏不要休眠
2. 还是建议从装机开始就用 Microsoft 账户登录

### 方法 1: 换一个能连接微软服务器的网络环境

使用前提：记得自己 Microsoft 账户的密码

部分用户反映：连接一个有网络的设备后就能用 Microsoft 账户正常登录。“能连接微软服务器的网”就是字面意思，视地区不同可能需要开移动/联通/电信的手机热点，也可能需要使用一些魔法

脑筋急转弯：实际上卡登录界面时打不开网络设置，一个比较好的点子是使用手机数据线开启 USB 共享网络，就能把网络环境同步给电脑

### 方法 2: 用高级启动选项、安全模式恢复系统

使用前提：你的主板/操作系统支持你走到这些地方

高级启动选项：在无限登录的界面，长按 Shift 键，按重新启动，可以尝试进入安全模式 / 修复此系统 / 回退到还原点等功能

安全模式也可以通过在物理开机后长按 F2/F8/Del 等键（取决于主板厂家）进入

但 Windows 10/11 的高级启动和安全模式都卡登录账户的权限，如果电脑只有本地管理员是能正常进入的，但如果电脑登录过任何一个 Microsoft 账户，就必须需要任何一个 Microsoft 账户授权，此时请参考方法 1 的网络设置

（很明显前两种办法我都用不了，本地管理员已经无了，Microsoft 账户密码存密码管理器里了，密码管理器进不去了）

### 方法 3: WinPE 环境重新设置系统登录账户的密码 

使用前提：你曾经制作过 Windows 10 版本匹配你的电脑的 WinPE 启动介质

关于 WinPE 环境，官方文档有比较详细的介绍，请看：[Windows PE (WinPE) 概述](https://learn.microsoft.com/zh-cn/windows-hardware/manufacture/desktop/winpe-intro?view=windows-11)

### 方法 4: 重装系统

使用前提：你有其他能开机的 Windows 系统设备，有一个大于 8GB 容量的 U 盘（可用光盘和光驱代替）

#### 官方源制作 Windows 10 安装媒介

为了避免全家桶带一些不干净的东西，装系统还是得参考官方文档：
1. [创建适用于 Windows 的安装介质](https://support.microsoft.com/zh-cn/windows/%E5%88%9B%E5%BB%BA%E9%80%82%E7%94%A8%E4%BA%8E-windows-%E7%9A%84%E5%AE%89%E8%A3%85%E4%BB%8B%E8%B4%A8-99a58364-8c02-206f-aa6f-40c3b507420d)（原理说明书）
2. [下载 Windows 10](https://www.microsoft.com/zh-cn/software-download/windows10)（操作步骤和安装文件下载链接）

上述文档基本是傻瓜式操作，鉴于大多数主机不再提供光驱，建议选择 U 盘作为安装媒介。

官方安装程序会自动格式化你的 U 盘变成安装盘，只有一个坑点：“您现有的 Windows 版本” == “您准备要安装 Windows 10 的设备现在跑的是什么 Windows 版本”

对于绝大多数 Windows 10 家庭中文版用户，请您还是要选择 “Windows 10” 而不是 “Windows 10 Home China​”，可能有些反直觉。

#### 系统安装过程

> 写给小白：进 BIOS 只是选择你想从什么启动，你可以选择把 U 盘优先级放在硬盘前面。然后在安装完成后拔掉 USB，也可以仅此一次选择 U 盘运行。
> U 盘启动后一路确认傻瓜化操作就行，实在心里没底就搜个视频看看，看到“海内存知己”就是完成了
 
制作好安装 U 盘后插电脑上，开机长按 F2/F8/Del 等键（取决于主板厂家）进 UEFI-BIOS，选择用 U 盘设备启动，思考一下格式化哪个硬盘分区，没什么特别的

#### 驱动安装过程

Windows 10 的官方安装介质哪儿都好，就是不会像国产全家桶一样打包赠送驱动……

1. 如果电脑联网：先把所有 Windows 更新都下了，系统更新能自动的补上大多数必需的声卡网卡显卡驱动，全自动安装
2. 如果主板是网购的：找客服，让客服把所有驱动和工具都交出来，半自动安装
3. 如果电脑没网：把其他有网的设备用数据线插电脑上，开启 USB 共享网络，然后参考 1 和 2
4. 如果没有其他有网的设备：去网吧开一个小时，下对应网卡型号的驱动，借个 U 盘拷回家里，在“设备管理器”全手动安装

## 吐槽

备份真的是好习惯，Windows 用户应该家中应该常备 WinPE U 盘……  
