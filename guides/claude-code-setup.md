# Claude Code 完整指南

> 从安装到精通 · 适用于 macOS / Windows / Linux

---

## 1. IDE 选择与环境准备

部署 Claude Code 前，推荐安装一个 Agent IDE（免费即可）：

| IDE | 下载 | 特点 |
|-----|------|------|
| **Cursor** | [cursor.com](https://cursor.com) | 最流行，AI 原生集成 |
| **Trae** | [trae.ai](https://trae.ai)（国内版）/ [trae.ai/en](https://trae.ai/en)（海外版） | 字节出品，国内可用 |
| **Google Anti-gravity** | [官网](https://idx.google.com) | Google 出品，云端 IDE |

> 以下教程以 macOS 为主，Windows 差异处已标注。

---

## 2. 安装 Claude Code

### macOS

**步骤 1：装 Homebrew**

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

**步骤 2：把 Homebrew 加到 PATH**

装完后终端会有提示，或者直接让 Claude Code 帮你配：

```
帮我把 homebrew 加到 PATH 路径变量里面去
```

**步骤 3：安装 Claude Code**

```bash
brew install --cask claude-code@latest
```

**步骤 4：验证**

```bash
claude --version
```

### Windows

**步骤 1：安装 Git**

在 IDE 终端输入：

```powershell
winget install Git.Git
```

遇到选项直接输入 `y`（yes）。

**步骤 2：安装 Claude Code**

- **PowerShell**（IDE 终端默认用这个）：

```powershell
irm https://claude.ai/install.ps1 | iex
```

- **CMD**：

```cmd
curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
```

**步骤 3：把 Claude Code 加进 PATH**

```powershell
winget install Anthropic.ClaudeCode
```

安装完后把 Claude Code 的可执行文件路径配置到系统环境变量的 Path 里。

**步骤 4：验证**

重启 IDE 终端后：

```powershell
claude --version
```

---

## 3. 配置大模型

### 方案 A：DeepSeek API（推荐，国内友好）

```bash
# 写入环境变量
cat >> ~/.zshrc << 'EOF'
export ANTHROPIC_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
export ANTHROPIC_BASE_URL=https://api.deepseek.com/v1
EOF

source ~/.zshrc
```

> Windows 用户：在系统环境变量里添加这两条。

### 方案 B：Anthropic API（官方）

```bash
export ANTHROPIC_API_KEY=sk-ant-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

不需要设 `ANTHROPIC_BASE_URL`。

### 方案 C：CC Switch 多模型管理

CC Switch 是一个 Claude Code 的模型切换工具，支持多种模型自由切换。

**安装步骤：**

1. GitHub 下载页：https://github.com/farion1231/cc-switch/releases/tag/v3.14.1
2. 根据系统选择安装包下载
3. **务必在启动 Claude Code 之前先设置好 CC Switch**

**三种工作模式：**

| 模式 | 功能 |
|------|------|
| **默认模式** | 启动后的初始状态，增强版对话终端，用于意图确认和信息查询 |
| **计划模式（Plan）** | 分析需求并制定执行步骤，不直接动代码 |
| **Accept Edits** | 执行代码变更的最终确认与写入，是"落地"阶段 |

> 在 CC Switch 中可以在三种模式之间自由切换。

---

## 4. 权限模式

Claude Code 有三种权限态度，在 Accept Edits 模式下执行命令时会问你：

| 选项 | 含义 |
|------|------|
| 仅同意这一次 | 只允许当前这次命令执行 |
| 同意且不再询问 | 该项目之后执行同类操作不再问 |
| 不同意 | 拒绝，再商量 |

**如果你想一路绿灯（自动执行所有操作）**：

```bash
claude --dangerously-skip-permissions
```

> ⚠️ 注意：这会跳过所有权限确认。建议只在信任的项目中使用。

---

## 5. 文件输入方式

### @ 指令引用本地文件

在对话中使用 `@` 符号让 Claude Code 读取项目文件：

```
帮我看看 @/src/utils/helper.ts 这个文件的逻辑
对比一下 @/src/old.ts 和 @/src/new.ts 的差异
```

### 拖拽图片

直接拖图片到终端，Claude Code 能识别图片内容（适合 UI 还原、截图提问等）。

---

## 6. CLAUDE.md 配置

CLAUDE.md 是 Claude Code 的项目规则文件，启动时自动读取。

### 全局配置（所有项目生效）

```bash
nano ~/.claude/CLAUDE.md
```

```markdown
# CLAUDE.md

## About Me

- 角色：全栈独立开发者
- 技术栈：Next.js / TypeScript / PostgreSQL
- 偏好：代码注释用英文，commit message 用英文
- 回复用中文

## Work Style

- 修改代码前先读文件
- 保持与周围代码风格一致
- 优先使用项目已有的工具库
```

### 项目级配置

在项目根目录创建 `CLAUDE.md`：

```bash
cd /path/to/your/project
nano CLAUDE.md
```

```markdown
# CLAUDE.md

## Project Info

- Next.js 14 App Router
- TypeScript strict mode
- pnpm as package manager
- Testing: Vitest + Playwright

## Rules

- Use server components by default
- API routes in app/api/, follow RESTful
- Run tests before committing
```

### 项目级 vs 全局级

| | 全局 CLAUDE.md | 项目 CLAUDE.md |
|---|---|---|
| 位置 | `~/.claude/CLAUDE.md` | 项目根目录 |
| 作用范围 | 所有项目 | 仅当前项目 |
| 优先级 | 低 | 高（会覆盖全局） |
| 适合放什么 | 个人信息、通用风格 | 项目技术栈、特定规则 |

---

## 7. 上下文管理

Claude Code 的上下文窗口有限，管理好上下文能大幅提升体验。

### 查看上下文用量

```bash
/context
```

会展示详细的上下文占比信息。

### 主动压缩

当上下文使用率高于 60% 时建议压缩：

```bash
/compact
```

> Claude Code 也会在上下文即将满时自动压缩。

### 彻底清空

```bash
/clear
```

### 上下文状态栏常驻

让 Claude Code 帮你在终端底部显示当前目录、模型、上下文剩余百分比：

```
帮我配一个 statusLine，显示当前目录 + 模型 + 上下文剩余百分比
```

根据 CC 引导完成后重启终端即可。

### 恢复历史会话

```bash
/resume
```

选择你想恢复的会话。

---

## 8. 子 Agent（SubAgent）

子 Agent 是 Claude Code 可以调用的独立执行单元，适合并行处理多任务。

### 创建子 Agent

输入 `/agents`，在 Library 下选择创建项目级子 Agent：

1. 为子 Agent 描述用途（如"专门处理数据库迁移"）
2. 选择模型（一般选 Sonnet 即可；如果用了 CC Switch，选哪个都一样）
3. 为子 Agent 选一个区别于主 Agent 的颜色
4. 创建完成

### 调用与管理

输入 `/agents`，在 Library 下选择已创建的子 Agent，根据需求进行操作。

> 子 Agent 适合：并行代码审查、多文件重构、独立模块开发。

---

## 9. Hook 钩子

Hook 可以在特定事件触发时自动执行脚本。

### 推荐 Hook 设置

直接告诉 Claude Code 帮你设置：

**任务完成提示音：**

```
设置一个 hook，每次完成任务之后都自动执行一个声音脚本，发出提示音"叮"进行提醒
```

**提交前格式检查：**

```
设置一个 hook，每次提交代码之前都自动触发代码格式的检查
```

---

## 10. Plugin 插件

Plugin 是打包了 Skill、SubAgent、Hook、MCP 的整合性概念。

### 进入插件管理

```bash
/plugin
```

在插件管理界面可以：
- 发现新插件
- 管理已下载插件
- 新增插件生态

---

## 11. Skills 技能

Skills 是 Claude Code 的可复用能力模块，相当于给 Claude Code "装技能包"。

### 技能市场

- **Lobehub Skills**：最大的技能合集网站，按分类查找或直接看精选合集
- **Agent Skills 指南（Claude Code 版）**：网页版教学文档，教你创建自己的 Skill

### 安装技能

将 Skill 文件放入对应文件夹：

| 级别 | 路径 |
|------|------|
| 项目级 | 项目根目录下的 `.claude/skills/` |
| 全局级 | `~/.claude/skills/` |

### 创建自己的 Skill

Skill 本质是一个 Markdown 文件，包含：
- **name** — 技能名称
- **description** — 一句话描述（Claude Code 用这个判断何时调用）
- **instructions** — 详细执行指令

参考 Agent Skills 指南编写即可。

---

## 12. MCP 扩展

MCP（Model Context Protocol）让 Claude Code 连接外部工具和服务。

### 学习资源

- 视频教程：用神器 Claude Code！打造贴身 AI 秘书团（小白教程）
- 文档教程：Claude Code 教程

### 常见 MCP 工具

| 工具 | 功能 |
|------|------|
| GitHub MCP | Issue/PR 管理、代码搜索 |
| Notion MCP | 文档读写、数据库查询 |
| 飞书 MCP | 消息发送、文档操作 |
| Postgres MCP | 数据库直接查询 |
| Filesystem MCP | 安全的文件读写 |

---

## 13. Git 设置

直接告诉 Claude Code，让它帮你操作：

```
帮我下载 Git，并与我的 GitHub 账号绑定
```

根据 CC 引导完成即可。

---

## 14. CLI 工具推荐

把下载链接直接发给 Claude Code，让它全权负责安装和引导：

| CLI 名称 | 功能 | 下载 |
|----------|------|------|
| **飞书 CLI** | 消息、文档、多维表格等 200+ 命令 + 24 个 AI Agent Skills | GitHub |
| **OpenCLI** | 万能命令行工具 | GitHub |

---

## 15. 双 Agent 协同配置

如果同时使用 Hermes，在 CLAUDE.md 中加入分工规则：

```markdown
## Co-Agent: Hermes

This workspace has Hermes Agent installed as a co-agent.

### Division of Labor

| Claude Code | Hermes |
|---|---|
| Code writing, editing, refactoring | Web research & information gathering |
| Git operations, commits, PRs | Scheduled/cron tasks |
| Debugging, testing, code review | Multi-platform messaging |
| File system operations | Long-running workflows |

### When to Suggest Hermes

- 搜索类任务 → 建议用 Hermes
- 定时/周期任务 → Hermes cronjob
- 多平台消息 → Hermes gateway
```

---

## 16. 验证

### 启动测试

```bash
cd /path/to/your/project
claude
```

输入：

```
你好，帮我看看这个项目的文件结构
```

Claude Code 应该能读取项目文件并回复。

### 检查 CLAUDE.md 是否生效

```
你读取了我的 CLAUDE.md 吗？复述一下你对我的了解。
```

---

## 17. 常见问题

| 问题 | 排查 |
|------|------|
| `command not found: claude` | npm 全局路径不在 PATH；Windows 检查环境变量 |
| `401 Unauthorized` | API Key 不对或 `ANTHROPIC_BASE_URL` 配错 |
| 走 DeepSeek 模型名报错 | Claude Code 默认调 Claude 模型，需在 DeepSeek 侧做模型名映射 |
| 权限被拒，无法读写文件 | 工作目录需读写权限 |
| 启动慢 | 首次启动下载依赖，等 1-2 分钟 |
| 上下文满了 | `/compact` 压缩或 `/clear` 清空 |
| 忘记之前做到哪了 | `/resume` 恢复历史会话 |

---

## 18. 安装完成检查清单

- [ ] `claude --version` 输出版本号
- [ ] API Key 环境变量已设置
- [ ] `~/.claude/CLAUDE.md` 已定制
- [ ] 项目根目录 `CLAUDE.md` 已按需创建
- [ ] `claude` 启动后能正常对话
- [ ] statusLine 已配好（上下文使用率可见）
- [ ] （如有 Hermes）双 Agent 分工规则已写入
- [ ] CC Switch 已安装（可选）
- [ ] Git 已绑定 GitHub（可选）

---

*AgentX · 下一步 → 参见《双 Agent 协同使用手册》*
