# Claude Code零基础小白教程第12章：Token 节省完全指南：用最少的钱，办最大的事

> **第四阶段**：效率与技巧——Skill 深度玩法、Token 节省与实用生态
> 
> **本阶段目标**：掌握需求表达的黄金法则、Token 节省的实用技巧、社区精选 Skill 的深入使用，以及 Claude Code 与 IDE、浏览器、自动化工具的生态集成。让你从“会用”升级为“用得聪明、用得省”。
> 
> 本阶段共四章，从第11章节到第14章节

Token 是 Claude Code 的“燃料”，也直接关系到你的使用成本。本章将帮你搞清楚每一分钱花在哪里，以及如何用最少的 Token 完成最多的任务。

### 12.1 理解 Token 消耗：每一分钱花在哪里

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/MiaEMf64Ymog3gEQQDntunAtNWz0tZntib9cD8Fl1LpFAI5pbGavlkanashzvda4m6YYfX6dPfLWaxibLf8GicLdXnMO23fC0xI53FUbqqNZVK8/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=0)

#### Token 消耗的可视化

在任何时候，你都可以在 Claude Code 中输入以下命令查看当前会话的 Token 消耗：

`       1          /usage            `

输出类似：

`       1  2          Token usage: 45,230 input | 8,912 output | 54,142 total   Cost estimate: $0.23 (GLM-5.1)            `

这会告诉你：

-   输入消耗了多少 Token（你发的消息 + 系统提示 + 文件内容）
    
-   输出消耗了多少 Token（Claude Code 的回复）
    
-   估算费用
    

#### 哪些操作最费 Token

操作

Token 消耗

原因

读取大文件（>1000 行）

高

整个文件内容作为输入

冗长对话历史

高

每轮对话都累积在上下文中

重复上下文

高

同样的信息被多次发送

搜索代码库

中

搜索结果作为输入

执行命令并读取输出

中

命令输出作为输入

单次简短问答

低

输入和输出都少

**最大的 Token 杀手**：对话历史过长。每当你和 Claude Code 多聊一轮，整段对话历史都会被重新发送给模型。一个 50 轮的对话，每次请求都带着前面 49 轮的完整内容——这是 Token 消耗的指数级增长。

#### 系统提示、工具调用、思考链的 Token 分配

Claude Code 每次发请求时，Token 预算大致分配如下：

组成部分

占比

说明

系统提示（System Prompt）

5-10%

Claude Code 的基础指令，不可减少

工具定义（Tool Definitions）

5-10%

可用工具的说明，不可减少

Skills 加载

1-3%

已触发的 Skills 文档，按需加载

对话历史

40-70%

**主要的 Token 消耗来源，可优化**

文件和代码上下文

10-30%

读取的文件内容，可选择性加载

模型输出

10-20%

回复的 Token

优化对话历史和文件上下文，是 Token 节省的核心战场。

### 12.2 节省 Token 的 10 个实用技巧

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

以下 10 个技巧按“投入产出比”排序，越靠前的越优先实施。

#### 技巧 1：精确选择文件范围

不要直接说“帮我分析这个项目”，Claude Code 会试图读取所有文件。使用 `@file` 指定具体的文件：

❌ **浪费 Token 的做法**：

`       1          帮我分析用户模块            `

（Claude Code 可能会搜索并读取所有包含“user”的文件）

✅ **省 Token 的做法**：

`       1          帮我分析 @file:src/modules/user/user.service.ts 和 @file:src/modules/user/user.controller.ts 这两个文件            `

（只读取指定文件）

#### 技巧 2：善用 `.claudeignore`

在项目根目录创建 `.claudeignore` 文件，排除不需要 AI 关注的大目录和文件：

`       1  2  3  4  5  6  7  8  9  10  11  12  13  14  15          node_modules/   dist/   build/   *.log   package-lock.json   yarn.lock   .git/   coverage/   .next/   .cache/   *.png   *.jpg   *.svg   *.woff   *.woff2            `

这些目录和文件类型通常不需要 AI 读取，排除后能大幅减少意外读取带来的 Token 浪费。

#### 技巧 3：使用 `/compact` 压缩对话历史

前面已经提到，这里再强调一次：当对话超过 30 轮，一定记得 `/compact`。它能将对话历史压缩到原来的 20-30%，同时保留关键决策信息。

#### 技巧 4：优先用国产便宜模型完成 80% 的简单任务

国产模型的价格通常是官方 Claude 模型的 1/5 到 1/10。日常简单任务用国产模型，只有复杂架构设计、深度调试等场景才切换到更强的模型。

**价格参考（2026 年 5 月，以百万 Token 计）**：

模型

输入价格

输出价格

适合场景

GLM-5.1

¥1-2

¥2-4

日常编码

DeepSeek-V4

¥0.5-1

¥1-2

批量处理

Qwen Code

¥1-2

¥2-3

编程专项

Claude Opus 4.5（中转）

¥30-50

¥60-100

复杂架构

同样的任务，用 DeepSeek 比用 Claude Opus 省 90% 以上的成本。

#### 技巧 5：稳定信息写入 CLAUDE.md

每次对话都重复告诉 Claude Code 项目信息，是对 Token 的极大浪费。把稳定的背景信息写入 CLAUDE.md，它每次启动自动加载，不需要你在对话中重复。

#### 技巧 6：明确要求“只输出关键代码”

在需求中加上输出约束，减少模型生成冗余解释：

`       1          修改这个函数，只输出关键代码改动，不需要解释。            `

这样模型的输出 Token 可以减少 50-70%。

#### 技巧 7：用 Skill 固化常用流程

Skill 文件本身只消耗 30-50 Token 的加载成本，但能避免你在每次对话中重复描述流程。一个项目如果有 3-5 个常用的操作流程，用 Skill 固化下来，长期来看能省下大量 Token。

#### 技巧 8：分批处理大文件

修改一个 2000 行的大文件时，不要一次性全读进去。告诉 Claude Code 关注特定区域：

`       1          只关注 user.service.ts 的第 50-100 行，修改这部分代码            `

这样只有指定行范围的内容被加载，而不是整个文件。

#### 技巧 9：关闭不必要的 MCP 工具

每配置一个 MCP 服务器，其工具定义都会增加系统提示的长度。如果你暂时不需要某个 MCP 工具，在 `.mcp.json` 中注释掉或删除它。

#### 技巧 10：用 `/model` 随时切换

在同一个会话中，你可以根据当前任务的需要随时切换模型：

`       1          /model            `

复杂任务用强模型，简单任务切回便宜模型。一个会话中可以切换多次。

### 12.3 不同场景下的 Token 策略模板

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

#### 新功能开发

阶段

推荐模型

原因

需求分析 + 架构设计

Claude Opus（中转）

需要深度推理

代码实现

GLM / Qwen Code

日常编码能力足够

测试生成

GLM / DeepSeek

成本低，任务标准化

#### 代码审查

`       1          只审查 git diff 的变更部分，不要读取整个文件。            `

只审查变更部分而不是整个文件，Token 消耗减少 80% 以上。

#### 文档生成

文档生成通常是标准化、大批量的任务——用最便宜的模型批量生成初稿，然后人工精修或用一个强模型做最终润色。

#### 紧急 Bug 排查

别纠结成本。直接用最强模型、全速处理。这种场景下的时间成本远比 Token 成本高。
