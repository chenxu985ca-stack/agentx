# Hermes Agent 安装与配置指南

> 适用 macOS / Linux · 预计 30 分钟完成

---

## 1. 前置要求

| 条件 | 检查方式 |
|------|----------|
| macOS 或 Linux | `uname -a` |
| 终端可访问外网 | `curl -I https://github.com` |
| 有 DeepSeek API Key | [DeepSeek 后台](https://platform.deepseek.com) → API Keys |

> 如果没有 DeepSeek Key：去 platform.deepseek.com 注册 → 充值 ¥10 起 → 创建 API Key

---

## 2. 安装 Hermes

### 一键安装

```bash
curl -fsSL https://ghfast.top/https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

> `ghfast.top` 是 GitHub 镜像加速，国内用户也能正常下载。

### 验证安装

```bash
source ~/.zshrc
hermes --version
```

看到版本号（如 `v0.15.1`）即安装成功。

---

## 3. 配置 Hermes

### 3.1 配置模型

编辑配置文件：

```bash
nano ~/.hermes/config.yaml
```

写入以下内容：

```yaml
model:
  default: deepseek-v4-pro
  provider: deepseek
  base_url: https://api.deepseek.com/v1
```

> ⚠️ **关键**：provider 必须写 `deepseek`，不能写 `custom`。Hermes 有原生 DeepSeek 适配。

### 3.2 配置 API Key

```bash
nano ~/.hermes/.env
```

写入：

```
DEEPSEEK_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

> ⚠️ 环境变量名是 `DEEPSEEK_API_KEY`，不是 `OPENAI_API_KEY`。

### 3.3 设置文件权限

```bash
chmod 600 ~/.hermes/.env
```

防止其他用户读取你的 API Key。

---

## 4. 验证配置

### 启动测试

```bash
hermes
```

输入：

```
你好，请用中文回复，说一句"配置成功"
```

如果收到正确回复，配置完成。

### 常见启动报错

| 报错 | 原因 | 解决 |
|------|------|------|
| `command not found: hermes` | 安装未加入 PATH | 执行 `source ~/.zshrc` |
| `401 Unauthorized` | API Key 错误 | 检查 `.env` 里 Key 是否正确 |
| `model not found` | 模型名写错 | 确认是 `deepseek-v4-pro`，不是 `deepseek-chat` |
| `connection refused` | 网络不通 | 检查代理或换 `https://api.deepseek.com` |

---

## 5. 定制记忆文件

### USER.md（告诉 Hermes 你是谁）

```bash
nano ~/.hermes/USER.md
```

示例：

```markdown
# About Me

- 角色：全栈独立开发者
- 技术栈：Next.js / TypeScript / PostgreSQL
- 工作语言：中文
- 偏好：回复用中文，代码注释用英文
```

### MEMORY.md（记录重要信息）

```bash
nano ~/.hermes/MEMORY.md
```

示例：

```markdown
# Environment

- OS: macOS 14
- Shell: zsh
- Editor: VS Code
- 工作目录: ~/projects/
```

---

## 6. 配置镜像加速（国内用户必做）

如果安装技能或更新时 GitHub 访问慢：

```bash
# 编辑 Hermes 配置
nano ~/.hermes/config.yaml
```

添加：

```yaml
github:
  mirror: https://ghfast.top/
```

---

## 7. 安装完成检查清单

- [ ] `hermes --version` 输出版本号
- [ ] `hermes` 启动后能正常对话
- [ ] `/model` 可以看到 deepseek-v4-pro
- [ ] USER.md 和 MEMORY.md 已定制
- [ ] `.env` 权限为 600

---

*AgentX · 配置完成，开始使用 → 参见《快速使用指南》*
