# AgentX — 交付 SOP

## 通用前置检查

- [ ] 确认客户 OS（macOS / Windows / Linux）
- [ ] 确认网络环境（国内/海外，是否需要镜像）
- [ ] 确认客户现有 API Key 情况（有/无 DeepSeek/OpenAI 账号）
- [ ] 预约远程桌面（ToDesk / AnyDesk / 向日葵）
- [ ] 创建客户文档夹 `~/clients/<客户名>/`

---

## 🟢 基础配置 (¥599, 1小时交付)

### 交付清单
1. **安装 Hermes Agent** (20min)
   ```bash
   curl -fsSL https://ghfast.top/https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
   source ~/.zshrc
   hermes --version  # 验证
   ```

2. **配置模型** (15min)
   - 编辑 `~/.hermes/config.yaml`
     ```yaml
     model:
       default: deepseek-v4-pro
       provider: deepseek
       base_url: https://api.deepseek.com/v1
     ```
   - 编辑 `~/.hermes/.env`
     ```
     DEEPSEEK_API_KEY=<客户Key或代申请>
     ```
   - 验证：`hermes` → 输入 "你好" → 确认回复

3. **定制记忆文件** (15min)
   - `~/.hermes/USER.md` — 根据客户职业/需求定制
   - `~/.hermes/MEMORY.md` — 记录环境信息

4. **教学** (10min)
   - 启动命令：`hermes`
   - 基本对话
   - `/model` 切换模型
   - 如何停止和重启

### 交付物
- [ ] Hermes 可正常运行
- [ ] USER.md / MEMORY.md 已定制
- [ ] 客户成功发送第一条消息并收到回复
- [ ] 一份简短的《使用指南》发客户微信（源文件：`guides/quickstart.md`）

---

## 🔵 双 Agent 协同 (¥1,999, 半天交付)

### 交付清单
包含基础配置全部 + 以下：

1. **Claude Code 安装配置** (30min)
   - 确认 Claude Code CLI 已安装
   - 配置 `CLAUDE.md`（根据客户项目定制）
   - 配置双 Agent 分工规则
   - 验证：Claude Code 可以正常调用

2. **协同工作流搭建** (30min)
   - CLAUDE.md 写入 "Co-Agent: Hermes" 章节
   - Hermes USER.md 写入 Claude Code 分工
   - 创建共享技能目录
   - 验证协同：在 Hermes 里说"让 Claude Code 帮我查一下代码"

3. **20+ 核心技能激活** (20min)
   ```bash
   hermes skills install research
   hermes skills install github
   hermes skills install productivity
   hermes skills install software-development
   ```

4. **实操带教** (60min)
   - 场景1：用 AI 写代码（Claude Code）
   - 场景2：用 AI 查资料（Hermes）
   - 场景3：双 Agent 接力
   - 场景4：客户自己的工作任务实战

### 交付物
- [ ] 基础配置全部交付物
- [ ] Claude Code 可正常使用
- [ ] 双 Agent 分工规则就位
- [ ] CLAUDE.md / USER.md / MEMORY.md 定制完成
- [ ] 20+ 核心技能已激活
- [ ] 客户完成至少一个实战场景
- [ ] 《双 Agent 使用手册》（源文件：`guides/dual-agent-handbook.md`）+ 30 分钟教学录屏

---

## 🟣 企业全栈部署 (¥3,999, 1-3天交付)

### 交付清单
包含双 Agent 全部 + 以下：

1. **Gateway 多平台接入** (2小时)
   - 根据客户需求配置 Telegram/Discord/Slack/微信/飞书/钉钉
   - 配置 allowlist 安全策略
   - 测试每个平台：发消息 → Agent 回复

2. **Cron 定时任务定制** (1小时)
   ```bash
   hermes cronjob create --name "每日竞品监测" --cron "0 9 * * *" --prompt "搜索<竞品名>最新动态，总结三条要点"
   ```
   - 至少 5 条客户专属定时任务
   - 验证每条任务正常触发

3. **业务专属技能开发** (2小时)
   - 根据客户业务开发 3 个自定义技能
   - 可包括：竞品分析、日报生成、客户邮件回复模板等
   - 写入 `~/.hermes/skills/custom/`

4. **MCP 工具集成** (1小时)
   - 配置客户需要的 MCP 服务器
   - 如：GitHub MCP、Notion MCP、飞书 MCP 等

5. **安全加固** (30min)
   - 配置 tirith 安全扫描
   - 设置 `sudo_password` 或密码免密
   - 确认 API Key 权限最小化

6. **1 周陪跑** (7天)
   - 每日 check-in 微信消息
   - 有问题随时响应（工作日 9:00-21:00）
   - 第 7 天汇总优化建议

### 交付物
- [ ] 双 Agent 全部交付物
- [ ] Gateway 多平台接入配置文档
- [ ] 5 条 Cron 任务正常运行
- [ ] 3 个自定义技能文件
- [ ] MCP 工具集成完成
- [ ] 安全扫描通过
- [ ] 《企业全栈部署手册》（源文件：`guides/enterprise-handbook.md`）
- [ ] 7 天陪跑记录

---

## 售后流程

| 时间 | 基础版 | 标准版 | 企业版 |
|------|--------|--------|--------|
| 交付后 1 天 | 微信确认一切正常 | 同左 | 同左 |
| 交付后 3 天 | - | 主动回访 | 每日 check-in |
| 交付后 7 天 | 售后到期提醒 | 主动回访 | 汇总优化建议 |
| 交付后 14 天 | - | 售后到期提醒 | 主动回访 |
| 交付后 30 天 | - | - | 售后到期提醒 |

## 客户文档模板

每个客户独立文件夹 `~/clients/<客户名>/`：
```
<客户名>/
  ├── notes.md           # 需求记录、沟通笔记
  ├── config-backup/     # 客户的 config.yaml / .env（脱敏）
  ├── skills-custom/     # 为客户定制的技能备份
  └── delivery-checklist.md  # 本次交付的勾选清单
```
