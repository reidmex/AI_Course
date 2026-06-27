---
title: "Claude Code零基础小白教程第10章：Hooks（钩子）与自动化工作流详解"
url: https://mp.weixin.qq.com/s?__biz=MjM5NzQ0NzIyOQ==&mid=2448131882&idx=1&sn=e409bedcd9837f67f657afbb1c5751bc&chksm=b2c6f47885b17d6e2b60cd74783393a87fcf1455a194cdee8193f8a499f59b2ee2e8d4a8da80&cur_album_id=4566215426073788416&scene=190#rd
byline: "萝卜啊"
saved: 2026-06-27T09:08:12.590Z
---
# Claude Code零基础小白教程第10章：Hooks（钩子）与自动化工作流详解

> **第三阶段**：进阶精通——打造你的 AI 工作流
> 
> **本阶段目标**：掌握 CLAUDE.md、Skills、MCP、Hooks 四大进阶能力，让 Claude Code 从“能用”升级为“懂你”的专属 AI 工作伙伴。本阶段共四章节，第7章节到第10章节

### 10.1 Hooks 是什么：在关键时刻自动触发的“守门员”

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/MiaEMf64YmohJbWJ1ic8cmrAvl3eUmFotOplwf6p7eZzZ3azOxdPdVvYnKG3WrV9swdr2ibMGHKSWM4X8p31ic882vPLxyq1iarklXRibLIGsnzwo/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=0)

Hooks（钩子）是一种“事件触发器”——当某个特定事件发生时，自动执行你预先定义的操作。

Git 用户应该很熟悉这个概念：Git Hooks 在 commit 前、push 前自动执行检查脚本。Claude Code 的 Hooks 也是同样的道理，只是触发的事件不同。

#### Hooks 的生命周期位置

Claude Code 的 Hooks 可以在以下时机触发：

-   PreToolUse： 在 Claude Code 调用某个工具之前触发（如执行命令前、写文件前）
-   PostToolUse： 在 Claude Code 调用某个工具之后触发（如命令执行完成后、文件写入完成后）
-   Notification： 特定事件通知（如会话开始、会话结束）
-   UserPromptSubmit： 在用户提交输入时触发
-   Stop： 在 Claude Code 停止响应时触发

#### 两类 Hooks 的定位

类型

定位

典型场景

PreToolUse

守门员——在行动前拦截和检查

禁止危险命令、强制代码格式化

PostToolUse

质检员——在行动后补充和验证

自动运行测试、更新文档

### 10.2 实战 Hooks 配置

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/MiaEMf64YmoiamvYn942YfG7E3cP8ScviaCLm6On6z83t29pG8d0iaKMHSJYhF2yAwDUj3htp7xDdR7ZKtU5ciayM356eDibYqGNVYWvQ3PaNcwW0/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=1)

Hooks 配置在 `.claude/settings.json`（项目级）或 `~/.claude/settings.json`（用户级）中。

#### 实战 Hook 1：代码编辑后自动格式化

每次 Claude Code 写完代码后，自动运行 Prettier 格式化，保持代码整洁。

`       1  2  3  4  5  6  7  8  9  10  11  12  13  14  15          {     "hooks": {       "PostToolUse": [         {           "matcher": "Edit|Write",           "hooks": [             {               "type": "command",               "command": "npx prettier --write ${CLAUDE_TOOL_INPUT_FILE_PATH} 2>/dev/null || true"             }           ]         }       ]     }   }            `

**解读**：

-   matcher: "Edit|Write"： 匹配“编辑文件”和“写入文件”两种工具操作
-   command： 格式化命令——`|| true` 确保即使格式化失败也不会阻断 Claude Code 的后续操作
-   效果：Claude Code 每改完一个文件，自动帮你格式化
    

#### 实战 Hook 2：禁止危险命令

这是安全守门员——防止 Claude Code（或恶意输入）执行危险操作。

`       1  2  3  4  5  6  7  8  9  10  11  12  13  14  15          {     "hooks": {       "PreToolUse": [         {           "matcher": "Bash",           "hooks": [             {               "type": "command",               "command": "if echo '${CLAUDE_TOOL_INPUT}' | grep -qE 'rm -rf /|sudo rm|git push --force'; then echo 'BLOCKED: Dangerous command detected' >&2 && exit 1; fi"             }           ]         }       ]     }   }            `

**效果**：当 Claude Code 尝试执行包含 `rm -rf /`、`sudo rm`、`git push --force` 等危险命令时，Hook 会拦截并阻止执行，输出警告。

**你可以扩展这个列表**，加入你不想让 Claude Code 执行的任何命令。

#### 实战 Hook 3：Git 提交前自动跑测试

每一次 Git 提交前，自动运行测试——测试不通过，提交不成立。

`       1  2  3  4  5  6  7  8  9  10  11  12  13  14  15          {     "hooks": {       "PreToolUse": [         {           "matcher": "Bash",           "hooks": [             {               "type": "command",               "command": "if echo '${CLAUDE_TOOL_INPUT}' | grep -q 'git commit'; then npm test || (echo 'Tests failed, commit blocked' >&2 && exit 1); fi"             }           ]         }       ]     }   }            `

**效果**：Claude Code 执行 `git commit` 之前，先跑测试。测试通过 → 继续提交；测试失败 → 阻止提交，输出失败信息。

> **与 Git Hooks 的区别**：Git 自带的 pre-commit hooks 也能实现类似效果，但 Claude Code 的 Hooks 更灵活——你可以注入任何逻辑，比如“如果修改了 API 文件，自动检查是否更新了文档”。

### 10.3 Hooks 的进阶编排

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/MiaEMf64Ymojfj5jZYFugKQ9JVicia7Dr3ITugWv7Y2nNw70HUffvYjKLkhQnRNymibdxAJaFiceLMnBk7F1FOHXbzPWTlXjl0uCPBcDNw3x77DY/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=2)

#### 多个 Hooks 的触发顺序

你可以在同一个事件上注册多个 Hooks。例如，在 PostToolUse 上同时注册“格式化”和“检查拼写”两个 Hook。它们的执行顺序是配置文件中定义的顺序。

**建议顺序**：先做安全检查（PreToolUse），再做质量检查（PostToolUse）。

#### 结合 Skills 打造完整自动化流水线

Hooks + Skills 可以实现“零操作”的自动化流水线：

1.  Claude Code 写代码（正常 Agent Loop）
    
2.  PostToolUse Hook 自动格式化（Prettier）
    
3.  PreToolUse Hook 在 git commit 前自动跑测试
    
4.  PostToolUse Hook 在测试通过后自动更新覆盖率报告
    
5.  Stop Hook 在会话结束时自动生成会话摘要
    

配置完成后，你不需要记得执行这些步骤——Hooks 会在正确的时机自动触发。

#### 注意事项：Hooks 执行失败时

如果 Hook 返回了非零退出码，Claude Code 的默认行为是停止当前操作并报告失败。这意味着：

-   合理的失败（如“测试没通过，阻止提交”）是**预期行为**
    
-   不合理的失败（如“格式化工具报错了，导致 Claude Code 无法继续”）可能需要调整 Hook 命令，加上 `|| true` 容错
    

**设计 Hook 的原则**：

-   PreToolUse 的 Hook 失败 → 应该阻断（安全考虑）
    
-   PostToolUse 的 Hook 失败 → 应该容错（不影响主流程）
    

* * *

**第三阶段到此结束。**

你已经完成了 CLAUDE.md、Skills、MCP、Hooks 四大进阶技能的学习。这些是你打造个性化 AI 工作流的基石。从现在开始，你的 Claude Code 不再是“通用版”，而是为你的项目和团队量身定制的专属助手。

下一阶段，我们将进入**效率与技巧**——学习如何用更少的 Token 做更多的事，以及社区中最值得安装的精选 Skill 和生态工具。
