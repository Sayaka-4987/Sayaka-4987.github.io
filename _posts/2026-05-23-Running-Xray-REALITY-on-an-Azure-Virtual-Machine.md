---
layout:     post                         # 使用的布局（不需要改）
title:      Vibe Coding 在 Azure 上跑通 Xray + REALITY VPN               # 标题 
subtitle:   一次比想象复杂得多的 Debug        # 副标题
date:       2026-05-23 				    # 时间
author:     WYX 					    # 作者
header-img: img/post-bg-the_great_wall_of_china.jpg 	    # 这篇文章的标题背景图片
catalog: true 						    # 是否归档
tags:								    # 标签
    - VPN
    - REALITY
    - Vibe Coding
---

# Vibe Coding 在 Azure 上跑通 Xray + REALITY VPN

这篇不是教程，这是一次我以为在“配一个工具”，结果最后变成在补课计算机基础知识，debug Linux、systemd 和网络行为的过程。

顺便说一句，这个项目一开始是典型的 vibe coding。我就这样打开我的 GPT，请 G 老师给我傻瓜复制脚本，改几行参数，然后一路运行。你大概也能猜到结果。表面上是跑起来了，但其实完全没通。

## 1. 一开始的假象非常具有迷惑性

Xray 安装好了，服务也启动成功了。systemctl status 显示 active running。客户端配置也填得很完整。

然后我点了 v2rayN 的测试按钮，delay = -1

没有报错，没有提示，就是不工作。

这种状态特别危险，因为它会让你下意识觉得，“是不是我哪一行参数写错了”，然后开始反复改配置。

但这次的问题，完全不在配置。是配置根本没被用。

日志里有一句话：

> Reading config: /usr/local/etc/xray/config.json

但我写的是 server.json。

也就是说，我所有的配置其实是“对的”，但系统根本没加载它。Xray 实际运行的是一个空的 config.json。

这是一个很典型但很隐蔽的错误类型：你做对了事情，但系统没用。

在工程里这种问题的危险点是，它不会报错，也不会 crash，只是安静地做错。

修复也很简单，直接把 server.json 覆盖到 config.json。

但这只是第一层。

## 2. 第二层问题是 443 端口

修完配置之后，服务直接起不来了。

报错是：

> listen tcp 0.0.0.0:443: bind: permission denied

这时候问题开始从“应用层配置”转成“操作系统权限”。在 Linux 里，1024 以下的端口需要特权。systemd 的 service 文件里跑的是 nobody 用户，这是一个合理的默认安全设置。

但它意味着，我不能绑定 443 端口，这时候我以为已经接近终点了。因为这是一个“我知道答案”的问题。

## 3. 第三层问题，是我以为我修好了，但其实没有

标准解法是：

> setcap cap_net_bind_service=+ep /usr/local/bin/xray

理论上这会允许非 root 进程绑定低端口。我执行了。看起来也成功了。但服务还是起不来。

## 4. 真正的坑，在 systemd

service 文件里有一行：

> NoNewPrivileges=true

这个配置的语义是，进程在启动后不能获得新的权限，包括 capabilities。

也就是说，我刚刚手动赋给 xray 的能力，在运行时被 systemd 拦掉了。

这一步非常隐蔽，因为：

1. 没有明显错误提示
1. capability 确实存在
1. 大部分教程不会提到

从外部看，一切都“看起来正确”，但系统行为仍然是错的。

## 5. 最终的修复，是改 systemd

加上 AmbientCapabilities 和 CapabilityBoundingSet，并且把 NoNewPrivileges 关闭。

服务瞬间恢复正常。

监听 443 成功，客户端连接也正常。

这里有一个我觉得比“如何配置 REALITY”更重要的点。

这整个问题，不是一个问题，而是三层叠加：

1. 配置文件路径错了
2. 操作系统端口权限
3. systemd 安全机制阻止 capability
   
任何一个单独存在都很好解决，但叠在一起的时候，就会变成一个典型的“everything looks fine but nothing works”的场景。

## 6. 还有一个额外的坑是 SNI

很多教程会推荐用 google.com、microsoft.com、amazon.com 这种大厂域名。

我一开始也是这么做的，后来经过 ZX 严厉批评，我发现不是一个好选择，不是说不能用，而是这些域名的流量特征太典型，被用得太多，反而更容易被 GFW 快速抓捕。

REALITY 的目标不是“伪装成最像正常流量的东西”，而是“变成不会被注意到的东西”。更好的策略是选一些正常支持 TLS 的中小型网站，甚至一些小众二次元亚文化社区站点。

重点不是像谁，而是不显眼。

## 总结

回头看，这整个过程其实很典型，一开始我把它当作一个“配置问题”。但随着深入，它变成了：

- 我是否理解 Linux 如何控制端口权限
- 我是否理解 systemd 的安全边界
- 我是否意识到系统可能根本没加载我的配置

这和在 Azure 上 debug 一个 production incident，其实没有本质区别。

如果要总结一条经验的话，大概是这样，当你看到一个系统“没有报错，但就是不工作”的时候，优先怀疑三件事情：

- 配置有没有被真正使用
- 端口和权限有没有问题
- 运行环境有没有额外的安全限制

而不是第一时间去改参数。