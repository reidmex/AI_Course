---
title: "Claude Code零基础小白教程第13章：精选 Skill 详解与实战"
url: https://mp.weixin.qq.com/s?__biz=MjM5NzQ0NzIyOQ==&mid=2448131913&idx=1&sn=c5ed0e1032544ec22d964fa164cf3d94&chksm=b2c6f41b85b17d0d345c1ada65e735ddea3760f2e1797533f55e6e82a8e52acd61bd3b2ed79f&cur_album_id=4566215426073788416&scene=190#rd
byline: "萝卜啊"
saved: 2026-07-01T09:38:18.654Z
---
# Claude Code零基础小白教程第13章：精选 Skill 详解与实战

> **第四阶段**：效率与技巧——Skill 深度玩法、Token 节省与实用生态
> 
> **本阶段目标**：掌握需求表达的黄金法则、Token 节省的实用技巧、社区精选 Skill 的深入使用，以及 Claude Code 与 IDE、浏览器、自动化工具的生态集成。让你从“会用”升级为“用得聪明、用得省”。
> 
> 本阶段共四章，从第11章节到第14章节

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/MiaEMf64YmogVQdTDJq7iaJdCdRJ0FA5QX1lSicUBXmDhGR76RWgeGbSqfyib1eS0EpBve3KqVICDpwSDkxECy3kQiaic0Q9JRibyJCuPtkelicaWuE/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=0)

**注意**：本章聚焦于日常高频、轻量级的实用 Skill。重量级工程方法论 Skill（Superpowers、gstack、Hermes、GSD 等）将在第五阶段（第16-24章）进行深度拆解。

### 13.1 Skill 生态速览与入门

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

#### 社区 Skill 分布概览

截至 2026 年 5 月，Claude Code 的 Skill 生态主要分布在这几个地方：

-   官方内置 Skills： docx、pptx、xlsx 文件处理
-   Claude Code 插件市场： 通过 `/plugin` 命令浏览和安装
-   GitHub 社区： 搜索 `claude-code-skill` 标签，超过 6 万个仓库
-   npm 生态： 部分 Skill 以 npm 包形式发布

#### 安装 Skill 的三种方式

`       1  2  3  4  5  6  7  8          # 方式一：通过插件市场   /plugin install [skill-name]       # 方式二：通过 npx 或 npm   npx skills install [skill-name]       # 方式三：手动克隆到 .claude/skills/   git clone [repo-url] ~/.claude/skills/[skill-name]            `

#### 判断好 Skill 的三个标准

1.  是否活跃维护： 最近三个月有更新
2.  Token 成本是否明确： 好的 Skill 会在 README 中说明加载成本
3.  是否符合你的工作流： 不要因为 Skill 很火就装——只装你真正需要的

#### 新手快速入门：`/powerup` 交互式教程

Claude Code 内置了一个交互式学习命令，可以帮助你快速理解 Skills、Hooks 等概念：

`       1          /powerup            `

系统会启动一个交互式引导教程，通过实际操作带你学习 Skills 的创建和使用。对于刚接触 Claude Code 扩展系统的用户，这是一个零压力的入门方式。

### 13.2 文件处理类 Skill（非程序员友好）

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

#### docx、pptx、xlsx 系列（内置）

这些是 Claude Code 自带的内置 Skills，不需要额外安装。

**xlsx Skill 实战**：

`       1  2  3  4  5  6          帮我把这个月的销售数据（sales-2026-05.csv）生成一个 Excel 报表。   要求：   1. 按产品分类汇总销售额和销量   2. 添加一个图表展示月度趋势   3. 第一行冻结（方便滚动查看）   4. 自动调整列宽            `

Claude Code 会自动调用 xlsx Skill，生成一个格式化的 Excel 文件。你不需要知道 `openpyxl` 怎么用，不需要查图表 API 的参数——描述需求即可。

**docx Skill 实战**：

`       1  2  3  4  5          把 folder/ 文件夹下所有周报（.md 文件）合并成一份 Word 文档。   要求：   - 按时间倒序排列   - 自动生成目录   - 页眉添加公司 Logo            `

#### PDF Skill

**安装**：

`       1          /plugin install pdf            `

**典型场景——批量处理合同**：

`       1  2  3  4  5          把这些合同模板（template.docx）中的占位符替换为客户信息：   - {{客户名称}} → 张三   - {{合同金额}} → 50000   - {{签订日期}} → 2026-05-20   替换后生成 PDF 文件。            `

### 13.3 编程辅助类 Skill（程序员必备）

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

#### Code Reviewer Skill

安装后在项目中这样使用：

`       1          /review            `

或者更精确的：

`       1  2  3  4          /review 审查这次 commit 的改动，重点关注：   1. 是否有 SQL 注入风险   2. 错误处理是否完整   3. 是否有性能问题（N+1 查询等）            `

**作者推荐配置**：结合项目的 ESLint 和 Prettier 规则，在 CLAUDE.md 中说明代码风格偏好，这样 Code Reviewer Skill 审查时会保持一致的标准。

#### Test Generator Skill

`       1  2  3  4  5          为 src/services/user.service.ts 中的所有公开函数生成单元测试。   要求：   - 使用 Jest 框架   - 覆盖正常情况和边界条件（空参数、null 值、异常情况）   - 测试文件放在 __tests__/user.service.test.ts            `

对遗留代码项目尤其有用——原来零测试的代码库，可以快速补全测试用例。

#### API Doc Generator Skill

`       1  2          根据 src/routes/ 目录下的所有路由文件，生成 OpenAPI 3.0 规范的 API 文档。   保存为 api-docs.yaml。            `

#### Database Skill（需要配合 Postgres MCP）

配置好 Postgres MCP 后，你可以用自然语言操作数据库：

`       1          查询 users 表中有多少注册超过 30 天但未完成实名认证的用户。            `

`       1          帮我在 orders 表和 order_items 表之间创建一个关联查询的视图。            `

这对数据分析师和产品经理尤其有用——不需要写 SQL，用自然语言就能查询数据库。

### 13.4 日常工作流 Skill（跨岗位通用）

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

#### Weekly Report Skill

`       1          生成周报            `

Claude Code 会自动：

1.  读取本周的 Git 提交记录
    
2.  汇总文档修改
    
3.  生成结构化周报
    

#### Meeting Minutes Skill

`       1  2  3          把这段会议录音转文字整理成会议纪要：   [粘贴文字记录或指定音频文件]   会议纪要要求：参与人、讨论议题、决策结果、待办事项及责任人。            `

#### Research Assistant Skill（需配合搜索 MCP）

`       1  2  3  4  5          调研一下 2026 年国内 AI Agent 开发框架的现状。   要求：   - 至少覆盖 5 个主流框架   - 按功能、社区活跃度、学习成本三个维度对比   - 输出调研报告，带引用来源            `

#### Competitor Analysis Skill

`       1  2          分析竞争对手 X、Y、Z 的最新动态。   按以下维度整理：产品功能、定价策略、市场定位、融资情况。            `

### 13.5 作者私藏 Skill 组合与使用心法

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

#### “三件套”工作流

这是作者日常使用频率最高的三个 Skill 组合：

1.  早晨： 启动 Claude Code → **Daily Plan Skill** 规划当天任务
    
    `       1          根据昨天的工作进展和今天的优先级，帮我规划今天的工作安排            `
    
2.  编码时： **Code Reviewer Skill** 实时检查每次改动
    
    `       1          /review 检查刚才的修改            `
    
3.  下班前： **Weekly Report Skill** 生成日报
    
    `       1          生成本周周报            `
    

#### 改造别人的 Skill 为己用

核心思路：Skill 就是 Markdown 文件，改了就能定制。

例如，你安装了别人的 Weekly Report Skill，但报告模板不太符合你的团队风格。直接编辑 `SKILL.md`：

1.  找到输出模板部分
    
2.  换成你团队的周报格式
    
3.  保存
    

下次使用这个 Skill 时，就会按你的模板输出了。

#### 面向非程序员的“一键式” Skill 推荐

如果你不是程序员，装这 5 个 Skill 就能覆盖 90% 的办公场景：

Skill

用途

xlsx（内置）

Excel 报表自动生成

docx（内置）

Word 文档处理

weekly-report

周报/日报自动生成

meeting-minutes

会议纪要整理

research-assistant

调研报告自动生成
