---
date : '2025-03-11T23:02:22+08:00'
draft : false
slug : 'how-to-use-mcp-in-cherry-studio'
title : '如何在 Cherry Studio 中使用 MCP'
category : ['技术']
tags : ['MCP', 'Cherry Studio', 'UV', 'LLM']
description : 'Learn how to configure and use the Model Context Protocol (MCP) in Cherry Studio.'
---

# 在 Cherry Studio 中配置和使用模型上下文协议 (MCP)

模型上下文协议（Model Context Protocol，简称 MCP）允许大型语言模型（LLM）通过调用外部工具和服务获取实时信息，从而扩展其能力。本文将详细介绍如何在 Cherry Studio 中配置和使用 MCP。

> 💡 **提示**：建议提前阅读 [MCP 协议完全指南](https://vaayne.com/posts/2024/mcp-%E5%8D%8F%E8%AE%AE%E5%AE%8C%E5%85%A8%E6%8C%87%E5%8D%97/) 了解更多 MCP 相关的基础知识。

## 准备工作

### 1. 安装最新版本的 Cherry Studio

从官方网站 [cherry-ai.com](https://cherry-ai.com/) 下载并安装最新版的 Cherry Studio。

### 2. 了解 MCP 传输协议类型

MCP 支持两种传输协议：

- **STDIO**（标准输入/输出）：在本地运行，可访问本机文件和应用程序，但需要配置 Python 和 NodeJS 环境
- **SSE**（服务器发送事件）：在远程服务器运行，配置简单，但无法访问本地资源

## 配置环境（STDIO 类型需要）

如果你只使用 SSE 类型的 MCP 服务，可以跳过此部分。

### Windows 环境配置

#### 安装 Python （使用 uv)

1. 打开 PowerShell

   ![打开 PowerShell](https://s3.vaayne.com/vaayne/images/2025/03/1741700905-20250311214825235.png)

2. 运行安装命令
   ```
   powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
   ```

3. **关闭并重新打开 PowerShell**，输入 `uv` 验证安装是否成功

   ![UV 安装验证](https://s3.vaayne.com/vaayne/images/2025/03/1741701567-SCR-20250311-sxkt.png)

#### 使用 bun 安装 NodeJS （推荐）

1. 前往 [官方网站](https://bun.sh/docs/installation) 复制安装命令，打开 PowerShell 粘贴运行

   ```bash
   powershell -c "irm bun.sh/install.ps1|iex"
   ```
![Bun 安装](https://s3.vaayne.com/vaayne/images/2025/03/1741840956-20250313124236166.png)
2. 安装完成后，**关闭并重新打开 PowerShell**，输入 `bun --version` 验证安装

#### 安装 NodeJS

1. 前往 [官方网站](https://nodejs.org/en/download) 下载安装包并运行

   ![NodeJS 下载页面](https://s3.vaayne.com/vaayne/images/2025/03/1741701522-SCR-20250311-sygp.png)

2. 安装完成后，打开 PowerShell，输入 `node --version` 验证安装

   ![NodeJS 安装验证](https://s3.vaayne.com/vaayne/images/2025/03/1741701544-SCR-20250311-szyy.png)

### MacOS / Linux 环境配置

1. 安装 [Homebrew](https://brew.sh/)
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

2. 使用 Homebrew 安装 `uv`
   ```bash
   brew install uv
   ```

3. 使用 Homebrew 安装 Bun
   ```bash
   brew install oven-sh/bun/bun
   ```

4. 使用 Homebrew 安装 NodeJS
   ```bash
   brew install node
   ```

5. 在终端分别运行 `uv` 和 `bun` 或者 `node` 验证安装成功

   ![Mac 环境验证](https://s3.vaayne.com/vaayne/images/2025/03/1741702304-20250311221144160.png)

## 配置 MCP 服务器

在 Cherry Studio 中，通过以下步骤添加 MCP 服务器：

1. 进入 `设置 -> MCP 服务器`
2. 点击 `添加服务器` 按钮

![添加 MCP 服务器入口](https://s3.vaayne.com/vaayne/images/2025/03/1741591547-Pasted%20image%2020250309234257.png)

### STDIO 类型配置

STDIO 类型的 MCP 服务在本地运行，可以访问本地文件和系统资源。下面以官方 [Fetch MCP Server](https://github.com/modelcontextprotocol/servers/tree/main/src/fetch) 为例：

1. 点击 `添加服务器`，选择 `STDIO` 类型
2. 填写配置信息：
   - 名称：`fetch`（服务器的标识名）
   - 命令：`uvx`（用于运行服务的命令）
   - 参数：`mcp-server-fetch`（命令的参数）

![STDIO 类型配置示例](https://s3.vaayne.com/vaayne/images/2025/03/1741591546-Pasted%20image%2020250309234321.png)

**高级配置**：某些 MCP 服务需要环境变量，例如 [Brave Search MCP Server](https://github.com/modelcontextprotocol/servers/tree/main/src/brave-search) 需要 API 密钥：
```json
{
  "mcpServers": {
    "brave-search": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-brave-search"
      ],
      "env": {
        "BRAVE_API_KEY": "YOUR_API_KEY_HERE"
      }
    }
  }
}
```

**对于 npx 有问题的可以使用 bunx 代替**，上面的等效于
```json
{
  "mcpServers": {
    "brave-search": {
      "command": "bunx",
      "args": [
        "@modelcontextprotocol/server-brave-search"
      ],
      "env": {
        "BRAVE_API_KEY": "YOUR_API_KEY_HERE"
      }
    }
  }
}
```

### SSE 类型配置

SSE 类型的 MCP 服务配置简单，只需提供服务器的 URL。此类型服务在远程服务器上运行，无法访问本地资源。

![SSE MCP Server](https://s3.vaayne.com/vaayne/images/2025/03/1741591545-Pasted%20image%2020250309234502.png)

## 使用 MCP

确保 MCP 服务已配置并正常运行。在设置页面启用所需的 MCP 服务：

![启用 MCP 服务](https://s3.vaayne.com/vaayne/images/2025/03/1741591547-Pasted%20image%2020250309234257.png)

进入聊天界面，点击 MCP 服务器按钮选择要启用的 MCP 服务器：

![选择 MCP 服务器](https://s3.vaayne.com/vaayne/images/2025/03/1741703836-20250311223716241.png)

启用 MCP 服务后，每次操作都会与大模型交互，消耗大量 Token。建议只启用需要的 MCP 服务。

右上角的开关可启用全部 MCP 服务，也可选择单个 MCP 服务。

![启用 MCP 服务开关](https://s3.vaayne.com/vaayne/images/2025/03/1741703852-20250311223732497.png)

在对话中启用 `fetch` 和 `tavily-mcp` 服务，LLM 会多次调用 MCP 服务获取信息。

![调用 MCP 服务](https://s3.vaayne.com/vaayne/images/2025/03/1741704227-20250311224347252.png)

点击 MCP 状态栏查看调用参数和返回结果，方便分析结果的可靠性。

![查看 MCP 调用详情](https://s3.vaayne.com/vaayne/images/2025/03/1741704432-20250311224712236.png)

## FAQ

#### 如何解决网络问题？

国内网络环境可能导致安装 uv 和 NodeJS 时无法访问外网资源。以下是解决方案：

##### Python 包使用国内镜像源

使用 `-i` 参数指定国内镜像源：

```bash
uvx -i https://pypi.tuna.tsinghua.edu.cn/simple mcp-server-fetch
```

常用国内 PyPI 镜像：
- 清华大学：`https://pypi.tuna.tsinghua.edu.cn/simple`
- 阿里云：`http://mirrors.aliyun.com/pypi/simple/`
- 中国科技大学：`https://mirrors.ustc.edu.cn/pypi/simple/`
- 华为云：`https://repo.huaweicloud.com/repository/pypi/simple/`
- 腾讯云：`https://mirrors.cloud.tencent.com/pypi/simple/`

配置示例：
![国内镜像源配置](https://s3.vaayne.com/vaayne/images/2025/03/1741756014-20250312130654197.png)

##### NodeJS 包使用国内镜像源

使用 `--registry` 参数指定国内 npm 镜像：

```bash
npx --registry=https://registry.npmmirror.com -y @modelcontextprotocol/server-filesystem
```

常用 npm 镜像：
- 淘宝镜像：`https://registry.npmmirror.com`

#### 带有路径的 MCP 服务怎么安装

某些 MCP 服务未打包到 npm 或 pypi，需要本地运行源代码。先下载代码，安装依赖，再运行。例如：[web-search](https://github.com/pskill9/web-search)
```json
{
  "mcpServers": {
    "web-search": {
      "command": "node",
      "args": ["/path/to/web-search/build/index.js"]
    }
  }
}
```

### 如何使用 smithery 安装

目前不支持使用 [smithery](https://smithery.ai) 进行安装。smithery 自动写入配置文件，但我们暂不支持此方式。

### 找不到 npx 或者 uvx 命令
如果设置 MCP 服务器时提示找不到 `npx` 或 `uvx` 命令，请使用绝对路径，例如：
`bunx` 换成 `/opt/homebrew/bin/bunx` 或者 `uvx` 换成 `/opt/homebrew/bin/uvx`，具体路径可以使用 `which` 命令查看：
```bash
> which bunx
/opt/homebrew/bin/bunx
```
