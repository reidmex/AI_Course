---
title: "Claude Code零基础小白教程第3章：核心配置：让你的 Claude Code接上脑子"
url: https://mp.weixin.qq.com/s?__biz=MjM5NzQ0NzIyOQ==&mid=2448131812&idx=1&sn=8ab14a3a8ba3a465ae86f5feb770c26a&chksm=b2c6f5b685b17ca0a9757675d7bf7db119262af53a68eee8b4143b03ce9f478aa8764d654a2f&cur_album_id=4566215426073788416&scene=189#wechat_redirect
byline: "萝卜啊"
saved: 2026-06-26T09:46:41.882Z
---
# Claude Code零基础小白教程第3章：核心配置：让你的 Claude Code接上脑子

> **第一阶段包含第一章至第四章**：主要任务是快速上手——从安装到跑通第一个任务
> 
> **本阶段目标**：完成环境搭建、Claude Code 安装、国产模型配置，并成功执行你的第一个任务。预计用时 1-2 小时。

**本章是整份教程最重要的章节。安装 Claude Code 只是装了个壳，配置模型才是让它真正能用的关键。**

### 3.1 理解关键：Claude Code 的模型是怎么连接的

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/MiaEMf64YmohkPeDVHXJIfIzLKhVicUPI262sPX7skYkFKgC2EjjUanR9KucHcFgErictjjnkVCQU8t0yPX3P2HibGkWBLXmo8Jn9th5R21DaHE/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=0)

安装好 Claude Code 之后，如果你直接输入 `claude` 启动，大概率会遇到连接失败或认证错误。这是因为 Claude Code 默认连接的是 Anthropic 的官方 API 服务器，而这个服务器在国内基本无法直接访问。

#### 核心机制：两个环境变量

Claude Code 通过两个关键环境变量来决定“把请求发到哪里”和“用什么身份验证”：

`       1  2          ANTHROPIC_BASE_URL="https://api.anthropic.com"   # API 服务地址   ANTHROPIC_API_KEY="sk-ant-xxxxxxxxxxxx"          # API 密钥            `

-   `ANTHROPIC_BASE_URL`： Claude Code 发请求的目标地址。默认值是 `https://api.anthropic.com`，你的任务是把它改成国产模型或中转服务的地址。
-   `ANTHROPIC_API_KEY`： 你的 API 密钥，用于身份验证。每个模型服务商都会给你一个 Key。

只要把这两个变量设置正确，Claude Code 就能正常工作了——它不关心背后的模型是什么，只关心请求格式是否符合 Anthropic 的 API 规范。

#### 为什么需要“换源”

简单说：Anthropic 官方服务器在国内被网络限制，直连极不稳定，而且需要海外手机号注册和境外信用卡支付。但 Claude Code 的框架是**开放的**——它只是通过 HTTP 协议发请求，只要目标服务器实现了相同的 API 格式，就能替代。

这给了国内用户三条路：

路径

方案

成本

延迟

推荐度

路径A

国产模型直连

低（有免费额度）

低

⭐⭐⭐⭐⭐

路径B

中转 API（用官方模型）

中

中

⭐⭐⭐

路径C

本地模型（Ollama 等）

零

极低

⭐⭐

**对于 90% 的国内用户，路径 A（国产模型直连）是最佳选择**——成本低、速度快、无网络障碍、注册只需手机号。本书将路径 A 作为默认推荐方案进行详细讲解，路径 B 和 C 作为进阶和备选方案。

### 3.2 路径A：国产模型直连（推荐首选）

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

#### 3.2.1 智谱 GLM Coding Plan 配置

智谱 AI 是国内最早推出 Anthropic API 兼容接口的服务商之一，其 GLM 系列模型在代码生成方面表现突出。截至 2026 年 5 月，智谱推出了专门面向编程场景的 **Coding Plan**，新用户注册即赠送 2000 万 tokens 的免费体验额度。

**步骤一：注册并获取 API Key**

1.  打开 open.bigmodel.cn
    
2.  点击右上角“注册”，使用手机号即可完成注册（无需海外信息）
    
3.  登录后进入控制台，点击左侧菜单“API Keys”
    
4.  点击“创建新的 API Key”，复制生成的 Key（格式为 `xxx.yyy`）
    
5.  在“模型广场”找到 GLM 系列模型，确认 Coding Plan 已开通
    

> **⚠️ 重要**：API Key 只显示一次，请立即保存到安全的地方。如果不小心关掉了，只能删除重建。

**步骤二：获取兼容端点地址**

智谱提供了兼容 Anthropic API 格式的端点：

`       1          https://open.bigmodel.cn/api/anthropic            `

这个端点会接收 Anthropic 格式的请求，转发给 GLM 模型处理，然后返回 Anthropic 格式的响应。对 Claude Code 来说，它以为自己连的是 Anthropic 服务器。

**步骤三：配置环境变量**

**macOS / Linux**（在终端中执行）：

`       1  2          export ANTHROPIC_BASE_URL="https://open.bigmodel.cn/api/anthropic"   export ANTHROPIC_API_KEY="你的智谱API Key"            `

**Windows（Git Bash）**：

`       1  2          export ANTHROPIC_BASE_URL="https://open.bigmodel.cn/api/anthropic"   export ANTHROPIC_API_KEY="你的智谱API Key"            `

> **注意**：上面用 `export` 设置的环境变量只在当前终端窗口有效，关了窗口就没了。要让配置永久生效，见下文“永久配置方法”。

**步骤四：启动验证**

在同一个终端窗口中输入：

`       1          claude            `

如果一切正常，你会看到 Claude Code 的欢迎界面，提示你选择一个模型（如果服务商提供了多个模型的话）。输入一个简单的问题测试一下：

`       1          你好，请简单介绍一下你自己。            `

如果收到了正常回复，恭喜你，配置成功！

**永久配置方法**：

把上面的 `export` 命令加到你的 Shell 配置文件中：

-   macOS / Linux（zsh）： 编辑 `~/.zshrc`
-   macOS / Linux（bash）： 编辑 `~/.bashrc`
-   Windows（Git Bash）： 编辑 `~/.bashrc`

`       1  2  3          echo 'export ANTHROPIC_BASE_URL="https://open.bigmodel.cn/api/anthropic"' >> ~/.zshrc   echo 'export ANTHROPIC_API_KEY="你的智谱API Key"' >> ~/.zshrc   source ~/.zshrc            `

> **安全提醒**：如果你的电脑是多人共用，建议把 API Key 保存在更安全的文件中（如 `~/.claude/settings.json`），而不是直接写在 `.zshrc` 等可能被其他人看到的文件里。详见 3.3 节的 settings.json 配置方式。

#### 3.2.2 阿里云百炼（通义千问）配置

阿里云百炼平台提供了通义千问系列模型（Qwen），其中 Qwen Code 模型专为编程场景优化，响应速度极快（200ms 级别），且支持 128K 长上下文。

**步骤一：开通百炼平台**

1.  打开 bailian.console.aliyun.com
    
2.  使用阿里云账号登录（没有的话用手机号注册一个）
    
3.  在控制台中开通百炼服务（有免费额度）
    
4.  进入“模型广场”，找到 Qwen Code 模型并开通
    
5.  在“API-KEY 管理”页面创建 API Key
    

**步骤二：获取端点地址**

阿里云百炼的 Anthropic 兼容端点：

`       1          https://dashscope-intl.aliyuncs.com/compatible-mode/anthropic            `

> **注意**：百炼的 Anthropic 兼容接口在持续迭代中，具体地址可能会更新。请以阿里云官方文档的最新说明为准。如果上述地址不可用，也可以直接使用 Anthropic 原生格式的 Key + 百炼直连地址。

**步骤三：配置环境变量**

`       1  2          export ANTHROPIC_BASE_URL="https://dashscope-intl.aliyuncs.com/compatible-mode/anthropic"   export ANTHROPIC_API_KEY="你的百炼API Key"            `

**步骤四：指定模型**

在 Claude Code 启动后，你可能需要通过 `/model` 命令指定使用的模型名称（如 `qwen-code` 或 `qwen3-coder-plus`）。具体模型名参考百炼平台的模型列表。

#### 3.2.3 其他国产模型选项速览

服务商

推荐模型

兼容端点

特点

智谱 AI

GLM-5.1

`open.bigmodel.cn/api/anthropic`

综合能力强，中文理解好

阿里百炼

Qwen Code

`dashscope-intl.aliyuncs.com/compatible-mode/anthropic`

编程专项优化，速度快

DeepSeek

DeepSeek-V3/V4

通过 OpenRouter 或华为云 MaaS 中转

性价比极高，约 Claude 1/10 价格

MiniMax

ABAB 7

官方 Anthropic 兼容接口（需查阅最新文档）

多模态能力

Moonshot

Kimi Code

通过中转服务

长上下文支持好

**选型建议**：

-   日常编程： 智谱 GLM 或阿里 Qwen Code，两者差距不大，看谁送得多选谁
-   成本敏感： DeepSeek，价格最低
-   复杂架构设计： 考虑用路径 B（中转 API）连官方 Claude 模型

### 3.3 路径B：中转 API 使用官方 Claude 模型（高阶场景）

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

#### 什么场景下值得用？

国产模型在日常编程任务上已经相当不错了，但在以下场景中，官方 Claude 模型（特别是 Opus 系列）仍然有明显优势：

-   复杂系统架构设计，需要深度逻辑推理
    
-   跨多个服务的大型重构，需要全局理解
    
-   需要理解大量上下文（>100K tokens）的遗留代码分析
    
-   需要最高质量的代码审查和安全分析
    

#### 中转服务的选择

中转 API 服务的原理是：服务商在海外部署服务器连接 Anthropic 官方 API，然后在国内提供接入点转发请求。你不需要自己翻墙，也不用海外支付。

**选择中转服务时的注意事项**：

-   延迟和稳定性是关键，价格倒在其次
    
-   优先选择有 SLA 承诺的服务商
    
-   注意安全：API 中转商理论上可以记录你的请求内容，敏感项目慎用
    

> **声明**：本书不推荐具体的中转服务商，因为这类服务的可用性和质量变化很快。读者可以在国内技术社区（如 V2EX、知乎）搜索“Claude API 中转”获取最新的服务商评价。

#### 配置方式

使用中转服务时，环境变量配置与国产模型类似，只是把 BASE\_URL 和 API\_KEY 换成中转商提供的地址和 Key：

`       1  2          export ANTHROPIC_BASE_URL="中转商提供的地址"   export ANTHROPIC_API_KEY="中转商提供的Key"            `

#### “混合方案”：日常用国产，关键时刻用中转

这是作者最推荐的策略：

-   80% 的日常任务： 国产模型（路径 A），低成本、低延迟
-   20% 的复杂任务： 中转 API 使用 Claude Opus 4.5，要最高质量

在同一个会话中，你可以随时用 `/model` 命令切换模型——日常编码用 GLM，遇到难题切到 Opus，解决问题后再切回来。第 12 章会详细讲解这种混合策略的成本优化方法。

### 3.4 路径C：本地模型零成本方案（探索性方案）

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

如果你对数据隐私有极高要求，或者想完全零成本使用 Claude Code，可以尝试本地模型方案。

#### Ollama 本地部署

Ollama 是一个免费的开源工具，可以在本地运行各种开源大模型。

**安装 Ollama**：

`       1  2  3  4  5  6  7  8          # macOS   brew install ollama       # Linux   curl -fsSL https://ollama.com/install.sh | sh       # Windows   # 从 ollama.com 下载安装包            `

**下载模型**（以 Qwen 3 为例）：

`       1          ollama pull qwen3:32b            `

模型大小从 7B 到 72B 不等，32B 是一个性能和速度的折中选择。下载时间取决于你的网速，可能需要几十分钟。

**启动 Ollama**：

`       1          ollama serve            `

**配置 Claude Code 连接本地模型**：

使用社区工具 `openclaude-cn` 或直接配置环境变量：

`       1  2          export ANTHROPIC_BASE_URL="http://localhost:11434/v1"   export ANTHROPIC_API_KEY="ollama"  # Ollama 不需要真实 Key            `

#### 适用场景与局限

**适合**：

-   简单的文件操作、文档生成、数据整理
    
-   对数据隐私极度敏感的场景
    
-   断网环境
    

**不适合**：

-   复杂的编程任务（本地模型的代码能力远弱于云端模型）
    
-   需要理解大项目的上下文（本地模型上下文窗口有限）
    
-   需要多步骤推理的任务
    

**推荐组合**：简单任务用本地模型（零成本）+ 复杂任务按需调用云端 API（付费但值得）。这是既能省钱又能保证复杂任务质量的策略。

### 3.5 快速切换模型的工具推荐

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

一旦你配置了多个模型源（国产、中转、本地），频繁修改环境变量会非常麻烦。以下工具可以帮你快速切换。

#### Claude Code 内置 `/model` 命令

在 Claude Code 交互界面中，直接输入：

`       1          /model            `

系统会列出可用模型，你可以选择切换。但这个功能要求你的 API 服务端支持模型列表查询。

#### CC Switch

如第 2 章介绍的，CC Switch 提供了图形化界面或 CLI 命令来切换模型源：

`       1  2  3          cc-switch use glm      # 切换智谱   cc-switch use qwen     # 切换通义千问   cc-switch use deepseek # 切换 DeepSeek            `

这种方式不需要手动改环境变量，切换一个命令搞定。

### 本章小结与配置验证

完成本章后，你的 Claude Code 应该已经接上了模型。做一个最终验证：

**第一步：确认环境变量已设置**

`       1  2          echo $ANTHROPIC_BASE_URL   echo $ANTHROPIC_API_KEY            `

应该输出你配置的地址和 Key。如果没有输出，说明环境变量没配好，回到对应小节检查。

**第二步：启动 Claude Code**

`       1          claude            `

**第三步：做一个简单测试**

在 Claude Code 中提问：

`       1          用 Python 写一个函数，计算斐波那契数列的第 n 项，包含简单的测试。            `

如果 Claude Code 能够生成代码并（可能）执行测试，说明一切正常。

**如果遇到问题**，请参考本章各小节的排错提示，以及附录 B 的常见错误汇总。

下一章，我们将执行你的第一个正式任务——真正让 Claude Code 帮你做点实事。
