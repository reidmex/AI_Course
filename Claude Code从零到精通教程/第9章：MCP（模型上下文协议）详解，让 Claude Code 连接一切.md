---
title: "Claude Code零基础小白教程第9章：MCP（模型上下文协议）详解，让 Claude Code 连接一切"
url: https://mp.weixin.qq.com/s?__biz=MjM5NzQ0NzIyOQ==&mid=2448131874&idx=1&sn=63a8a91ecee86d382bcab7c54842affb&chksm=b2c6f47085b17d66eaca1d67661f9e963262f23fab3abec7f4cd203dd1d68a3ebba2179c5c9b&cur_album_id=4566215426073788416&scene=189#wechat_redirect
byline: "萝卜啊"
saved: 2026-06-26T10:00:02.191Z
---
# Claude Code零基础小白教程第9章：MCP（模型上下文协议）详解，让 Claude Code 连接一切

> **第三阶段**：进阶精通——打造你的 AI 工作流
> 
> **本阶段目标**：掌握 CLAUDE.md、Skills、MCP、Hooks 四大进阶能力，让 Claude Code 从“能用”升级为“懂你”的专属 AI 工作伙伴。本阶段共四章节，第7章节到第10章节

### 9.1 MCP 是什么：AI 世界的“USB 接口”

![01-section](https://mmbiz.qpic.cn/sz_mmbiz_jpg/MiaEMf64YmohJpoG3VXiaROcjAJxSN8jEsfx61ANKuIdIIFukhicIWZQ3D5fzx6bxymotzrM2qmflXIEnH5fibTCoH5gu12WQntT6x80mXtiaHHA/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=0)

01-section

如果说 Skills 是给 AI 写的“内部工作流程”，那么 MCP（Model Context Protocol，模型上下文协议）就是 AI 连接外部世界的“接口标准”。

**用 USB 来类比**：在 USB 出现之前，鼠标用 PS/2 口，键盘用 AT 口，打印机用并口……每种设备都有自己独特的接口。USB 出现之后，一个接口搞定所有设备。

MCP 在 AI 领域做的是同一件事：它为 AI 连接各种外部工具和数据源定义了一个统一的协议。无论是连接数据库、访问 API、操作 GitHub Issues 还是搜索网页，AI 只需要知道“MCP 怎么用”，而不是“PostgreSQL 的驱动怎么装、GitHub 的 API 文档怎么查”。

#### MCP 与内置工具的关系

Claude Code 自带一组内置工具（Bash 命令执行、文件读写、搜索），这些是“基础能力”。MCP 是“扩展能力”——当你需要内置工具覆盖不到的功能时，通过 MCP 接入专门的服务器来扩展。

**内置工具可以做的事**：

-   读写本地文件
    
-   执行终端命令
    
-   搜索项目代码
    
-   Git 操作
    

**MCP 扩展后可以做的事**：

-   查询数据库（`@anthropic/mcp-server-postgres`）
    
-   操作 GitHub Issues/PR（`@anthropic/mcp-server-github`）
    
-   搜索网页（`@anthropic/mcp-server-brave-search`）
    
-   读写 Notion/飞书文档
    
-   调用任何 REST API
    

### 9.2 配置你的第一个 MCP 服务器

![02-section](https://mmbiz.qpic.cn/sz_mmbiz_jpg/MiaEMf64YmogPB9YuQ1armLNsHMPpIRJIPt16sdYH7bPrAjZaKTqARE5iag1596gx3RE1V0nkMrkDLm6khC8ibJltkyYqycU4YLUeOMliaEiaPys/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=1)

02-section

\#### 配置文件位置

MCP 服务器可以配置在两个级别：

-   项目级： `.mcp.json`（放在项目根目录），只在当前项目中生效
-   用户级： `~/.claude.json`（放在用户主目录），对所有项目生效

推荐做法：把项目相关的 MCP 配置放在 `.mcp.json`（随 Git 共享给团队），把个人工具（如 Notion、飞书）的 MCP 配置放在 `~/.claude.json`。

#### 实战 MCP 1：接入 GitHub

**第1步**：安装 GitHub MCP 服务器
```
npm install -g @anthropic/mcp-server-github
```

**第2步**：获取 GitHub Personal Access Token

1.  打开 github.com/settings/tokens
    
2.  点击“Generate new token (classic)”
    
3.  勾选 `repo` 和 `read:org` 权限
    
4.  生成并复制 Token
    

**第3步**：配置 `.mcp.json`（项目级）
```
{
     "mcpServers": {
       "github": {
         "command": "npx",
         "args": [
           "-y",
           "@anthropic/mcp-server-github"
         ],
         "env": {
           "GITHUB_PERSONAL_ACCESS_TOKEN": "你的GitHub Token"
         }
       }
     }
}
```

**第4步**：验证连接

在 Claude Code 中输入：
```
列出这个仓库最近 5 个 Issue
```

如果一切正常，Claude Code 会调用 GitHub MCP 服务器，获取并列出 Issue。

**现在你可以做的事**：

-   “创建一个 Issue 来记录这个 Bug”
    
-   “把 Issue #12 分配给我”
    
-   “查看这个 PR 的改动，做一个代码审查”
    
-   “列出所有带有 bug 标签的 Issue”
    

#### 实战 MCP 2：接入文件系统（扩展文件操作能力）

文件系统 MCP 服务器可以让你操作项目目录之外的文件：
```
{
     "mcpServers": {
       "filesystem": {
         "command": "npx",
         "args": [
           "-y",
           "@anthropic/mcp-server-filesystem",
           "/path/to/allowed/directory"
         ]
       }
     }
}
```

注意 `/path/to/allowed/directory` 是你允许 Claude Code 访问的目录。默认情况下，Claude Code 只能访问项目内的文件。如果你需要它处理桌面上的文件或文档文件夹中的内容，就需要通过文件系统 MCP 来扩展权限。

### 9.3 常用 MCP 服务器推荐

![03-section](https://mmbiz.qpic.cn/sz_mmbiz_jpg/MiaEMf64YmoiaBs7LshBWQrRVkfWPlicc0QyOxmRqiaRnGqcPh0LZH5cUR6fFAib9myAqUGKZmuXAWJ8icibN1EExQknCkLwuK9aqhhI0mZUObkLHg/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=2)

03-section

#### 适合程序员的

MCP 服务器|功能|安装命令
-|-|-|
Postgres|直连数据库，自然语言查询|`npm install -g @anthropic/mcp-server-postgres`
Docker|管理容器、查看日志|`npm install -g @anthropic/mcp-server-docker`
Puppeteer|浏览器自动化，用于 E2E 测试|`npm install -g @anthropic/mcp-server-puppeteer`
Git|增强 Git 操作（超越内置能力）|`npm install -g @anthropic/mcp-server-git`

#### 适合非程序员的

MCP 服务器|功能|安装命令
-|-|-|
Brave Search|联网搜索（需要 Brave API Key）|`npm install -g @anthropic/mcp-server-brave-search`
Notion|读写 Notion 文档和数据库|社区维护，安装方式见其 GitHub
Google Drive|操作 Google Drive 文件|社区维护
Slack|发送消息、读取频道|社区维护

#### 安装与配置速查

所有 MCP 服务器的配置格式基本一致：

`       1  2  3  4  5  6  7  8  9  10  11          {     "mcpServers": {       "服务器名称": {         "command": "命令",         "args": ["参数1", "参数2"],         "env": {           "环境变量名": "值"         }       }     }   }            `

把它们添加到 `.mcp.json`（项目级）或 `~/.claude.json`（用户级）即可。

### 9.4 MCP + Skills 的协同威力

![04-section](https://mmbiz.qpic.cn/mmbiz_jpg/MiaEMf64Ymoiaehibx4rnrvALIIyNdQEA6RKVQlKAzHa5HCAqqcE77ibLMicJmuH4icRr5GFDlPJLrzPujFVicgO7FibicNoYo5D6c2tCsdAn2YuquPo/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=3)

04-section

MCP 提供了“做什么”的能力，Skills 提供了“怎么做”的流程。两者结合，可以创造出强大的自动化工作流。

#### 实战案例：“竞品分析报告自动生成”工作流

这个工作流结合了：

-   Brave Search MCP： 联网搜索竞品的最新动态
-   competitor-analysis Skill： 定义分析报告的模板和流程

**操作步骤**：

1.  先配置好 Brave Search MCP（需要注册 Brave Search API Key）
    

`       1  2  3  4  5  6  7  8  9  10  11          {     "mcpServers": {       "brave-search": {         "command": "npx",         "args": ["-y", "@anthropic/mcp-server-brave-search"],         "env": {           "BRAVE_API_KEY": "你的Brave API Key"         }       }     }   }            `

1.  创建一个 competitor-analysis Skill（SKILL.md），定义分析维度：公司概况、产品功能、定价策略、市场定位、最新动态
    
2.  在 Claude Code 中启动工作流：
    

`       1  2  3  4  5  6          请用 competitor-analysis Skill，搜索并分析以下三家竞争对手的最新情况：   - 竞争对手 A   - 竞争对手 B   - 竞争对手 C       把结果整理成一份竞品分析报告，保存为 competitor-report.md            `

Claude Code 的执行过程：

1.  加载 competitor-analysis Skill，了解需要收集哪些信息
    
2.  调用 Brave Search MCP，搜索每家竞争对手的信息
    
3.  将搜索结果按 Skill 定义的模板整理
    
4.  生成报告并保存
    

**整个流程，你不需要打开浏览器、复制粘贴、排版——Claude Code 通过 MCP 获取信息，通过 Skill 定义输出格式，一气呵成。**
