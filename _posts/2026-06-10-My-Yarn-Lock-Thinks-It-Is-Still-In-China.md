---
layout:     post                           # 使用的布局（不需要改）
title:      我的 yarn.lock 还以为自己在中国                    # 标题 
subtitle:   一个关于 npm 镜像、CI 超时、和数字移民遗留问题的技术复盘        # 副标题
date:       2026-06-10                     # 时间
author:     WYX                         # 作者
header-img: img/post-bg-deprecated-server.jpg         # 这篇文章的标题背景图片
catalog: true                             # 是否归档
tags:                                    # 标签
    - React
    - JavaScript
    - npm
    - yarn
    - 镜像站
---

# 我的 yarn.lock 还以为自己在中国 

## 0. 故事要从这里开始说起

我有一个自制的用于玩“克苏鲁的呼唤”（你说得对，但是 CoC 是一款基于洛夫克拉夫特宇宙的恐怖跑团游戏）的第 7 版规则的随机数生成网站。

项目链接：[CoC7thdice](https://sayaka-4987.github.io/CoC7thdice/)

这个页面是我还在上大学的时候用 React 写的，开发背景是，当时 QQ 封禁了很多 QQ 群里的骰子机器人，我选择给自己留一个静态网页作为骰子功能的 fall back，不要让腾讯害得大家玩不了桌游。所以，我还顺手提供了一些骰子常见娱乐功能，比如抽塔罗牌占卜。

我抽了一张塔罗牌，命运之轮，逆位，然后我对着文案寻思着，这文案什么意思，什么叫“边疆的不行”？

可以想象这种胡言乱语大概是来自于多年前的野生 OCR 软件认错字了，然后使用者在把繁体中文转换为简体中文的时候，没想到它的文化水平这么低，没有校对出这个文盲错误。

然后，我开始试图更新一个 4 年没动过的个人网站，我的 GitHub Actions 跑起来以后，`npm install` 卡了 14 分钟、20 分钟、30 分钟……最后，过了一个小时，`npm install` 终于以 `ENOTFOUND` 失败了。

我以为是 dependency hell，一个四年没动的 JavaScript 项目，经历了浏览器内核版本升级，dependency 全军覆没也不奇怪，结果……没有那么简单。

## 1. 语雀、淘宝、开源精神与时代眼泪

这个仓库最后一次成功部署是 2022 年，当时我人在中国，使用的 npm 源是 `registry.nlark.com`，语雀团队维护的 npm 镜像，曾经是国内的 JavaScript 开发者常用的源之一。 

回到 2022 年，这个配置完全合理，中国国内直连 `registry.npmjs.org` 会遇到一些问题大家都懂，不再细说，换镜像是基本操作。 

但 2024 年 nlark.com 域名停止服务了，DNS 不能解析，而我的 yarn.lock 里，60 个 package 还指向这个已经不存在的地址。 

## 2. 为什么这么简单的问题还会卡一个小时呢

这是一个我也反应了一会儿的点，npm 遇到 DNS 解析失败是静默失败。

当 `npm install`（或 `yarn install`）尝试从一个已经无法解析的域名下载包时，它不会瞬间 failed 把 pipeline 变红，而是

1. 尝试 DNS 解析
2. 等待超时（通常 30s-60s）
3. 重试
4. 再等待
5. 再超时
6. 最终报错 `ENOTFOUND`

一次 60 秒的过程乘以 60 个包，刚好就是一小时。

> 实际是并发请求 + 重试退避叠加的结果，不是严格的串行乘法，但数量级对就行。

在 CI 日志里，只能看到：

```bash
Run npm install --legacy-peer-deps
```

然后就什么都没有，没有进度条，没有报错，没有任何输出。npm 它只是安静地在那里死了，等待一个再也不会回应的服务器。 

## 3. 为什么我没有第一时间想到这个 root cause

因为我已经不在中国了，2025 年我搬到了网络环境完全不同的地方，日常开发中不再需要考虑本地镜像换源问题，GFW 对我来说已经是一个不需要随时想着的事情，但 yarn.lock 不知道这些事，yarn.lock 它只是一个快照，技术中立、记忆可靠，忠实保存了它被创建时的一个小世界。

在它的认知里， `registry.nlark.com` 仍然是一个可以访问的、可信赖的源。 

所以 root cause 就是，我润了，但我的 lockfile 没润。 

更讽刺的是，就在几天前，我还在教我的好朋友，遇到 npm 出问题，首先要怀疑 GFW，然后我自己一脚踩进了 GFW 遗留问题的变体里。 

## 4. 然后怎么修好的

修复本身很简单： 

- yarn.lock 全局替换 `registry.nlark.com`，换成官方源
- CI workflow 把之前混用的 npm 命令对齐为 yarn

提交，推送，`yarn install` 这个步骤终于通过了，整个过程不到 5 分钟，而我之前浪费了超过一个小时。

当然，这只是第一层。

后面还有 Node 18 终止服务导致的证书链失效，postcss 与 Node 20 的 exports 兼容性问题，一个不知何时丢失的 index.html、以及经典的 ERR_OSSL_EVP_UNSUPPORTED。

但是那些至少是常规版本升级的必经之路，而“镜像已死，但 lockfile 不知道”这种问题，喜剧效果更佳。

## 5. 国内 npm 镜像简史，以及怎么自查你的项目

简单梳理一下国内 npm 镜像的变迁，给不了解背景的读者：

| 时期       | 镜像                    | 状态     |
| ---------- | ----------------------- | -------- |
| ~2014–2021 | registry.npm.taobao.org | 已迁移   |
| 2018–2024  | registry.nlark.com      | 已停服   |
| 2021–now   | registry.npmmirror.com  | 现行主流 |

顺带提醒一下，如果你的项目里也有指向前两个域名的 lockfile，现在就应该更新，不要等到 CI 卡一个小时才想起来。快速排查命令：

```bash
# 检查 yarn.lock
grep -c "registry.nlark.com" yarn.lock
grep -c "npm.taobao.org" yarn.lock

# 检查 package-lock.json
grep -c "registry.nlark.com" package-lock.json
grep -c "npm.taobao.org" package-lock.json

# 检查 .npmrc / .yarnrc
cat .npmrc 2>/dev/null
cat .yarnrc 2>/dev/null
```

如果输出不为 0，那么，恭喜你，你的 lockfile 也还以为自己在中国。 

## 6. 更深一层的教训

这个 bug 本身不复杂，但它涉及了一个容易被忽视的问题：lockfile 是快照，它有地理记忆。 

它记录的不只是版本号和依赖关系，还有 resolved URL，也就是“这个包当时是从哪里下载的”，当你更换网络环境（搬家、换国家、换公司、换 CI 平台），lockfile 里的 URL 可能已经指向一个不存在的地方。 

这不仅限于中国的镜像站，任何私有 registry、公司内部源、或者已经下线的第三方镜像都可能产生同样的问题。所以最佳实践建议是： 

- 每次迁移开发环境后，检查 lockfile 中的 resolved URL
- CI 环境和本地环境使用相同的 registry
- 如果项目长期不维护，重新激活前先 audit lockfile
- 在 .npmrc 或 .yarnrc 中显式声明 registry，不要依赖全局配置

## 7. 写在最后

这个骰子 + 塔罗牌网站上一次成功部署是 2022 年。

那时候我还在北京，用着国内 npm 镜像源，有两个固定的跑团群（这个骰子网站就是给跑团用的），一切正常。

四年后，我在太平洋的另一头，镜像已经关停，一起跑团的朋友工作以后淡出了圈子，语雀的 DNS 不再解析，但 yarn.lock 还忠实地保存着 60 个指向我曾经拥有的一个旧世界的 URL，安静地等待一个永远不会回来的响应。 

有时候，CI/CD 配置文件比人更念旧。 

顺便一提，在等 `npm install` 的那一个小时里，我修了一组塔罗牌数据的 OCR 错别字、给前同事们写了 2 条 LinkedIn 评论、翻了 18 页 mutuals 找回 4 个失联前同事、研究了加州卷和西雅图卷的起源、并且设计了一套模块化的预制工作日午餐酱料轮换系统。 

npm 一件事都没完成，垃圾！
​‌
