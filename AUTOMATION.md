# Hermes 业务自动化配置

## 启动 Gateway（后台常驻）

```bash
# 安装 gateway 服务
hermes gateway install

# 启动（后台运行）
hermes gateway start

# 查看状态
hermes gateway status
```

Gateway 启动后 Hermes 可以在后台：
- 执行 cron 定时任务
- 接收消息平台指令
- 自主触发记忆整理和技能生成

---

## 推荐定时任务

### 1. 每日 AI Agent 热点扫描
```bash
hermes cronjob create \
  --name "AI Agent 热点扫描" \
  --cron "0 8 * * *" \
  --prompt "搜索今天关于 AI Agent、Claude Code、Hermes 的最新中文资讯，挑 3 条最有话题性的，写成一句话摘要，适合发即刻/小红书"
```
→ 每天早上 8 点自动搜热点，你直接选题发内容

### 2. 竞品监测
```bash
hermes cronjob create \
  --name "竞品监测" \
  --cron "0 10 * * *" \
  --prompt "搜索国内提供 AI Agent 部署/搭建服务的个人和团队，看看他们最近在做什么、怎么定价、有什么新动作"
```

### 3. 内容灵感生成
```bash
hermes cronjob create \
  --name "内容灵感" \
  --cron "0 14 * * *" \
  --prompt "基于今天 AI 领域的热门话题，帮我生成 3 个「即刻」动态的内容灵感，每个不超过 200 字。要求：有料、有趣、适合技术社区"
```

### 4. 客户跟进提醒
```bash
hermes cronjob create \
  --name "客户跟进提醒" \
  --cron "0 9 * * 1" \
  --prompt "检查 ~/clients/ 目录下的客户记录。列出所有已交付客户中需要本周回访的（基于 SOP 售后时间表），生成回访消息草稿"
```

### 5. 周末复盘
```bash
hermes cronjob create \
  --name "周复盘" \
  --cron "0 10 * * 6" \
  --prompt "总结这周我用 Claude Code + Hermes 完成了什么有价值的事，写成一篇「本周 AI Agent 实战周报」格式，适合发即刻和知乎"
```

---

## 技能开发

随着业务推进，Hermes 会自动从你的使用中生成技能。你也可以主动引导：

```
# 在 hermes 对话中：
"我刚做了一次客户交付，帮我生成一个交付 checklist 技能，下次照着做"
"帮我把竞品监测的流程总结成技能"
```

主动创建的技能会存在 `~/.hermes/skills/custom/`，Claude Code 也能加载使用。

---

## 查看和管理定时任务

```bash
hermes cronjob list          # 列出所有任务
hermes cronjob run <name>    # 手动立即执行一次
hermes cronjob pause <name>  # 暂停
hermes cronjob resume <name> # 恢复
hermes cronjob remove <name> # 删除
hermes cronjob logs <name>   # 查看执行日志
```
