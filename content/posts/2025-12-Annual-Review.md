---
date : '2025-12-26T13:02:22+08:00'
draft : false
slug : '2025-annual-review'
title : '【2025 年终总结】巨浪袭来时：适应、重塑与开源'
category : ['年终总结']
tags : ['AI', 'Cherry Studio', 'YU7', '2025']
description : '2025 年终总结'
cover:
    image: "https://s3.vaayne.com/vaayne/images/2026/01/1767259922-Gemini_Generated_Image_tcx7y4tcx7y4tcx7.png"
    caption: "2025 AI Agents" # display caption under cover
    relative: false # when using page bundles set this to true
---

AI 界一日，人间界一年。

回顾 2025 年，从初夏初次使用 Claude Code 带来的巨大震撼，到深冬坐在小米 YU7 里让智驾接管方向盘，我的生活已经被 “Agent”彻底重塑。

## 从 Copy Paste 到 Vibe Coding

记得是在 6 月份，我们公司集体去青岛团建，到达的当天我在推特上刷到了很多人对 Claude Code 的赞赏，我立刻意识到这是一款必须尝试的工具。由于只有 Max 用户才能使用 Opus 模型， 很多人也是说只有 Opus 才能完整的发挥 CC 的全部潜能，我就毫不犹豫地购买了 $100 的 Max 订阅，想看看最强的编程模型究竟能带来怎样的体验。

当晚聚餐后我早早回到酒店，开始使用 Claude Code。第一次真实体验到 AI Agent 的工作方式，震撼之情难以言表。虽然 Manus 早已推出 Agent 功能，并有不少 KOL 在推广，但我一直认为那是营销噱头，缺乏实际操作体验。这是我第一次真实地触摸到了什么是 AI Agent。

Claude Code 将 AI 编程推向了一个新的高度。我认为从 ChatGPT 横空出世到 Claude Code 的 出现，可以划分为三个阶段：
1. **ChatGPT 阶段**：需要手动复制粘贴上下文，本质上仍是手动编程，所有代码都需自己处理。
2. **Copilot 阶段**：借助类似 VSCode Copilot 插件或 Cursor 这类编辑器，AI 可以自动感知上下文，自动补全代码，预测下一个编辑内容。Cursor 的体验无出其右。但 Copilot 阶段仍只能算是半自动化编程，省略的只是复制粘贴的部分。
3. **Claude Code 阶段**：AI Agent 全自动管理上下文，修复错误，提交 PR。至此，有了 Vibe Coding，编程进入了一个全新的时代。人们开始从 Prompt Engineer 进化为 Context Engineer。现在，我们不再需要精心设计一大段 Prompt，而是提供一系列相关的 Context，让 AI 自动提取有效信息来完成任务。我们需要做的只是提供更准确的 Context 和更好的 Context 管理方式。

抛开 Anthropic 的企业文化不谈，我认为 2025 年，Anthropic 在 AI 编程领域的贡献是最大的。从 MCP、Agent Skill、Claude Code、Claude Agent SDK 等各种概念的推出，到 Sonnet 4.0 和 Opus 4.5 模型的发布，每一个都极大推动了整个行业的发展。相比之下，OpenAI 只能在后面紧追不舍，直到 GPT 5.2 才只能在模型层面将将追上，其他如 Codex CLI 和生态系统仍有很大差距。Google 在编程领域更是远远落后，直到 Antigravity 引入 Opus，大家才开始真正使用。

特别想提一下 Claude Agent SDK。有了这个 SDK，很多 Agent 不再需要从零开始编写，只需套用这个 SDK 即可。它帮助我们管理 Agent 的整个生命周期，我们只需提供合适的 Context，如 System Prompt、MCP Tools、Skills 等，而这些都是独立于 Agent 本身的。更重要的是，这不仅限于编程领域，任何行业都可以应用，只需提供相关行业的 Skills 和 Tools。此外，Chat Bot 也正从传统的 Chat 模式向 Agent 模式转变，例如之前的 ChatBots 如 [ChatWise](https://chatwise.app/) 做的更多的是支持不同的 LLM Providers，支持 MCP；而 12 月份 [yetone](https://x.com/yetone) 发布的 [Alma](https://alma.now/) 则明显就不只是 Chat，更多的是 Agent 了。

## Cherry Studio 与开源

2025 年也是我参与开源项目最多的一年，[Cherry Studio](https://github.com/CherryHQ/cherry-studio) 作为我主要参与的项目，给了我很多锻炼的机会。通过为 Cherry Studio 添加 MCP 支持、记忆管理功、API 服务以及 Agent 的实现，我不仅更深入地理解了 AI Agent，也结识到了很多志同道合的朋友。

### MCP
2025 年初，MCP（模型上下文协议）概念刚刚兴起时，我便开始为 Cherry Studio 集成 MCP 功能。

3 月份的时候，大家还不了解 MCP，更不知道如何实现。最初我尝试了一版完全基于 function call（现称 tool use）的实现，但由于当时许多模型尚不支持 function call，或支持不一致，社区中出现了大量 issues。随后，我又做了一版基于 prompt 的实现，通过一些 hack 使所有模型都能支持 MCP。为了帮助用户解决环境配置问题，我还在 Cherry Studio 中集成了快速安装 bun 和 uv 的功能。

虽然现在 MCP 的热度下降了，但是给 Cherry Studio 添加 MCP 支持的经历让我学到了很多。

[Google Trends MCP](https://trends.google.com/trends/explore?date=2025-01-01%202025-12-31&q=%2Fg%2F11x5hnm0vb&hl=en)


### Memory

五月份的时候，我参考了 mem0 的实现，为 Cherry Studio 添加了一版最简单的记忆管理功能。用户可以根据提问查找相关的记忆，并保存用户信息。

做记忆管理的初衷，是我感觉 ChatGPT 的记忆功能做得太好了，而我也意识到未来 AI 公司的竞争不再是模型之间的竞争，而是对用户理解的竞争。更理解用户的模型才能拥有更高的用户粘性。而这也是 OpenAI 等公司的护城河，但如果用户数据都绑定在一个平台，其实是很危险的，特别是国内用户，封号是常有的事。
所以我希望通过 Cherry Studio 让用户可以把记忆数据留在自己手里，避免被平台绑架。可惜后面我由于工作上的事情，没有精力继续完善这个功能。

最近发现 [Wey Gu](https://x.com/wey_gu) 的 [Nowledge Mem](https://mem.nowledge.co/) 做得很好，把我想做的都实现了，我们也在 Cherry Studio 中内置集成了 Nowledge Mem MCP，可以让用户直接使用 Nowledge Mem 作为记忆存储。

### API

社区中有很多用户希望能通过 API 使用 Cherry Studio 的功能，但是因为精力有限一直没有人有时间去做，后来我因为要实现 Agent 的功能，需要一个 API server 来暴露 Cherry Studio 的功能，所以就顺便实现了这个功能。

当时的实现主要有两个 API，一个上提供 `/chat/completions` API，它可以把 OpenAI Chat Completion 的模型供其它 App 使用；另一个是 MCP 服务，可以把 Cherry Studio 作为 MCP 管理的客户端，通过 StreamableHTTP 协议提供 MCP 功能。

有了这两个 APIs，Cherry Studio 本身就可以作为一个 Proxy，在 Cherry Studio 中添加各种 LLM Providers，然后通过 API 给其它 App 使用。不用在每个 APP 中再配置一遍。而提供 Streamable HTTP 协议的 MCP 服务，更是让用户可以直接在 Cherry Studio 中只启动一个 stdio 的 MCP，但是其它 App 可以通过 Streamable HTTP 协议来连接这个 MCP，而不用再启动一个 stdio 的进程了。

最近 yetone 等 Alma 提供的 API Proxy 功能，得到了大家的一致好评。其实 Cherry Studio 十月份就提供了类似的功能，但是一我不太会宣传，二是 yetone 实现的是 Anthropic 的 API，而我们之前的实现只是 OpenAI 的 Chat Completion API。基于 Anthropic 的好处是，现在 Claude Code 这类的 Coding Agent 更火了，大家有更多这类 Proxy 的需求。

### Agent

六月份的时候接触到了 Claude Code, 被它强大的 Agent 功能震撼到了，并且意识到虽然叫 Claude Code 但是它其实是通用的 Agent，各行各业都适用，所以我七月就开始基于 Claude Agent SDK （当时还叫 Claude Code SDK） 做了一版简单的 Agent 集成，但是也是因为工作上的事情，还有 Cherry 本身的情况，就搁置了一直没有 merge 进去。

后来十月份的时候 Agent 的概念更火了，大家都开始做 Agent 了，Cherry Studio 里面大家也一致把 Agent 作为最高优先级的功能来做了，所以我又重头开始实现了一版 Agent 集成，因为这次大家都知道这个很重要，大家一起帮忙，做起来就快很多，然后 1.7 版本就上了。

### 总结

参与开发 Cherry Studio 是很棒的经历，也认识了很多很棒的朋友，很多的贡献者都是还没毕业的学生，不得不感慨时间真的过的太快，也不得不感慨现在 AI 对人能力边界的提升。

我参与 Cherry Studio 开发有个很重要的原因是，我希望可以 AI 平权，普通人也可以使用最先进的 AI 模型，AI 技术，而不用被平台绑架，可以自己控制数据。国外有御三家，国内有豆包等平台，这些平台都在捆绑用户数据，用户数据都在平台手里，用户是没有选择权的。我希望通过 Cherry Studio 这种开源项目，让用户可以自己选择使用什么模型，自己控制数据。

但同时我也看到 Cherry Studio 本身变得越来越复杂，普通人使用门槛越来越高，离我的期望越来越远了。我更希望是有一个类似 ChatGPT 一样简单又复杂的工具，ChatGPT 的 UI 无比的简单，但是它实现的功能却非常复杂强大。我希望 Cherry Studio 未来也能走这条路，既有强大的功能，又有简单易用的界面，让更多人可以使用。

## YU7 与智能驾驶

今年对我来说还有一件大事就是买了小米 YU7 Max，虽然这款车争议非常大，我却很喜欢它。


简单列一下优缺点

### 优点
- 外观符合我的审美
- 配置足够丰富，电池，芯片，电机都很不错
- 续航表现很好，实际使用中满电可以跑 500 多公里
- 800V 快充很够用（5C 更快），现在去高速服务区上个卫生间来回的时候就可以走了，基本不用多等
- 车机是我体验过最好的，加上天际屏，整体体验非常棒
- 后排乘坐体验很好，靠背可以调节很大角度，长途乘坐也不累
- 车内很安静，120 码以下几乎听不到噪音

### 缺点
- 前排座椅太短，对腿部支撑不够，长途驾驶还是会累
- 音响效果一般，虽然是 25 个扬声器的高配，但整体效果还是很一般（最近升级了系统好一点了）
- 标配轮胎和轮毂表现一般，我选配了 20 寸轮毂，胎偏薄
- 车内后视镜不是流媒体的，使用起来不太方便

### 辅助驾驶

需要重点聊一聊辅助驾驶，不是说小米的辅助驾驶很牛，而是我感受到了类似使用 Claude Code 那样的震撼。

现阶段所有车辆的智能驾驶在城区都很一般，无论它怎么宣传，基本上都是鸡肋。但是高速上的辅助驾驶已经非常实用了。这种情况就和编程领域的 AI Agent 一样，一部分场景下非常好用，另一部分场景下完全不行。但是趋势不可挡，未来几年这一块一定会越来越好。离全自动驾驶不会太远。

小米的辅助驾驶分为三种模式：
- 全速自适应巡航（只控制速度不控制方向）
- 居中辅助（控制速度，并且可以帮忙保持在路中间）
- 领航辅助（NOA）在居中辅助的基础上支跟随地图自动选择道路，自主超车，也就是平常说的智驾，NOA 又分高速和城区。

我主要使用的是 NOA 高速模式，使用高速 NOA 的时候我也不会全区交给它，只是用做辅助，自己随时准备接管，毕竟生命是自己的，而且现在辅助驾驶没有问责机制，出了事故还是要自己负责的。
但是我为什么又愿意使用它呢？因为它确实能大幅度缓解驾驶疲劳，提高驾驶体验。它在堵车，隧道，桥梁等路况简单，规则明确的场景下表现非常好，而这些场景也是开车中最无聊的场景。所以使用 NOA 能提高驾驶体验，我可以只在想开的地方自己开，在无聊的地方让车帮我开。
仔细想想和使用 Claude Code 的感觉很像，Claude Code 在我写代码的时候帮我处理了很多无聊的重复性的工作，让我可以专注在更有创造性的工作上，而 NOA 也帮我处理了很多无聊的驾驶场景，让我可以专注在更有趣的驾驶场景上。

> Tips: 使用 NOA 的时候，一定要开启变道确认功能，这样车在变道前会先提示你确认，避免一些不必要的危险。

### 总结

换了 YU7 之后，我感觉车不再仅仅是一个代步工具，它更像是一位随行的助手。开车时，仿佛车里住着一个小精灵——它熟悉你的偏好，时刻帮你关注路况，疲劳时替你掌控方向盘，还能为你导航、播放音乐。语音交互在车内场景下显得格外自然，加上这两年 LLM 的快速发展，车载大模型的表现也已相当出色。

人类的一大优势在于善用工具。当你真正理解辅助驾驶的能力边界后，充分利用它便能带来显著的体验提升。这与 AI 编程的逻辑如出一辙——从 AI 辅助编程中获益最多的往往是资深程序员，因为他们深谙 AI 的边界所在，能够更精准地借助 AI 提升自己的效率。

## 回顾与展望

AI 界一日，人间界一年。AI 发展的太快，未来社会将走向何方，谁也无法预言。

有人担忧巨头垄断，但开源模型与闭源头部模型的差距正在缩小，即便差距依然存在，也并非不可逾越——如今模型的智能水平已足够强大，无需依赖超大公司，个人与小团队同样能成就诸多可能。

也有人憧憬个人创业的黄金时代，认为每个人都将尽情释放创造力，如同今天人人皆可写代码一样。然而现实果真如此吗？热爱创造的人终究是少数，能真正创造出价值的更是凤毛麟角。更令人忧虑的是，当 AI 能够替代大部分工作，当人们无法通过劳动为他人创造价值时，许多人或许也将失去生活的意义与方向。

这是最好的时代，也是最坏的时代。我们正站在历史的浪尖，见证着时代的波澜壮阔，而更大的风浪尚在前方。面对这一切，我们唯一能做的，便是不断学习、持续适应，在巨浪中找到属于自己的航向。
