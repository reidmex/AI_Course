---
title: "Claude Code零基础小白教程第2章：Claude Code 安装：三种方式任你选"
url: https://mp.weixin.qq.com/s?__biz=MjM5NzQ0NzIyOQ==&mid=2448131802&idx=1&sn=15371be29148cc7ace2a2b41abcd9a48&chksm=b2c6f58885b17c9ec492cdafe2256bb31ef058836d25ff9de3cf594ad1967a5c0979cc2e30da&cur_album_id=4566215426073788416&scene=189#wechat_redirect
byline: "萝卜啊"
saved: 2026-06-26T09:54:35.615Z
---
# Claude Code零基础小白教程第2章：Claude Code 安装：三种方式任你选

> **第一阶段包含第一章至第四章**：主要任务是快速上手——从安装到跑通第一个任务
> 
> **本阶段目标**：完成环境搭建、Claude Code 安装、国产模型配置，并成功执行你的第一个任务。预计用时 1-2 小时。

Claude Code 的安装本身并不复杂——本质上就是一个 npm 包。但在国内网络环境下，有几个细节需要注意。本章提供三种安装方式，覆盖 macOS、Windows、Linux 全平台。

### 2.1 npm 官方安装（最推荐，最稳定）

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/MiaEMf64Ymohr4zQyT7Zz78QAxO9ogxCKVaYV2icoM5uxxWQX2Ps9Kz5Z8pTxF0rCtn5LUlicMA2pUicz0COZZBvicasaVYY2uLLgQH1Yoia4kdYM/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=0)

这是最正统的安装方式，适用于所有平台，也是后续升级最方便的途径。

#### 基本安装命令
```
npm install -g @anthropic-ai/claude-code
```

`-g` 参数表示全局安装（global），安装完成后 `claude` 命令在整个系统中都可以使用。

#### 国内镜像加速

由于 npm 官方源在国内访问速度不稳定，强烈建议使用国内镜像：
```
npm install -g @anthropic-ai/claude-code --registry=https://registry.npmmirror.com
```

`npmmirror.com` 是淘宝 NPM 镜像的新域名，同步频率高、速度快。如果这个源不稳定，也可以换腾讯云镜像：
```
npm install -g @anthropic-ai/claude-code --registry=https://mirrors.cloud.tencent.com/npm/
```

#### macOS 权限问题

如果你在 macOS 上遇到 `EACCES` 权限错误（很常见，因为 npm 的全局目录需要管理员权限），有两种解决方案：

**方案一：直接加 `sudo`（简单直接）**
```
sudo npm install -g @anthropic-ai/claude-code --registry=https://registry.npmmirror.com
```

系统会提示你输入开机密码，输入时屏幕不显示任何字符是正常的，输完回车即可。

**方案二：用 nvm 管理 Node.js（一劳永逸）**

如果你用 nvm 安装的 Node.js，全局安装不需要 `sudo`，因为所有文件都在你的用户目录下。没有使用 nvm 的话，现在改还来得及，但工程量较大，建议先用方案一。

#### 验证安装

安装完成后，验证：
```
claude --version   # 应输出类似: 2.1.139 (Claude Code v2.1.x)
```

如果提示 `claude: command not found`，说明全局安装的路径没有被加入到 PATH 环境变量中。这是 Windows 用户的高频问题，详见下文 2.3 节。

#### 升级 Claude Code

npm 全局安装的包，升级和安装用的是同一个命令：
```
npm install -g @anthropic-ai/claude-code --registry=https://registry.npmmirror.com
```

或者用 npm 的 update 命令：
```
npm update -g @anthropic-ai/claude-code
```

建议每月检查一次更新，Claude Code 的功能迭代非常快。

### 2.2 macOS Homebrew 安装

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/MiaEMf64Ymohgj9buWQ1RCxtP8NvWs87EMuomDOJ0euY4XztOJodouCTSh3nwTUhEprINCZSfHDUK2OyVTKFbpOPkzTsPuPUPiaARsh75Q1j4/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=1)

如果你使用 Mac 且已经装了 Homebrew，这是最省心的方式——不需要 npm，也不需要处理权限问题。
```
brew install --cask claude-code
```

`--cask` 参数表示安装的是一个独立的应用程序（而非命令行工具），但安装完成后 `claude` 命令同样可以在终端中使用。

**优点**：

-   不需要科学上网，Homebrew 在国内通常可以直接访问
    
-   自动处理依赖，安装完成即可用
    
-   升级方便：`brew upgrade --cask claude-code`
    

**注意**：Homebrew Cask 版本可能比 npm 版本晚几小时到一天，如果需要第一时间用最新功能，npm 安装更快。

### 2.3 Windows 专用安装指南

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/MiaEMf64YmojjPEhjPuRiabFN1NQp6ic9xfsz0UFcu1pYjoiazKFW9oYEiaCT6xGu3zJMgPDH4ibfzwa1ibJqas5T0ibfep1ooqTMJt3y7FafqGzaz0/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=2)

Windows 用户需要特别注意两个前置条件：

1.  必须先安装 Git（第 1 章已覆盖）
2.  所有操作在 Git Bash 中执行（不要用 CMD 或 PowerShell）

满足这两个条件后，npm 安装命令和 macOS/Linux 完全一样：
```
npm install -g @anthropic-ai/claude-code --registry=https://registry.npmmirror.com
```

#### 替代方案：WinGet 安装

如果你的系统支持 WinGet（Windows 11 自带，Windows 10 需手动安装），也可以：
```
winget install Anthropic.ClaudeCode
```

但注意，WinGet 安装也需要 Git 环境，且安装完成后首次启动可能需要在 Git Bash 中额外配置 PATH。

#### Windows PATH 配置详解

如果在 Git Bash 中执行 `claude --version` 提示 `command not found`，说明 npm 的全局安装目录没有被加入 PATH。

**排查步骤**：

1.  先在 Git Bash 中查看 npm 的全局安装路径：
    ```
    npm config get prefix
    ```
    输出类似 `/c/Users/你的用户名/AppData/Roaming/npm`
    
2.  确认 `claude` 是否在这个路径下：
    ```
    ls "$(npm config get prefix)/claude"
    # 或
    ls "$(npm config get prefix)/claude.cmd"
    ```
    如果文件存在但命令不识别，说明 PATH 没配。
    
3.  将 npm 全局路径加入 PATH：
    
    打开 Git Bash，编辑 `~/.bashrc` 文件（如果没有就创建一个）：
    ```
    echo 'export PATH="(npm config get prefix)"' >> ~/.bashrc
    source ~/.bashrc
    ```
    
    然后重新打开 Git Bash，再次验证 `claude --version`。
    

如果以上步骤仍无法解决，参考附录 B 的 Windows 专属排错章节。

### 2.4 一键安装的“捷径”工具

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/MiaEMf64Ymojs4k8ROMZqMKgGOxnOajibTBial3ZjIq0hCIfpbeUsmTaeFZ5Oy9l1RThgY97k5xpuXw9l7dsibibGIvzHSq5lSlibKrl4CjBD4AK8/640?wx_fmt=jpeg&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=3)

社区开发者针对国内用户的需求，开发了一些图形化或一键式安装工具。这些工具不是必须的，但对于不想折腾命令行的用户来说可以省不少事。

#### CC Switch

CC Switch 是一个开源的 Claude Code 配置切换工具，**图形化界面**，支持一键安装 Claude Code 并预设国产模型的配置。

-   项目地址：github.com/cc-switch（请以实际最新地址为准）
    
-   支持 macOS 和 Windows
    
-   内置智谱 GLM、阿里百炼、DeepSeek 等国产模型的预设配置
    
-   可视化切换模型，无需手动编辑环境变量
    

**macOS 安装**：

`       1  2          brew tap cc-switch/tap   brew install cc-switch            `

安装完成后，在应用程序中找到 CC Switch，按界面提示操作即可。

#### cc-switch CLI 工具

如果你偏好命令行，也有一个同名的轻量级 CLI 工具：

`       1          npm install -g cc-switch            `

使用方式：

`       1  2  3          cc-switch init     # 初始化配置向导   cc-switch list     # 查看可用模型列表   cc-switch use glm  # 切换到智谱 GLM            `

**这些工具的价值**：它们帮你省去了手动编辑环境变量的步骤，降低了配置出错的概率。但本教程后续章节仍会详细讲解手动配置的方法——因为理解底层原理，才能在出问题时快速定位和修复。

### 本章小结

到这一步，你的电脑上应该已经装好了 Claude Code。验证一下：

`       1          claude --version            `

如果能看到版本号（如 `2.1.139`），恭喜你，安装成功。

先别急着运行 `claude`——你还需要给它“接上脑子”（配置模型）。这是下一章的内容，也是整本教程最关键的一章。
