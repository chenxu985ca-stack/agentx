# Claude Code 安装与配置指南

> 适用于 macOS / Linux · 预计 20 分钟完成

---

## 1. 前置要求

| 条件 | 检查方式 |
|------|----------|
| macOS 或 Linux | `uname -a` |
| Node.js >= 18 | `node --version` |
| npm 已安装 | `npm --version` |
| 有 API Key（DeepSeek 或 Anthropic） | — |

> 如果没有 Node.js：`brew install node`（macOS）或 `apt install nodejs`（Linux）

---

## 2. 安装 Claude Code

### npm 全局安装

```bash
npm install -g @anthropic-ai/claude-code
```

### 验证安装

```bash
claude --version
```

---

## 3. 配置 API

### 方案 A：使用 DeepSeek API（推荐）

Claude Code 可以走 DeepSeek 后端，性价比更高：

```bash
export ANTHROPIC_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
export ANTHROPIC_BASE_URL=https://api.deepseek.com/v1
```

写入 shell 配置文件使其永久生效：

```bash
# macOS 用 .zshrc，Linux 可能是 .bashrc
cat >> ~/.zshrc << 'EOF'
export ANTHROPIC_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
export ANTHROPIC_BASE_URL=https://api.deepseek.com/v1
EOF

source ~/.zshrc
```

### 方案 B：使用 Anthropic API（官方）

如果你有 Anthropic API Key：

```bash
export ANTHROPIC_API_KEY=sk-ant-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

不需要设置 `ANTHROPIC_BASE_URL`。

---

## 4. 配置 CLAUDE.md

CLAUDE.md 是 Claude Code 的项目规则文件。放在项目根目录下，Claude Code 启动时会自动读取。

### 全局配置（所有项目生效）

```bash
nano ~/.claude/CLAUDE.md
```

写入：

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

### 项目级配置（单个项目生效）

在项目根目录创建 `CLAUDE.md`：

```bash
cd /path/to/your/project
nano CLAUDE.md
```

示例内容（Node.js 项目）：

```markdown
# CLAUDE.md

## Project Info

- Next.js 14 App Router
- TypeScript strict mode
- pnpm as package manager
- Testing: Vitest + Playwright

## Rules

- Use server components by default, only add 'use client' when needed
- API routes in app/api/, follow RESTful conventions
- Run tests before committing
```

---

## 5. 双 Agent 协同配置

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

## 6. 验证

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

## 7. 常见问题

| 问题 | 排查 |
|------|------|
| `command not found: claude` | npm 全局安装路径不在 PATH，检查 `npm config get prefix` |
| `401 Unauthorized` | API Key 不对或 `ANTHROPIC_BASE_URL` 配错 |
| 走 DeepSeek 模型名报错 | Claude Code 默认调 Claude 模型，需要在 DeepSeek 侧做模型名映射 |
| 权限被拒，无法读写文件 | Claude Code 在工作目录需要读写权限 |
| 启动慢 | 首次启动会下载依赖，等 1-2 分钟 |

---

## 8. 安装完成检查清单

- [ ] `claude --version` 输出版本号
- [ ] `ANTHROPIC_API_KEY` 环境变量已设置
- [ ] `~/.claude/CLAUDE.md` 已定制
- [ ] 项目根目录 `CLAUDE.md` 已按需创建
- [ ] `claude` 启动后能正常对话
- [ ] （如有 Hermes）双 Agent 分工规则已写入 CLAUDE.md

---

*AgentX · Claude Code 配置完成 → 参见《双 Agent 协同使用手册》*
