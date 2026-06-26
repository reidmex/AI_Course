---
title: "Claude Code零基础小白教程第7章：CLAUDE.md详解，给你的 AI 写一份“员工手册”"
url: https://mp.weixin.qq.com/s?__biz=MjM5NzQ0NzIyOQ==&mid=2448131852&idx=1&sn=82fec9cccc33fb8c587b91462db93cc8&chksm=b2c6f45e85b17d482c6d2f1cfea6bed8c8c2257290ade63a3407d0b41cee586579f8839c0a8c&cur_album_id=4566215426073788416&scene=189#wechat_redirect
byline: "萝卜啊"
saved: 2026-06-26T09:47:38.945Z
---
# Claude Code零基础小白教程第7章：CLAUDE.md详解，给你的 AI 写一份“员工手册”

> **第三阶段**：进阶精通——打造你的 AI 工作流
> 
> **本阶段目标**：掌握 CLAUDE.md、Skills、MCP、Hooks 四大进阶能力，让 Claude Code 从“能用”升级为“懂你”的专属 AI 工作伙伴。本阶段共四章节，第7章节到第10章节

### 7.1 CLAUDE.md 是什么，为什么它是 Claude Code 的“灵魂文件”

![01-section](https://mmbiz.qpic.cn/sz_mmbiz_jpg/MiaEMf64Ymoh4w8zuLSfrHzqt2sErCYa3xFvkFV1hFqCaTp5eiayG8WAocktX9tmmN0tV20kPEcffKWrFMSumgT8O6siao8VmWEFb2YnbY8VGA/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=0)

01-section

想象你新招了一个助理。如果什么都不交代，直接让他干活，他只能凭通用经验行事，做出来的结果大概率不符合你的习惯。但如果你花十分钟写了一份简单的操作手册——告诉他你的偏好、项目的约定、常用的工具和路径——他的工作效率和准确度会立刻提升几个档次。

CLAUDE.md 就是 Claude Code 的“员工手册”。

它是一个放在项目根目录（或你的主目录）下的 Markdown 文件。Claude Code 每次启动时，会第一时间读取这个文件，将里面的内容作为本次会话的基础上下文。这意味着：

-   你不需要每次都解释“这个项目是做什么的”
    
-   你不需要反复强调“请用中文回复”或“测试命令是 npm test”
    
-   Claude Code 会按照你定义的流程来做事，而不是用通用的方式
    

**有 CLAUDE.md 和没有 CLAUDE.md 的区别**，可以用一个场景来说明。

**没有 CLAUDE.md**：你告诉 Claude Code “帮我重构这个项目的用户模块”。Claude Code 开始探索项目结构，试图理解技术栈、代码风格、测试框架……这个过程可能消耗数分钟的 Token 和时间，而且它推断的约定可能不完全准确。

**有 CLAUDE.md**：Claude Code 在启动时已经知道：

-   这是一个 Express + TypeScript 项目
    
-   测试命令是 `npm test`，代码规范用 ESLint + Prettier
    
-   用户模块在 `src/modules/user/`，数据库操作通过 Prisma
    
-   所有 API 接口需要遵循 `src/shared/response.ts` 中的统一返回格式
    

然后它直接开始干活，不需要任何探索步骤。

**这不是锦上添花，这是 Claude Code 从“初级外包”升级为“资深团队成员”的关键**。

### 7.2 CLAUDE.md 的黄金模板

![02-section](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

02-section

一个高质量的 CLAUDE.md 应该包含以下核心信息。你可以直接复制下面的模板，填入你自己的项目信息。

#### 面向程序员的模板

``       1  2  3  4  5  6  7  8  9  10  11  12  13  14  15  16  17  18  19  20  21  22  23  24  25  26  27  28  29  30  31  32  33  34  35  36  37  38  39  40  41  42  43  44  45  46  47          # CLAUDE.md       ## 项目概述               这是一个 [项目一句话描述]，主要功能包括 [核心功能列表]。   技术栈：[语言/框架]，数据库：[数据库类型]       ## 构建和测试命令       - 安装依赖：`npm install`   - 启动开发服务器：`npm run dev`   - 运行测试：`npm test`   - 运行单个测试文件：`npm test -- [文件名]`   - 代码检查：`npm run lint`   - 构建生产版本：`npm run build`       ## 代码风格约定       - 使用 2 空格缩进   - 变量命名：[camelCase / snake_case]   - 所有公开函数必须写 JSDoc 注释   - API 接口返回格式统一使用 `{ code: 0, data: {}, message: '' }`   - 错误处理：Service 层抛出自定义错误，Controller 层统一捕获       ## 项目结构   - `src/`：源代码     - `src/routes/`：路由定义     - `src/controllers/`：控制器     - `src/services/`：业务逻辑     - `src/models/`：数据模型     - `src/middleware/`：中间件     - `src/utils/`：工具函数   - `tests/`：测试文件，按模块分目录   - `docs/`：文档   - `scripts/`：脚本       ## 开发约定   - 新功能开发前先创建 feature 分支   - 提交格式：`type(scope): description`（如 `feat(user): add login api`）   - PR 合并前必须通过 CI 测试和至少一人审查       ## 注意事项   - 不要直接修改 `dist/` 目录下的文件   - 数据库迁移文件在 `migrations/` 中，不要手动修改   - 敏感信息通过环境变量配置，不要硬编码            ``

#### 面向非程序员的模板

如果你不是程序员，CLAUDE.md 可以更侧重于任务描述和操作习惯：

``       1  2  3  4  5  6  7  8  9  10  11  12  13  14  15  16  17  18  19  20  21  22  23  24          # CLAUDE.md       ## 我的工作说明   我是一个 [岗位]，日常主要和 [文件类型/数据/文档] 打交道。   我使用 Claude Code 来帮我处理 [主要任务类型]。       ## 常用任务   - 数据分析：读取 CSV/Excel 文件，计算统计指标，生成图表   - 文档汇总：读取多个 Markdown 文件，汇总成结构化报告   - 文件整理：按规则批量重命名、分类文件       ## 我的偏好   - 回复语言：中文   - 输出风格：直接、简洁，不要过多的解释性文字   - 文件操作前请先确认，不要在未告知的情况下删除文件   - 生成图表优先使用 Python + Matplotlib，颜色方案偏好蓝白系       ## 数据文件说明   - 销售数据存放在 `~/Documents/sales/`，文件名格式为 `YYYY-MM_销售报表.csv`   - 周报存放在 `~/Documents/weekly-reports/`，文件名格式为 `周报-姓名-第N周.md`       ## 注意事项   - 不要修改 `archive/` 文件夹中的任何文件   - 处理大量文件时请分步进行，避免一次性操作            ``

**关键原则**：CLAUDE.md 不是越详细越好，而是要精准覆盖 Claude Code 在执行任务时“最需要知道但无法从代码中自动推断”的信息。

### 7.3 CLAUDE.md 的进阶用法

![03-section](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

03-section

#### 工作流嵌入

在 CLAUDE.md 中，你可以定义 Claude Code 完成特定类型任务后需要执行的检查：

``       1  2  3  4  5  6          ## 代码修改后的检查清单   每次修改代码后，请自动执行以下检查：   1. 运行 `npm test` 确保所有测试通过   2. 运行 `npm run lint` 确保代码风格符合规范   3. 如果修改了 API 接口，更新对应的接口文档   4. 如果修改了数据模型，检查是否需要创建数据库迁移            ``

这相当于给你的 AI 设定了一个“交付标准”——每次完成工作后，它会自己对照清单检查，确保没有遗漏。

#### 安全边界

如果你的项目中有敏感数据或不想让 AI 碰的区域，在 CLAUDE.md 中明确禁止：

``       1  2  3  4  5          ## 安全边界   - 绝对不要执行以下命令：`rm -rf /`、`DROP TABLE`、`git push --force`   - 不要读取或修改 `.env` 文件   - 不要访问 `~/.ssh/` 目录   - 涉及网络请求时，不要向外部服务发送任何 `API_KEY` 或敏感凭证            ``

#### 团队共享

如果你在一个团队中使用 Claude Code，将 CLAUDE.md 纳入版本管理（Git），这样每个团队成员启动 Claude Code 时都能获得相同的上下文。这也意味着：

-   新成员入职时，CLAUDE.md 就是 AI 的项目文档
    
-   当项目约定发生变化时，更新 CLAUDE.md 并提交，所有人的 AI 都会同步更新
    
-   结合 Git 的 blame 功能，可以追溯“谁给 AI 定了这个规则”
    

> **团队协作的高级用法**：可以在 CLAUDE.md 中指定多个模型的不同用途，例如“日常开发用 GLM，代码审查用 Opus”，Claude Code 会在相应场景下提示你切换模型。详见第 12 章的混合模型策略。
