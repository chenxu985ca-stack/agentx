# AgentX · D1 即刻动态

## 正文

今天装了个 Hermes Agent，我以为就是一行 curl | bash 的事。

结果 DeepSeek API 死活 401，折腾了半小时。

踩了三个坑：

1️⃣ 模型名不是 deepseek-chat，是 deepseek-v4-pro
2️⃣ Hermes 有原生 deepseek provider，不能走 custom
3️⃣ 它读的是 DEEPSEEK_API_KEY，不是 OPENAI_API_KEY

解决之后，让 Claude Code 和 Hermes 互相认识了一下——现在一个负责写代码，一个负责搜资料、跑定时任务、管多平台消息。感觉像给自己招了两个数字员工。

总结：装 Agent 容易，配好难。大部分人的 AI Agent 都是装完吃灰的，因为没人帮你走完配置和分工这最后一步。

我觉得这是一个机会。

#AI Agent #ClaudeCode #独立开发 #AI工具

---

## 配图建议

截两张图：
1. `hermes --version` 终端输出（证明装好了）
2. Claude Code 和 Hermes 双开的桌面截图

---

## 发布时间

今晚 20:00-21:00（即刻高峰期）

---

## 发完后的动作

- 在评论区补充一条：「顺便问一下，大家平时用 AI Agent 主要做什么？我想看看有没有什么我不知道的玩法」
- 有评论就及时回复，前 10 个互动很重要
