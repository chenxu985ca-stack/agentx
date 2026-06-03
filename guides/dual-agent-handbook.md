# AgentX 双 Agent 协同使用手册

> 标准版交付文档 · 适用 Claude Code + Hermes 双引擎用户

---

## 目录

1. [系统概览](#1-系统概览)
2. [Claude Code 快速上手](#2-claude-code-快速上手)
3. [Hermes 快速上手](#3-hermes-快速上手)
4. [分工法则](#4-分工法则)
5. [协同工作流实战](#5-协同工作流实战)
6. [技能系统](#6-技能系统)
7. [常见问题](#7-常见问题)

---

## 1. 系统概览

你现在的 AI 团队结构：

```
你（人类）
  ├── Claude Code（写代码、改文件、Git 操作）
  └── Hermes（查资料、定时任务、多平台消息）
```

| | Claude Code | Hermes |
|---|---|---|
| **定位** | 你的 AI 程序员 | 你的 AI 助理 |
| **擅长** | 写代码、重构、Debug、Git | 搜索、定时任务、跨平台 |
| **运行方式** | 终端 CLI，按需启动 | 常驻后台，随时可用 |
| **启动命令** | `claude` | `hermes` |

---

## 2. Claude Code 快速上手

### 启动

在项目目录下：

```bash
cd /path/to/your/project
claude
```

### 基本用法

直接描述你想做的事，Claude Code 会自己读代码、写代码、执行命令：

```
> 帮我修复这个登录页面的 bug
> 给这个函数加单元测试
> 重构一下这个模块，拆成两个文件
```

### 关键概念

- **CLAUDE.md** — 项目规则文件，Claude Code 启动时会自动读取。你的项目里已经有了，包含双 Agent 分工规则
- **Memory** — Claude Code 会记住你的偏好和项目信息
- **权限确认** — Claude Code 在执行修改前会征求你的同意

### 常用操作

| 操作 | 做法 |
|------|------|
| 写新功能 | 直接描述需求，Claude Code 会自己规划并实现 |
| 修 Bug | 描述 bug 现象或粘贴报错信息 |
| 代码审查 | "帮我 review 一下这次的改动" |
| 提交代码 | "帮我 commit 这些改动" |
| 询问代码 | "这个函数是干嘛的？" |

---

## 3. Hermes 快速上手

参考《AgentX 快速使用指南》（`quickstart.md`），这里补充高级功能。

### Hermes 特有功能

| 功能 | 用法 |
|------|------|
| 网页搜索 | "帮我搜一下 React 19 的新特性" |
| 定时任务 | `hermes cronjob create` |
| 多平台消息 | 通过 Gateway 配置 |

---

## 4. 分工法则

### 核心原则：谁擅长，谁来做

**找 Claude Code 做的事：**
- 写代码、改代码
- 调试、修 bug
- 代码审查、重构
- Git 操作（commit、branch、merge）
- 项目文件管理
- 运行测试

**找 Hermes 做的事：**
- 搜索技术资料、最新动态
- 定时任务（每日报告、监测提醒）
- 查阅文档、论文
- 需要联网的信息获取
- 非代码类的文本处理

### 实际判断流程

```
有个任务 → 需要改代码吗？
  ├── 是 → 找 Claude Code
  └── 否 → 需要搜索/定时/跨平台吗？
            ├── 是 → 找 Hermes
            └── 否 → 随便哪个都行
```

---

## 5. 协同工作流实战

### 场景一：开发新功能

**步骤：**

1. **Hermes 做调研**（10min）
   ```
   > 帮我搜一下 Next.js App Router 的最佳实践，总结 5 条要点
   > 查一下 XXX 库的最新 API 文档
   ```

2. **Claude Code 写代码**（30min）
   ```
   > 根据 Hermes 搜到的最佳实践，帮我实现一个新路由 /dashboard
   > 参考这个 API 文档，对接 XXX 接口
   ```

3. **Claude Code 写测试 + 提交**
   ```
   > 给刚才的代码加测试
   > commit 一下，message 写 "feat: add dashboard route"
   ```

### 场景二：修 Bug

**步骤：**

1. **Claude Code 定位问题**
   ```
   > 用户反馈登录后白屏，帮我排查一下
   ```
   Claude Code 会自己读报错日志、追踪代码、定位根因。

2. **（如需要）Hermes 搜索类似问题**
   ```
   > 搜一下 "Next.js middleware redirect infinite loop"，看看社区怎么解决的
   ```

3. **Claude Code 修复 + 验证**
   ```
   > 按这个方案修一下，然后跑一下登录流程的测试
   ```

### 场景三：每日工作流

**早上：**
```
Hermes:  帮我总结昨晚 GitHub 上关注的项目的更新
Claude Code: （继续昨天的开发任务）
```

**开发中：**
- Claude Code 写代码
- 遇到不确定的技术选型 → Hermes 搜索对比

**下班前：**
```
Claude Code: 帮我总结今天改了什么，生成 commit message
Hermes:  把这些改动摘要发到团队 Telegram
```

---

## 6. 技能系统

Hermes 技能是预制的自动化能力模块。

### 查看已安装技能

```bash
hermes skills list
```

### 你的技能清单

| 技能分类 | 包含技能 |
|----------|----------|
| **research** | 学术搜索、博客监测、技术动态 |
| **github** | 仓库分析、代码审查辅助、Issue 管理 |
| **productivity** | 日程管理、笔记整理、邮件草拟 |
| **software-development** | 调试辅助、技能开发、技术文档 |

### 安装更多技能

```bash
hermes skills install <category>
```

可用的分类有：`finance`、`design`、`marketing`、`data-science` 等。

### 技能共享

Claude Code 和 Hermes 共享一个技能目录。Hermes 安装的技能，Claude Code 也能感知到。

---

## 7. 常见问题

### Q: 两个 Agent 会不会冲突？

不会。它们运行在不同的进程中，读写不同的配置文件。不存在竞争。

### Q: 可以同时开两个吗？

可以。两个终端窗口，左边 Claude Code 右边 Hermes，各干各的。

### Q: Claude Code 能调用 Hermes 吗？

当前版本通过 CLAUDE.md 里的分工规则实现"软协同"——Claude Code 会在合适的场景提示你用 Hermes。完全互调还在规划中。

### Q: API 费用怎么算？

两个 Agent 共用同一个 DeepSeek API Key。费用取决于你的实际用量：
- 简单对话：很便宜
- 大量代码生成：中等
- 长文档处理：稍高

建议先在 DeepSeek 后台设置月度预算上限。

### Q: 代理/网络问题

如果在国内使用，Hermes 安装技能时可能遇到 GitHub 访问慢的问题。我们已经配置了镜像加速，如果还有问题，联系我。

---

## 附录：日常命令速查

```bash
# Claude Code
claude                  # 启动
claude "修一下这个bug"   # 带任务启动
claude --resume         # 恢复上次会话
Ctrl+C                  # 中断当前操作

# Hermes
hermes                  # 启动
hermes update           # 更新
hermes skills list      # 查看技能
hermes cronjob list     # 查看定时任务
Ctrl+C                  # 退出
```

---

*AgentX · 双引擎 AI 团队，为你工作*
