---
date : '2025-03-09T23:55:23+08:00'
draft : false
slug : 'mcp-guide'
title : 'MCP 协议完全指南'
tags : ['AI', 'MCP', 'LLM', '开发']
categories : ['技术']
description : '深入解析模型上下文协议 (MCP) 的架构、工作原理及应用场景，帮助开发者了解如何利用 MCP 增强 AI 应用的能力与灵活性。'
---

## 引言
模型上下文协议 (Model Context Protocol, MCP) 是近期 AI 领域的最热门的话题，它为大型语言模型提供了与外部世界交互的标准化方式。本文将深入介绍 MCP 的架构、工作原理、开发方法及其应用场景，帮助读者全面了解这一协议如何增强 AI 应用的能力和灵活性。

## 什么是 MCP

**模型上下文协议（MCP）** 是一种开放协议，旨在为大型语言模型（LLM）提供标准化的上下文访问方式。官方举例说，MCP 犹如 AI 应用的 "USB-C 端口"，使 LLM 能够通过统一的接口无缝连接至多种数据源和工具（如文件系统、数据库或外部 API）。

基于 JSON-RPC（一种使用 JSON 编码的远程过程调用协议）构建的 MCP 提供了一种面向客户端和服务器之间上下文交换和采样协调的有状态会话协议。

### MCP 的架构
![](https://s3.vaayne.com/vaayne/images/2025/03/1741591536-Pasted%20image%2020250309222148.png)

MCP 采用客户端-服务器模型，其核心组件包括：

- **MCP Hosts**：运行 MCP 的主应用程序，例如 Claude Desktop、智能 IDE 或 AI 工具，负责通过 MCP 访问外部数据。
- **MCP Clients**：协议客户端，与服务器建立一对一连接，负责发送请求并接收响应。
- **MCP Servers**：轻量级程序，通过 MCP 暴露特定功能（如文件读取或 API 调用）。

这是 MCP 的时序图

![](https://s3.vaayne.com/vaayne/images/2025/03/1741591538-Pasted%20image%2020250309222608.png)

1. Host 开始初始化 Client
2. Client 尝试与 Server 建了连接，并询问 Server 有哪些能力
3. 服务端就回应说并返回所有支持的功能（例如天气查询，网络搜索），这样客户端和服务端就建立了 1:1 的连接
4. User 和 LLM 说我希望查询"未来一周北京的天气"，LLM 就会告诉 Client 说我需要调用 "网络搜索" 和 "天气查询" 的 Tools。
5. Client 收到 Host 的请求，就会把请求的信息发给 Server
6. Server 收到信息就开始处理，处理完成就返回给 Client
7. Client 收到返回的信息，再将信息返回给 Host 进行 LLM 对话
8. LLM 给予 Client 给的信息再生成最终的回复返回给 User

若想深入了解 MCP 的更多细节，欢迎访问 [官方网站](https://modelcontextprotocol.io/introduction)，[协议详情](https://spec.modelcontextprotocol.io/specification/2024-11-05/architecture/)

## 为什么需要 MCP

随着 LLM 应用的普及，我们期望 LLM 能够承担更多功能，不仅限于简单对话，还能与现实世界进行有效交互。这种交互依赖外部数据和工具，而将所有数据和工具直接集成到 LLM 中显然不现实。借助 MCP，只要支持了该协议，就能轻松将各种数据源和工具连接到 LLM。

以一个常见场景为例，我们在与 LLM 交互时，经常需要联网搜索最新信息以减少幻觉。然而，并非所有聊天机器人都支持联网功能，即使支持联网的也可能不包含你习惯使用的搜索引擎。在没有 MCP 的情况下，用户只能等待开发者添加特定搜索引擎的支持；而有了 MCP 后，只需简单配置，就能将所需服务接入当前使用的聊天机器人。

这种机制类似于我们日常使用浏览器的方式：我们可以通过 Chrome 使用 Google 搜索，也可以用 Firefox 访问 Google 或其他搜索引擎。这得益于浏览器与搜索引擎之间存在标准化的 HTTP 协议，而 MCP 协议在 LLM 生态系统中扮演着相同的角色。

![llm-app-without-mcp](https://s3.vaayne.com/vaayne/images/2025/03/1741591539-llm-app-without-mcp.png)

![llm-app-with-mcp](https://s3.vaayne.com/vaayne/images/2025/03/1741591540-llm-app-with-mcp.png)

### MCP 和 Tool Call 的区别

Tool Call 是一种机制，它告知 LLM 有哪些工具可供使用，随后 LLM 根据用户的 Prompt 来决定需要调用哪些特定工具。然而，LLM 本身并不具备直接调用这些工具的能力，这就需要依靠 MCP host（如 Cursor 或 Cherry Studio 等应用）通过 MCP client 和 MCP server 的交互来实现实际调用。

诚然，即使没有 MCP 也能实现工具调用功能，但这样一来，每种工具的调用方式都需要单独实现，这正是缺乏标准化带来的问题。

以一个具体场景为例：假设你希望 LLM 告诉你未来一周北京的天气情况，你有两个可用的工具——一个是"获取特定地点和日期的天气信息"(get weather with locations and date)，另一个是"获取当前时间"(get current time)。在没有 MCP 的情况下，你需要在代码中预先编写如何调用这两个功能，然后在 LLM 需要时分别触发调用。而有了 MCP 之后，你只需配置好相应的 servers，便可通过统一的方式进行调用，区别仅在于传递的参数不同。

## MCP 协议讲解

MCP 协议主要分为这几大部分
- 消息格式
- 生命周期
- 传输协议
- 版本
- 额外的工具类

### 传输协议

这里面其它的都好理解，不理解也没有关系，SDK 都会很好的包装起来，唯独传输协议需要特别关注一下

现在 MCP 支持下面传输协议
1. stdio （标准输入/输出）：通过标准 I/O 进行通信
2. HTTP with Server-Sent Events (SSE)：通过 HTTP 与外部服务进行通信，SSE 是一种允许服务器向客户端推送实时更新的技术
3. 自己实现 [传输协议](https://spec.modelcontextprotocol.io/specification/2024-11-05/basic/transports/#custom-transports)

#### stdio

我们看到官方的例子里面都是提供的这种例子，通过 stdio 的好处是 server 运行在本机，数据更安全，坏处是占用本机的资源，每到一个新的环境需要重新配置一遍。
例如：
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/allowed/files"]
    },
    "git": {
      "command": "uvx",
      "args": ["mcp-server-git", "--repository", "path/to/git/repo"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "<YOUR_TOKEN>"
      }
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "postgresql://localhost/mydb"]
    }
  }
}
```

它的具体流程是这样的：
1. 本机的 Client 启动一个子进程来运行 Server
2. Client 给 Server 发送 JSON-RPC 格式的消息，而具体发送的方式就是将消息写入 stdin，消息发送结束之后再发送一个换行符，这样 Server 就知道消息接收完毕，可以开始处理了
3. Server 处理完之后，也生成 JSON-RPC 格式的消息，并写到 stdout，并且也可以记录 log 到 stderr，log 的格式就是 UTF-8 字符串
4. 不断循环 2 ～ 3
5. 如果不需要的 Server 了，Client 就可以终止运行这个 Server 的子进程

![](https://s3.vaayne.com/vaayne/images/2025/03/1741591541-Pasted%20image%2020250309224410.png)
#### sse

SSE 格式中，Server 可以作为一个独立的进程运行，可以运行在本机也可以运行在远程服务器上。这种就类似传统的 API 服务，API server 可以同时支持多个 Clients 的接入，并且对本地运行的环境没有要求，所以理论上我在手机上也是可以使用的。劣势就是如果 Server 在远程，数据都会经过远程服务器。

SSE 的情况下，Server 必须提供两个 API：
1. SSE 端点，作为 Client 和 Server 建立连接和接收消息的端点
2. HTTP POST 端点，用于 Client 给 Server 发送消息的端点

具体流程如下：
1. 客户端通过 SSE 端点向 Server 请求建立连接
2. Server 收到 Client 的请求，并返回 POST 端点给 Client （后续 Client 需要通过这个 POST 端点给 Server 发送消息），双方建立连接
3. Client 通过 POST 端点给 Server 发送消息，Server 处理消息，并通过 SSE 端点返回给 Client
4. 重复 2 ～ 3，直到 Client 结束连接

![](https://s3.vaayne.com/vaayne/images/2025/03/1741591542-Pasted%20image%2020250309224421.png)

## 如何开发一个 MCP Server

[MCP servers](https://github.com/modelcontextprotocol/servers) MCP 官方的 servers 合集

MCP Server 提供三项能力
- Prompts：预定义模板或指令，用于指导语言模型交互
- Resources：为模型提供额外上下文的结构化数据或内容
- Tools：允许模型执行操作或检索信息的可执行函数

### Prompts
Prompts 的场景是我可以在 Server 端管理一些预定义的 prompts, 这样做的好处是，我可以随时在 Server 端更新 Prompt 而无需更改应用端的代码

**list prompts request**
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "prompts/list",
  "params": {
    "cursor": "optional-cursor-value"
  }
}
```
**list prompts response**
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "prompts": [
      {
        "name": "code_review",
        "description": "Asks the LLM to analyze code quality and suggest improvements",
        "arguments": [
          {
            "name": "code",
            "description": "The code to review",
            "required": true
          }
        ]
      }
    ],
    "nextCursor": "next-page-cursor"
  }
}
```

#### Resources

Resources 是通过一种标准化的方式向 Client 暴露 Server 所拥有的资源，每个资源都有一个唯一的 URL 与之对应。类似于 REST API，每个端点都应该代表一个资源，可以 list 所有的资源，也可以 get 获取单个资源。

Resources 比较难以理解的是它的使用场景，我之前见过一个例子，有人开发了一个 Search Web 的 MCP Server，每次搜索之后 Server 会把搜索结果缓存起来，然后这个缓存就是以 Resource 的形式告诉给 Client ，Client 每次向 Server 调用 Tool 进行真正的搜索之前，都会先查询 Resource 里面是不是存在类似的资源，不存在才进行查询。在我看来这个就是纯粹未来实现 Resource 而实现 Resource。

#### Tools

Tools 则是 MCP Server 最常用也最实用的功能，这个 Tool 和 OpenAI 函数调用里面的 Tool 是一样的，都是允许 LLM 和外部世界进行交互的工具。

API 主要有两个
1. ``tools/list`` 列出 Server 支持的所有工具
2. ``tools/call``  Client 请求 Server 去执行某个工具，并将结果返回

流程图如下，和 Function Call 是一样的。

![](https://s3.vaayne.com/vaayne/images/2025/03/1741591543-Pasted%20image%2020250309233101.png)

### 完整示例代码

以下是一个简单的 MCP Server 实现示例，来自 [官方](https://github.com/modelcontextprotocol/python-sdk)：

```python
# server.py
from mcp.server.fastmcp import FastMCP

# Create an MCP server
mcp = FastMCP("Demo")

# Add an addition tool
@mcp.tool()
def add(a: int, b: int) -> int:
    """Add two numbers"""
    return a + b

# Add a dynamic greeting resource
@mcp.resource("greeting://{name}")
def get_greeting(name: str) -> str:
    """Get a personalized greeting"""
    return f"Hello, {name}!"
```

启动 Server
```bash
mcp dev server.py
```

## 如何使用 MCP

要想使用 MCP，除了需要 Server，还需要 Client，[MCP 官网](https://modelcontextprotocol.io/clients) 列出来一些支持 MCP 的 Clients
![](https://s3.vaayne.com/vaayne/images/2025/03/1741591544-Pasted%20image%2020250309233448.png)

从这个图中我们可以看到不同 Client 对 MCP 能力的支持是不一样，Claude App 支持 Resources, Prompts 和 Tools；但是 Cursor 只支持 Tools。使用 MCP 的时候我们就需要选择合适的 Client。

大部分应用都有对应 MCP 使用的教程，但是因为 Cherry Studio 是我最近才开发支持的，还没有对应的教程，我就以 Cherry Studio 的使用为例。

### Cherry Studio 使用 MCP

[Cherry Studio](https://github.com/CherryHQ/cherry-studio) 是一款开源免费的桌面端聊天应用，支持 Windows，Linux 和 Mac。

#### 配置 MCP Server

**可以添加无限多个 Servers，但是过多的 Server 会浪费大量的 Token 需要酌情使用**

1. 进入 `设置-> MCP 服务器` 添加 Server
2. 选择 SSE 还是 STDIO 类型
3. 如果是 SSE 直接填入 URL，STDIO 需要填写命令，参数和环境变量
4. 选择是否启用

![](https://s3.vaayne.com/vaayne/images/2025/03/1741591545-Pasted%20image%2020250309234502.png)
![](https://s3.vaayne.com/vaayne/images/2025/03/1741591546-Pasted%20image%2020250309234321.png)

![](https://s3.vaayne.com/vaayne/images/2025/03/1741591547-Pasted%20image%2020250309234257.png)

#### 使用 MCP Server

进入聊天界面，输入框中有一个 MCP 配置项，默认会使用所有启用的 Servers，但是可以单独配置需要哪个，以减少 Token 的使用
![](https://s3.vaayne.com/vaayne/images/2025/03/1741591547-Pasted%20image%2020250309234803.png)

聊天界面中会显示使用了哪个 MCP 的 Tool，调用参数和结果，可以展开或者收起。
![](https://s3.vaayne.com/vaayne/images/2025/03/1741591548-Pasted%20image%2020250309234935.png)
![](https://s3.vaayne.com/vaayne/images/2025/03/1741591549-Pasted%20image%2020250309235027.png)

![](https://s3.vaayne.com/vaayne/images/2025/03/1741591549-Pasted%20image%2020250309235211.png)

使用的方式还是很简单的，如果有需求或者遇到问题可以去 [Github](https://github.com/CherryHQ/cherry-studio/issues) 上提交 issue。

## 总结

MCP 作为一种标准化协议，极大地简化了大语言模型与外部世界的交互方式，使开发者能够以统一的方式为 AI 应用添加各种能力。

MCP 的核心价值在于其标准化和可扩展性，它不仅降低了开发复杂度，还提高了 AI 应用的互操作性。随着越来越多的应用支持 MCP，我们可以预见未来 AI 生态系统将变得更加开放和强大。

## 资源与参考
- [MCP 官方网站](https://modelcontextprotocol.io/)
- [MCP 规范文档](https://spec.modelcontextprotocol.io/)
- [MCP GitHub 仓库](https://github.com/modelcontextprotocol)
- [MCP Server 合集](https://github.com/modelcontextprotocol/servers)
- [支持 MCP 的应用列表](https://modelcontextprotocol.io/clients)
- [Cherry Studio](https://github.com/CherryHQ/cherry-studio)
