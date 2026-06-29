---
title: "Claude Code零基础小白教程第11章：使用技巧，让 Claude Code 变得更“聪明”"
url: https://mp.weixin.qq.com/s?__biz=MjM5NzQ0NzIyOQ==&mid=2448131892&idx=1&sn=6400fabfb0c732408516349accf4c6f6&chksm=b2c6f46685b17d70019702c1de804d90ed97bc8d10774c83430f0eadf764eb06163c3c861013&cur_album_id=4566215426073788416&scene=190#rd
byline: "萝卜啊"
saved: 2026-06-29T06:10:44.712Z
---
# Claude Code零基础小白教程第11章：使用技巧，让 Claude Code 变得更“聪明”

> **第四阶段**：效率与技巧——Skill 深度玩法、Token 节省与实用生态
> 
> **本阶段目标**：掌握需求表达的黄金法则、Token 节省的实用技巧、社区精选 Skill 的深入使用，以及 Claude Code 与 IDE、浏览器、自动化工具的生态集成。让你从“会用”升级为“用得聪明、用得省”。
> 
> 本阶段共四章，从第11章节到第14章节

  

Claude Code 的能力上限很大程度上取决于你怎么跟它沟通。同样的工具，有人用它写出生产级代码，有人觉得它“不太聪明”——差距往往不在工具，而在使用技巧。

### 11.1 需求表达的黄金法则

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/MiaEMf64Ymogx00ol8y9phRCHiacia1NAJdu7kbje4bibs1ogdFooFQH8vGapI3LfxlCwpsx6ac5UiaYmicSQz4ajURYzFf5aBqPuYQ7JMXOYv0bA/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=0)

#### 非程序员必看：从“模糊想法”到“可执行指令”的转化模板

Claude Code 不是读心术师。你脑子里的“帮我把这个弄好”，对它来说是一道信息量为零的指令。把模糊想法转化为可执行指令，只需要填充以下三个要素：

**三要素模板**：

`       1  2  3          [目标]：你要达成什么结果？   [约束]：有什么限制条件？   [验收标准]：怎么判断任务完成了？            `

**一个烂需求 vs 一个好需求的对比**：

❌ **烂需求**：

`       1          帮我把这些数据处理一下。            `

Claude Code 会怎么想？“处理”是什么意思？排序？去重？统计？输出什么格式？存哪里？

✅ **好需求**：

`       1  2  3  4          帮我处理 sales.csv 文件：   - 目标：统计每个月的总销售额，找出销售额最高的三个月份   - 约束：使用 Python 处理，图表用蓝白配色   - 验收标准：生成一个 report.md 包含统计表格和柱状图，图表保存为 chart.png            `

Claude Code 一目了然，执行路径清晰，一次性做对的概率大幅提升。

#### 使用“假设你是一个……”角色设定

这是让 Claude Code 进入专家模式最有效的方法。在需求开头加上一句角色设定，模型的输出质量会明显提升。

**示例对比**：

❌ 不加角色：

`       1          帮我审查这段代码。            `

✅ 加角色：

`       1          假设你是一个有 10 年经验的资深后端工程师，专精于 Node.js 安全审计。请审查以下代码，重点关注 SQL 注入风险和认证漏洞。            `

**为什么有效**：大模型在训练时学习了海量文本，其中包括各领域专家的写作风格和思维模式。“角色设定”相当于激活了模型参数中与这个角色最相关的权重分布，让它用专家视角思考问题。

**常用角色设定参考**：

场景

推荐角色设定

代码审查

“你是一个资深安全工程师”

架构设计

“你是一个有 15 年经验的系统架构师”

文档写作

“你是一个技术文档撰写专家”

数据分析

“你是一个数据分析师，擅于从数据中发现业务洞察”

UI 设计

“你是一个 UI/UX 设计专家，注重视觉美感和用户体验”

### 11.2 任务拆解与分步确认技巧

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

#### 复杂任务不要一次性全给

很多新手容易犯一个错误：把一整个复杂需求写成一大段话，一股脑丢给 Claude Code。

比如这样：

`       1          帮我做一个电商后台管理系统，包含用户管理、商品管理、订单管理、数据统计，用户管理要支持注册登录权限控制，商品管理要支持分类和搜索……            `

这种做法的问题在于：

-   Claude Code 可能遗漏某些要求
    
-   中间某个步骤出错后，整个流程中断
    
-   你不确定它理解得对不对，只能等最终结果出来才发现偏差
    

**正确做法：分步推进，每步确认。**

`       1          先帮我在当前项目下创建一个 Express 后端项目骨架，包含基础的路由和错误处理中间件。            `

等这一步完成后：

`       1          现在添加用户管理模块，包含注册、登录、获取用户信息三个接口。先展示你的实现计划。            `

审阅计划，调整，确认：

`       1          计划没问题，开始实现。实现完运行测试。            `

以此类推。这种“小步快跑”的方式能确保每一步都在你的掌控之中。

#### 活用 `/plan` 模式：零成本试错

Claude Code 支持 Plan Mode（计划模式）——让它先制定方案，不做实际修改。这是安全使用 Claude Code 的必备习惯。

`       1          /plan 把用户模块从回调改成 async/await            `

Claude Code 会输出一个详细的修改计划：

-   需要修改的文件列表
    
-   每个文件的具体改动说明
    
-   潜在的风险和注意事项
    

你审阅后，可以：

-   “执行” —— 按计划执行
    
-   “第 3 步不要动，其他执行” —— 调整计划
    
-   “这个方案不太好，换一种方式” —— 让它重新规划
    

> **一个实用心法**：涉及文件修改超过 3 个的任务，一律先 `/plan` 再看结果。这几秒钟的确认能帮你省下大量的返工时间。

### 11.3 利用记忆与知识库减少重复劳动

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

#### `/memory` 命令：让 Claude Code 记住关键信息

Claude Code 支持会话内记忆功能。你可以用 `/memory` 命令让它记住当前项目的特定约定：

`       1          /memory 这个项目使用 React 18 + TypeScript，状态管理用 Zustand，API 请求封装在 src/api/ 目录下。所有组件放在 src/components/，页面放在 src/pages/。            `

之后在整个会话中，Claude Code 会记住这些信息。新建组件时会自动放在正确目录，调用 API 时会使用项目的封装方式。

#### 记忆内容的组织策略：建一个“项目字典”

建议在每个项目的 CLAUDE.md 中维护一个“项目字典”板块：

`       1  2  3  4  5  6          ## 项目字典   - API 请求：使用 src/api/request.ts 中的 request 函数   - 状态管理：使用 Zustand，store 文件放在 src/stores/   - 组件库：使用项目的 src/components/ui/ 中的封装组件   - 时间处理：使用 dayjs，不要用 moment   - 样式方案：使用 Tailwind CSS，自定义颜色在 tailwind.config.js 中            `

这样 Claude Code 每次启动时就会获得完整的“词汇表”，不会用错技术栈。

#### CLAUDE.md + Memory 的协同分工

CLAUDE.md

/memory

生效范围

永久，每次启动自动加载

当前会话

适合内容

项目级别的约定（技术栈、目录结构）

临时的上下文（当前任务目标、临时决策）

维护方式

编辑文件，随 Git 版本管理

会话中用 `/memory` 命令随时添加

**最佳实践**：稳定的项目约定写入 CLAUDE.md，临时的任务上下文用 `/memory`。不要把临时信息写进 CLAUDE.md，否则会污染后续所有会话。

### 11.4 反模式与常见陷阱

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

#### “这个 AI 好像变蠢了”——上下文过长的罪魁祸首

如果你发现 Claude Code 在一个长时间的会话中表现越来越差——忘记之前的约定、输出质量下降、重复犯同样的错误——这大概率是**上下文过长**导致的。

Claude Code 的上下文窗口虽然很大（200K tokens），但随着对话历史不断累积，早期的关键信息会被“稀释”，模型对上下文中不同部分的注意力会分散。

**解决方法**：使用 `/compact` 命令。

`       1          /compact            `

这个命令会压缩对话历史，保留关键决策点和上下文，丢弃冗余的中间过程。压缩后的对话历史更“浓缩”，模型的注意力更集中，输出质量会明显回升。

**什么时候该用 /compact**：

-   会话进行了超过 30 轮对话
    
-   Claude Code 开始“忘记”你之前说过的重要信息
    
-   输出质量明显下降
    

#### 避免使用否定式指令

AI 模型在处理否定句时容易出现“否定忽略”的现象——它可能忽略了“不要”而只关注了后面的词。

❌ **否定式指令**：

`       1          不要使用 jQuery            `

Claude Code 可能会“看到”jQuery 这个词，然后在代码中不经意地引入。

✅ **正向约束**：

`       1          使用原生 JavaScript 或 React 实现            `

这样指令清晰，没有歧义，Claude Code 会严格按照正向约束来选择技术方案。

**更多示例**：

-   ❌ “不要写太复杂的代码” → ✅ “代码尽量简洁，每个函数不超过 20 行”
    
-   ❌ “不要忘记错误处理” → ✅ “每个 API 调用都要有 try-catch 错误处理”
    
-   ❌ “命名不要太随意” → ✅ “变量命名使用描述性的名称，如 getUserById 而非 get”
    

#### 多任务切换时的会话管理

当你同时处理多个任务时，不要在一个会话里切换来切换去。Claude Code 会把所有对话历史都带在上下文中，不同任务的背景信息会互相干扰。

**推荐做法**：

-   每个独立任务启动一个新的 Claude Code 会话
    
-   如果需要同时进行多个任务，使用 Git Worktree 隔离（详见第 23 章）
    
-   任务完成后用 `/compact` 清理上下文，再开始下一个任务
