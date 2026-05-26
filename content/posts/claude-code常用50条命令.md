---
title: "claude-code常用50条命令"
date: 2026-05-24T16:50:00+08:00
draft: false
tags: ["claude-code"]
categories: ["博客"]
---
# Claude Code 斜杠命令分类指南

> 本文基于当前目录中的 `claude源码/src/commands.ts` 和 `claude源码/src/commands/*` 梳理。不同 Claude Code 版本、账号权限、实验开关、内部构建和插件安装状态会影响实际可见命令；最终以 `/help` 显示为准。

## 阅读方式

Claude Code 的斜杠命令大致分三类：

- **本地 UI 命令**：打开设置、选择器、状态面板，不一定会请求模型。
- **本地执行命令**：直接在当前会话里执行清理、导出、统计等动作。
- **Prompt 命令**：把一段预设任务提示交给模型执行，例如代码审查、安全审查、提交等。

建议先掌握高频命令：

```text
/help
/status
/context
/compact
/diff
/plan
/review
/mcp
/permissions
/resume
```

---

## 一、会话与上下文管理

这类命令负责管理当前对话、上下文、分支、导出和恢复。

| 命令 | 别名 | 参数 | 作用 |
|---|---|---|---|
| `/clear` | `/reset`, `/new` | 无 | 清空当前会话历史，释放上下文 |
| `/compact` | 无 | `[压缩说明]` | 压缩历史对话，保留摘要继续工作 |
| `/resume` | `/continue` | `[会话 ID 或搜索词]` | 恢复历史会话 |
| `/branch` | `/fork`* | `[name]` | 从当前节点创建会话分支 |
| `/rewind` | `/checkpoint` | 无 | 回退到之前的检查点 |
| `/rename` | 无 | `[name]` | 重命名当前会话 |
| `/tag` | 无 | `<tag-name>` | 给当前会话切换一个可搜索标签，部分构建可见 |
| `/btw` | 无 | `<question>` | 提一个临时问题，不打断主线任务 |
| `/copy` | 无 | `[N]` | 复制 Claude 最近一次或第 N 条回复 |
| `/export` | 无 | `[filename]` | 导出当前对话到剪贴板或文件 |
| `/exit` | `/quit` | 无 | 退出 REPL |

`/fork` 在部分构建中会作为 `/branch` 的别名出现；如果启用了独立 fork 子代理功能，可能会被单独命令接管。

### 示例

```text
/clear
/compact
/compact 保留数据库 schema、API 参数和未完成事项，其他内容简化
/resume
/resume auth refactor
/resume 018f4f0e-xxxx-xxxx-xxxx-xxxxxxxxxxxx
/branch redis-cache-experiment
/rewind
/rename 支付模块重构
/tag release-2026-05
/btw TypeScript satisfies 和 as const 有什么区别？
/copy
/copy 2
/export
/export 2026-05-26-重构记录.md
/exit
```

### 使用建议

- 上下文变长时优先用 `/compact`，不是 `/clear`。
- 开始一个风险较高的探索前，可以先 `/branch`。
- 会话多时先 `/rename`，以后 `/resume` 更容易找到。

---

## 二、状态、诊断与统计

这类命令用来查看当前环境、上下文占用、费用、版本、文件和安装状态。

| 命令 | 参数 | 作用 |
|---|---|---|
| `/help` | 无 | 查看当前可用命令 |
| `/status` | 无 | 查看版本、模型、账号、API 和工具状态 |
| `/doctor` | 无 | 诊断 Claude Code 安装与配置 |
| `/context` | 无 | 可视化当前上下文使用情况 |
| `/diff` | 无 | 查看未提交变更和按轮次拆分的改动 |
| `/files` | 无 | 列出当前上下文中的文件，部分构建可见 |
| `/usage` | 无 | 查看套餐用量限制 |
| `/cost` | 无 | 查看当前会话总费用与耗时 |
| `/stats` | 无 | 查看使用统计和活动数据 |
| `/insights` | 无 | 基于历史会话生成使用分析报告 |
| `/release-notes` | 无 | 查看版本发布说明 |
| `/version` | 无 | 输出当前会话运行的版本，部分构建可见 |
| `/heapdump` | 无 | 导出 JS heap 到桌面，偏调试用途 |

### 示例

```text
/help
/status
/doctor
/context
/diff
/files
/usage
/cost
/stats
/insights
/release-notes
/version
/heapdump
```

### 使用建议

- 提交前至少跑一次 `/diff`，确认 Claude 改动范围符合预期。
- 遇到认证、IDE、MCP、模型或网络相关异常，先看 `/status` 和 `/doctor`。
- `/context` 适合和 `/compact` 配合使用：先看占用，再决定是否压缩。

---

## 三、模型、模式与界面偏好

这类命令控制模型选择、推理力度、计划模式、快速模式以及终端界面偏好。

| 命令 | 参数 | 作用 |
|---|---|---|
| `/model` | `[model]` | 选择或切换模型 |
| `/effort` | `[low\|medium\|high\|max\|auto]` | 设置模型思考力度 |
| `/fast` | `[on\|off]` | 开关快速模式 |
| `/plan` | `[open\|<description>]` | 进入计划模式或查看当前计划 |
| `/advisor` | `[<model>\|off]` | 配置 advisor 模型，部分账号可见 |
| `/brief` | 无 | 开关 brief-only 模式，特性开关可见 |
| `/vim` | 无 | 在 Vim/普通编辑模式之间切换 |
| `/theme` | 无 | 切换主题 |
| `/color` | `<color\|default>` | 设置当前会话提示栏颜色 |
| `/output-style` | 无 | 已废弃，改用 `/config` 设置输出风格 |
| `/statusline` | 无 | 配置状态栏 UI |

### 示例

```text
/model
/model sonnet
/model opus
/effort auto
/effort high
/effort max
/fast on
/fast off
/plan
/plan open
/plan 重构登录模块，先给出风险、步骤和验证方式
/advisor off
/advisor sonnet
/brief
/vim
/theme
/color green
/color default
/statusline
```

### 使用建议

- 大改动先 `/plan`，确认方案后再让 Claude 修改代码。
- 简单任务可以降低 `/effort`；复杂设计、重构、排障可提高。
- `/fast` 更适合短平快任务；长时间复杂任务应留意成本和效果。

---

## 四、配置、权限与扩展

这类命令负责工作目录、权限、MCP、Memory、Hooks、Skills 和 Plugins。

| 命令 | 别名 | 参数 | 作用 |
|---|---|---|---|
| `/config` | `/settings` | 无 | 打开配置面板 |
| `/permissions` | `/allowed-tools` | 无 | 管理工具允许/拒绝规则 |
| `/sandbox` | 无 | `exclude "command pattern"` | 配置沙箱排除规则 |
| `/add-dir` | 无 | `<path>` | 增加 Claude 可访问的工作目录 |
| `/mcp` | 无 | `[enable\|disable [server-name]]` | 管理 MCP 服务器 |
| `/memory` | 无 | 无 | 编辑 Claude memory 文件 |
| `/hooks` | 无 | 无 | 查看工具事件 hook 配置 |
| `/skills` | 无 | 无 | 查看可用 skills |
| `/plugin` | `/plugins`, `/marketplace` | 无 | 管理 Claude Code 插件 |
| `/reload-plugins` | 无 | 无 | 激活当前会话里的插件变更 |
| `/keybindings` | 无 | 无 | 打开或创建快捷键配置 |
| `/privacy-settings` | 无 | 无 | 查看与更新隐私设置 |

### 示例

```text
/config
/permissions
/sandbox exclude "npm run dev"
/add-dir ../shared-lib
/add-dir /Users/me/work/company-sdk
/mcp
/mcp enable
/mcp disable github
/memory
/hooks
/skills
/plugin
/reload-plugins
/keybindings
/privacy-settings
```

### 使用建议

- 需要 Claude 读写当前项目外的目录时，用 `/add-dir`，不要直接让它绕过权限。
- MCP 连接异常时，先 `/mcp` 查看状态，再决定启用或禁用具体 server。
- 插件或 skill 刚安装后，如果当前会话没有生效，可以尝试 `/reload-plugins`。

---

## 五、代码审查、提交与协作

这类命令将常见工程任务封装为命令，部分命令只在内部构建、特定账号或特性开关中可见。

| 命令 | 参数 | 作用 | 可见性 |
|---|---|---|---|
| `/review` | `[PR/分支说明]` | 审查 Pull Request 或当前分支变更 | 常规 |
| `/ultrareview` | `[PR/分支说明]` | 更深度的云端审查，耗时更长 | 账号/特性相关 |
| `/security-review` | 无 | 对当前分支 pending changes 做安全审查 | 常规/构建相关 |
| `/commit` | 无 | 根据当前 git 变更创建一个 commit | 内部构建可见 |
| `/commit-push-pr` | `[说明]` | 提交、推送并创建 PR | 内部构建可见 |
| `/pr-comments` | 无 | 获取 GitHub PR 评论 | 构建/环境相关 |
| `/agents` | 无 | 管理 agent 配置 | 常规 |
| `/tasks` | 无 | 查看和管理后台任务 | 常规 |

### 示例

```text
/review
/review 42
/review 重点检查错误处理、并发安全和边界条件
/ultrareview
/security-review
/commit
/commit-push-pr
/commit-push-pr 增加支付回调验签并补充测试
/pr-comments
/agents
/tasks
```

### 使用建议

- `/review` 适合提交前找 bug、逻辑漏洞和遗漏测试。
- `/security-review` 只看安全风险，不等同于通用 code review。
- `/commit` 这类 prompt 命令会分析当前改动；提交前仍建议先 `/diff`。

---

## 六、账号、安装与客户端集成

这类命令和登录、安装、IDE、移动端、桌面端、GitHub/Slack 集成有关。

| 命令 | 别名 | 参数 | 作用 |
|---|---|---|---|
| `/login` | 无 | 无 | 登录 Anthropic 账号 |
| `/logout` | 无 | 无 | 退出 Anthropic 账号 |
| `/install` | 无 | `[options]` | 安装 Claude Code native build |
| `/desktop` | `/app` | 无 | 在 Claude Desktop 中继续当前会话 |
| `/mobile` | `/ios`, `/android` | 无 | 显示 Claude 移动端下载二维码 |
| `/ide` | 无 | `[open]` | 管理 IDE 集成和状态 |
| `/chrome` | 无 | 无 | Claude in Chrome Beta 设置 |
| `/terminal-setup` | 无 | 无 | 终端集成设置 |
| `/install-github-app` | 无 | 无 | 为仓库设置 Claude GitHub Actions |
| `/install-slack-app` | 无 | 无 | 安装 Claude Slack app |
| `/remote-control` | `/rc` | `[name]` | 连接本机用于远程控制会话 |
| `/session` | `/remote` | 无 | 显示远程会话 URL 和二维码 |
| `/remote-env` | 无 | 无 | 配置 teleport 远程环境 |
| `/web-setup` | 无 | 无 | 设置 Claude Code on the web |
| `/upgrade` | 无 | 无 | 升级套餐入口 |
| `/extra-usage` | 无 | 无 | 配置达到限制后的额外用量 |
| `/rate-limit-options` | 无 | 无 | 触发 rate limit 时查看选项 |
| `/stickers` | 无 | 无 | 订购 Claude Code 贴纸 |
| `/voice` | 无 | 无 | 开关语音模式，特性开关可见 |

### 示例

```text
/login
/logout
/install
/desktop
/mobile
/ide
/ide open
/chrome
/terminal-setup
/install-github-app
/install-slack-app
/remote-control my-macbook
/rc office-laptop
/session
/remote-env
/web-setup
/upgrade
/extra-usage
/rate-limit-options
/stickers
/voice
```

### 使用建议

- IDE 无法联动时，先看 `/ide`，再看 `/doctor`。
- 远程控制相关命令通常受账号、组织策略和特性开关影响。
- `/install-github-app` 和 `/install-slack-app` 属于外部服务集成，执行前确认目标仓库或工作区权限。

---

## 七、内部、实验或节日命令

当前源码还包含一些明显偏内部、实验、演示或特定活动的命令。它们不一定会在普通用户版本里出现。

| 命令 | 参数 | 说明 |
|---|---|---|
| `/init` | 无 | 初始化项目上下文或偏好提示 |
| `/init-verifiers` | 无 | 创建验证类 skill，内部/实验用途 |
| `/feedback` | `[report]` | 反馈问题，别名 `/bug` |
| `/passes` | 无 | 账号/活动相关入口 |
| `/think-back` | 无 | 2025 year in review 活动入口 |
| `/thinkback-play` | 无 | 播放 thinkback 动画 |
| `/ultraplan` | `<prompt>` | 更强规划命令，特性开关可见 |
| `/bridge-kick` | 无 | 远程桥接故障注入，内部测试用途 |

示例：

```text
/feedback 运行 /mcp 后界面卡住
/init
/init-verifiers
/think-back
/ultraplan 规划一次全仓 TypeScript strict 迁移
```

---

## 八、原文提到但当前源码未确认的命令

原文里出现过一些命令，但我在当前 `commands.ts` 的内置命令注册表中没有确认到它们作为独立 slash command 存在：

```text
/goal
/background
/loop
/undo
/search
/debug
/test
/docs
/refactor
/explain
/simplify
/recap
```

这些能力可能来自：

- 其他 Claude Code 版本；
- 插件或 skill；
- 作者把自然语言工作流写成了 slash command；
- 内部构建或实验开关；
- 普通对话指令，例如“运行测试”“解释这段代码”“重构这个函数”。

如果当前 `/help` 看不到这些命令，不建议在文档中把它们写成内置命令。可以改成自然语言任务：

```text
请运行项目测试并修复失败用例
请解释 src/auth/session.ts 的登录态刷新逻辑
请重构这个函数，把校验、查询和响应组装拆开
请搜索代码库里所有直接调用 legacyApi 的位置
```

---

## 九、按场景选择命令

### 开始新任务

```text
/status
/context
/plan 实现用户登录失败次数限制，先给出方案和验证方式
```

### 长会话继续工作

```text
/resume auth
/context
/compact 保留当前任务目标、已修改文件、未解决问题
```

### 提交前检查

```text
/diff
/review
/security-review
/commit
```

### 配置外部能力

```text
/permissions
/mcp
/plugin
/skills
/hooks
```

### 排查环境问题

```text
/status
/doctor
/ide
/mcp
/release-notes
```

---

## 十、命令速查索引

### 高频基础

```text
/help
/status
/context
/compact
/diff
/plan
/resume
/clear
/copy
/export
```

### 会话管理

```text
/clear
/compact
/resume
/branch
/rewind
/rename
/tag
/btw
/copy
/export
/exit
```

### 状态诊断

```text
/help
/status
/doctor
/context
/diff
/files
/usage
/cost
/stats
/insights
/release-notes
/version
/heapdump
```

### 模型与模式

```text
/model
/effort
/fast
/plan
/advisor
/brief
/vim
/theme
/color
/output-style
/statusline
```

### 配置扩展

```text
/config
/permissions
/sandbox
/add-dir
/mcp
/memory
/hooks
/skills
/plugin
/reload-plugins
/keybindings
/privacy-settings
```

### 审查协作

```text
/review
/ultrareview
/security-review
/commit
/commit-push-pr
/pr-comments
/agents
/tasks
```

### 账号与集成

```text
/login
/logout
/install
/desktop
/mobile
/ide
/chrome
/terminal-setup
/install-github-app
/install-slack-app
/remote-control
/session
/remote-env
/web-setup
/upgrade
/extra-usage
/rate-limit-options
/stickers
/voice
```

---

## 总结

这份指南建议定位为“命令参考手册”，不强调个人经验排名，也不把未确认命令写成内置能力。

实际使用时记住三点：

1. 先用 `/help` 看当前版本真正可用的命令。
2. 对话变长用 `/context` + `/compact` 管理上下文。
3. 改代码前用 `/plan`，提交前用 `/diff` + `/review`。
