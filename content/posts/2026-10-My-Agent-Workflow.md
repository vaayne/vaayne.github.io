---
date: "2026-10-20T16:00:00+08:00"
draft: false
slug: "my-agent-workflow"
title: "我的 Agent 工作流：在哪都和本地一样"
category: ["技术"]
tags: ["AI", "Agent", "BB", "Magpie", "Claude Code", "Codex", "Pi"]
description: "用 BB + Magpie 在多台机器上统一使用 Pi、Codex 和 Claude Code，随时随地都和在本地一样。"
---

经常有朋友问我，是怎么同时用这么多 Agent 的。

聊下来发现，很多人用 Agent 的方式还和半年前的我一样：打开一个 Terminal，跑一个 TUI，盯着它干活。Agent 被限制在一台本地电脑上，电脑一合，活就停了；人一离开工位，就只能干等。

现在的 Agent 已经能连续干好几个小时的活了，我希望它在哪都能和在本地一样好用。这篇文章记录一下我现在的做法。

## 我的配置

我用的 Agent 主要有三个：[Pi](https://github.com/earendil-works/pi)、Claude Code 和 Codex。管理它们的是 [BB](https://github.com/get-bb/bb)，一个 Agent IDE；管理订阅和 API Key 的是 [Magpie](https://github.com/yetone/magpie)。

它们分布在三台机器上：

- **MacMini**：我的 Homelab，常年开机，BB Server 和 Magpie 都跑在这里。
- **MacBook**：主力工作设备。
- **VPS**：线上的机器，随时可以用。

![我的 Agent 工作流架构](/images/2026/10/my-agent-workflow-architecture.png)

我平时只打开 BB 的网页或者手机 App，新建线程时选好 Agent 和机器，剩下的事情都在这个界面里完成。

## 为什么选择 BB

### Work from everywhere

BB Server 跑在 MacMini 上，通过 bb connect 登录账号以后，就会得到一个 `getbb.app` 的地址。手机、浏览器、其他机器都从这个地址连回来，不用开端口，也不用折腾内网穿透。

![BB：Keep working from anywhere](/images/2026/10/my-agent-workflow-bb-anywhere.jpg)
_图片来自 BB 官网_

我之前把 BB 部署在 EC2 上，自己套了一层 OAuth，最后还是撤了：一个能执行任意命令的服务挂在公网上，总是不踏实。bb connect 省掉了这些麻烦。

### 统一的使用体验

Claude Code、Codex、Pi 都被原样接进了 BB，用的还是它们自己的原生 Agent，只是住进了同一个界面。会话、Diff、文件都用同一种方式看，Web UI 和手机 App 的体验也一致。我再也不用记三套 CLI 的快捷键和配置文件放在哪里。

### 可以随意扩展

BB 和 Pi 一样，可以通过插件改造成自己喜欢的样子。下面要讲的 Magpie 扩展，就是我自己给 BB 写的插件。

### 优秀的 Agent 编排

这是我最喜欢 BB 的一点。BB 提供了一个 `bb` CLI，Agent 可以用它创建子线程、等待结果，所以 Agent 之间能互相派活。

我的每个项目都有一个长期在线的 Lead 线程，它负责拆任务和派活，我只和它讨论方向。我的分工规则很简单：Codex 负责实现，再用另一家的模型做 Review，Claude 写的给 GPT 看，GPT 写的给 Claude 看。

举个真实的例子。10 月 16 日睡前，我把一个项目的需求交给了 Lead，然后就去睡了。凌晨 0 点到 3 点，它派出了 27 个 Codex 子线程，一个实现，一个审查，发现问题再派一个去修。那一天一共有 78 个线程，其中 76 个是 Agent 派给 Agent 的。

![2026-10-16 的线程时间线](/images/2026/10/my-agent-workflow-timeline.png)

左上角 MacMini 上那一串蓝点，就是我睡觉时 Lead 派出去的子线程。

## Magpie：统一订阅和 API Key

BB 统一了 Agent，Magpie 统一了订阅和 API Key。

以前各种 Provider 的配置散落在各处：Codex 一份，Claude Code 一份，Pi 又是一份，每台机器还要再配一遍。现在所有订阅账号和 API Key 都放在 Magpie 里统一管理，额度还剩多少也能一眼看清。

但 Magpie 只跑在 MacMini 上，其他机器怎么用？为此我写了一个 [bb magpie 扩展](https://github.com/vaayne/agent-kit/tree/main/bb-extensions/magpie)。所有机器本来就连着 BB Server，扩展就让模型请求也走这条已有的连接，转发到 MacMini 上的 Magpie。

这样一来，只需要在 BB Server 那台机器上配好 Magpie。以后任何机器加入这个 BB 集群，都不需要再配置 Provider。订阅账号也只在一台机器上登录，减少了封号的风险。

扩展还在 BB 里加了一个用量页面，所有机器、所有 Agent 的用量和订阅额度都在这一页：

![BB 里的 Magpie 用量页面](/images/2026/10/my-agent-workflow-magpie-usage.jpg)

## 最后

这是我 2026 年 10 月的工作流，过几个月大概又会变。但有一点我很确定：Agent 不该被关在一个 Terminal 里，也不该被绑在一台电脑上。

如果你也在用 BB，在插件市场里搜 `magpie` 就能装上这个扩展。也欢迎来聊聊你的 Agent 工作流。
