---
title: "Claude Code零基础小白教程第8章：Skills（技能）详解，把“最佳实践”固化下来"
url: https://mp.weixin.qq.com/s?__biz=MjM5NzQ0NzIyOQ==&mid=2448131865&idx=1&sn=ce37e01a852aa499011e03e103508d7a&chksm=b2c6f44b85b17d5db1ff020dbfd9d687356521114cb54ea2f79f1e980ee39ca66861818234bd&cur_album_id=4566215426073788416&scene=189#wechat_redirect
byline: "萝卜啊"
saved: 2026-06-26T09:59:36.760Z
---
# Claude Code零基础小白教程第8章：Skills（技能）详解，把“最佳实践”固化下来

> **第三阶段**：进阶精通——打造你的 AI 工作流
> 
> **本阶段目标**：掌握 CLAUDE.md、Skills、MCP、Hooks 四大进阶能力，让 Claude Code 从“能用”升级为“懂你”的专属 AI 工作伙伴。本阶段共四章节，第7章节到第10章节

### 8.1 Skills 是什么：给 AI 写的 SOP（标准操作流程）

![01-section](https://mmbiz.qpic.cn/sz_mmbiz_jpg/MiaEMf64YmoiaibKQTxxpXebdQEQqEyS2ECZxNyKibdoPiaicictSo3doDQ8964JhhrwiaE8h6oS95WJSoFcS6aia5d6DaQgKlppUVRm6VAr8UuR19QY/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=0)

01-section

如果你用过制造企业的 SOP（标准操作流程），Skills 的概念就非常好理解。

一个 Skill 就是一份 Markdown 文档，里面写清楚了一件事的“标准做法”。当你触发这个 Skill 时，Claude Code 会读取这份文档，然后严格按照文档中定义的流程来执行。

**Skills 的底层原理非常简单**：

-   它是一个 Markdown 文件（通常命名为 `SKILL.md`），放在项目的 `.claude/skills/` 目录下
    
-   Claude Code 不会自动加载所有 Skills——只在需要时按需加载
    
-   每次加载一个 Skill 只消耗约 30-50 个 Token（因为只是读取一份指令文档，不消耗模型推理成本）
    

**Skills 和传统工作流工具的区别**：

Skills（Claude Code）

n8n / Dify 等传统工作流

工作方式

智能导航：AI 理解流程含义，灵活适应变化

硬编码：每个步骤固定，遇到意外就报错

适应性

高——如果中间步骤条件变化，AI 会自动调整

低——条件判断需要提前定义所有分支

维护成本

低——改 Markdown 就行，AI 自然理解

高——需要修改节点配置，可能影响整个流水线

适合场景

需要一定灵活度的重复任务

完全固化的自动化任务

### 8.2 创建你的第一个 Skill

![02-section](https://mmbiz.qpic.cn/sz_mmbiz_jpg/MiaEMf64YmohCIf3q89ibjvjzXljopnHpZEFuhwGj6TiaFxpQ3fa6OcTlic2L9XlhszkQkDYklOC7OEJuiayToYXeHUJHGmuz6CiawVpFntlRsJP8/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=1)

02-section

#### Skill 的文件结构

一个 Skill 通常包含以下文件：

`       1  2  3  4  5  6          .claude/skills/   └── my-skill/       ├── SKILL.md       # 核心：Skill 的定义和流程（必需）       ├── template.md    # 辅助：输出模板（可选）       ├── examples/      # 辅助：案例参考（可选）       └── scripts/       # 辅助：常用脚本（可选）            `

`SKILL.md` 是唯一必需的文件。其他文件是辅助资源，Claude Code 在执行 Skill 时会按需读取。

#### 实战 Skill 1：“周报生成器”

这是一个跨岗位通用的 Skill，帮你从零散的工作记录中自动生成结构化周报。

**创建 Skill 文件**：

`       1          mkdir -p ~/.claude/skills/weekly-report            `

在 `.claude/skills/weekly-report/SKILL.md` 中写入：

``       1  2  3  4  5  6  7  8  9  10  11  12  13  14  15  16  17  18  19  20  21  22  23  24  25  26  27  28  29  30  31  32  33  34  35  36  37  38  39  40  41  42  43  44  45  46  47  48  49  50          # 周报生成器 Skill       ## 触发条件               当用户说"生成周报"、"写周报"、"weekly report"时，自动加载此 Skill。       ## 执行流程               ### 第一步：收集素材   1. 查找当前目录下是否有以下文件：      - `本周工作记录.md` 或 `todo.md`      - Git 提交记录（如果是代码项目）   2. 如果找不到任何素材，询问用户提供本周的工作内容       ### 第二步：整理信息   将收集到的内容按以下维度分类：   - **本周完成**：已完成的任务   - **进行中**：正在进行的任务及进度   - **遇到的问题**：阻塞项和风险   - **下周计划**：下一步安排       ### 第三步：生成周报   按以下模板输出周报：       ---   # [姓名] 周报（[日期范围]）       ## 本周完成       - [具体事项1]   - [具体事项2]       ## 进行中       - [事项]（进度：X%）       ## 遇到的问题   - [问题描述]（解决方案/需要支持）       ## 下周计划   - [计划事项1]   - [计划事项2]   ---       ### 第四步：保存   将周报保存为 `周报-YYYY-MM-DD.md` 文件。            ``

**使用这个 Skill**：

在你的项目目录下启动 Claude Code，然后说：

`       1          帮我生成本周周报            `

Claude Code 会自动加载 `weekly-report` Skill，按照你定义的四步流程收集素材、整理信息、生成周报、保存文件。

#### 实战 Skill 2：“代码审查员”

这是一个面向程序员的 Skill，确保每次代码审查都按团队的规范执行。

在 `.claude/skills/code-review/SKILL.md` 中写入：

`       1  2  3  4  5  6  7  8  9  10  11  12  13  14  15  16  17  18  19  20  21  22  23  24  25          # 代码审查 Skill       ## 触发条件   当用户说"review"、"审查代码"、"code review"时自动加载。       ## 审查维度   检查以下方面，按严重程度标记（🔴严重 / 🟡建议 / 🟢优化）：       ### 🔴 必须修复   - 安全漏洞（SQL 注入、XSS、未验证的输入）   - 逻辑错误（可能导致运行时异常的代码）   - 破坏性变更（修改了公共 API 但未更新调用方）       ### 🟡 建议修复   - 性能问题（不必要的循环、重复查询）   - 错误处理缺失（未捕获的可能异常）   - 代码重复（可抽取为公共函数）       ### 🟢 优化建议   - 命名不够清晰   - 注释缺失或过时   - 可以用更简洁的写法       ## 输出格式   按文件分组输出审查结果，每条意见标注行号和严重程度。            `

#### Skill 的触发方式

Claude Code 提供两种 Skill 触发方式：

**自动匹配**：在 SKILL.md 的“触发条件”部分定义关键词，当用户输入匹配时自动加载。这是最常用的方式——用户甚至不知道背后有个 Skill 在工作，只觉得 Claude Code “很懂”。

**手动调用**：用户可以明确指定使用某个 Skill：

`       1          请用周报生成器 Skill 帮我生成本周周报            `

或者在 Claude Code 中列出所有可用 Skills：

`       1          列出我安装了哪些 Skill            `

### 8.3 常用现成 Skills 推荐与使用

![03-section](https://mmbiz.qpic.cn/mmbiz_jpg/MiaEMf64Ymoia5wicdxicQZaCZWlsR5ibEtNXGM3bL94jXM3wuL9icY0fG1QSFFicQTDibYzeDk8HQfg4ttCWHULKcq6j2icvkiaD1jibicTKYHrpd26plE/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=2)

03-section

在你自己写 Skill 之前，社区已经有大量成熟的 Skills 可以直接使用。

#### 内置 Skills

Claude Code 安装时自带一组文件处理 Skills，无需额外安装：

-   docx： 读取和创建 Word 文档
-   pptx： 读取和创建 PowerPoint 演示文稿
-   xlsx： 读取和创建 Excel 表格

当你处理这些文件格式时，Claude Code 会自动调用对应 Skill，你不需要特别指定。例如：

`       1          帮我把这个 Excel 里的数据按月份汇总，生成一个新的 Excel 文件            `

Claude Code 会自动用 xlsx Skill 处理。

#### 社区热门 Skills 推荐

以下 Skills 可以在 Claude Code 社区市场或 GitHub 上找到。安装方式通常是复制到 `.claude/skills/` 目录或使用 `npx skills install` 命令。

Skill 名称

用途

适合人群

meeting-minutes

将会议录音转文字或聊天记录整理为会议纪要

所有人

research-assistant

联网搜索 + 信息聚合 + 引用管理

产品/运营/市场

competitor-analysis

按模板输出竞品分析报告

产品/市场

db-query

用自然语言操作数据库

数据分析/开发

pdf-form

批量填写 PDF 表单

行政/运营

> **安装社区 Skill 的通用方式**：在 Claude Code 中输入 `/plugin install [skill-name]`，或直接在终端执行 `npx skills install [skill-name]`。部分 Skill 需要配合 MCP 服务器使用（如联网搜索），详见第 9 章。

#### 定制他人的 Skills

拿来的 Skill 不一定完全适合你的场景，修改起来很简单——直接编辑 SKILL.md 文件：

-   修改触发条件：调整关键词匹配规则
    
-   调整流程：在某个步骤后面添加你需要的操作
    
-   修改输出格式：换成你团队的报告模板
    

因为 Skill 的本质就是 Markdown 文件，修改它和修改一份文档一样容易。

### 8.4 Skills 的进阶技巧

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/MiaEMf64YmohV9UWKPaxibBYY2oEB4m8f1wgP8ibT4V1JqrhPYzqRNSnJRDU9BdsojMmdKvoRvVQlckAnBicydCIIsyic1mMqiblZkBgu5I384PVw/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=3)

#### 多个 Skills 的协同编排

一个复杂任务可以被拆解为多个 Skills 串联执行。例如，一个“竞品分析”任务可以这样编排：

1.  research-assistant Skill： 联网搜索竞品信息
2.  competitor-analysis Skill： 按模板整理分析
3.  weekly-report Skill： 将分析结果整合到周报

你不需要一次性触发所有 Skill。你可以分步告诉 Claude Code：

`       1          先用 research-assistant 搜索一下竞争对手 A、B、C 的最新动态            `

等搜索结果出来后：

`       1          现在用 competitor-analysis Skill 整理一份对比分析            `

最后：

`       1          把这份分析加到本周周报里            `

#### 跨平台 Skills

一份 SKILL.md 文件可以在以下三个平台通用：

-   Claude Code CLI（本地终端）
-   Claude.ai 网页版（通过 Projects 功能加载）
-   Anthropic API（通过 System Prompt 注入）

这意味着你在本地调好的 Skill，可以直接用到在线 API 或网页版中，不需要重复开发。

#### 团队 Skills 管理

把 `.claude/skills/` 目录加入 Git 版本管理：

`       1  2          git add .claude/skills/   git commit -m "docs: add team skills"            `

这样团队每个成员 clone 项目后，自动获得相同的 Skills 配置。当有人优化了某个 Skill 的流程，其他人 `git pull` 即可同步。

**建立团队 Skills 知识库**：

-   每个项目维护自己专属的 Skills（如该项目的部署流程）
    
-   组织级别的通用 Skills（如周报格式、代码审查规范）放在一个公共仓库中
    
-   新成员入职时，clone 项目 + clone Skills 仓库，Claude Code 即刻“熟悉”团队规范
