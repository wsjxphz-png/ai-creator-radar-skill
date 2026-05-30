# 🤖 AI Creator Radar — Claude Code Skill

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://claude.ai/code)

一个面向**非技术背景内容创作者**的 AI 日报系统。每天早上自动从 YouTube、Reddit、Hacker News、Product Hunt 抓取内容，通过 DeepSeek 筛选评分，生成中文日报推送到飞书。

**零代码。免费。30 分钟部署。终身自动运行。**

---

## 🎯 这不是

- ❌ AI 新闻日报
- ❌ 科技资讯聚合器
- ❌ 程序员技术周刊

## ✅ 这是

一个 **AI 创作者参谋部**。每天帮你回答：

1. 今天有什么值得**学习**？
2. 今天有什么值得**模仿**？
3. 今天有什么值得**做**？
4. 今天有什么值得**赚钱**？
5. 今天有哪些 AI 能力可以帮我**突破过去做不到的事情**？

---

## 📰 日报栏目

| # | 栏目 | 说明 |
|---|------|------|
| 🔥 | 今日最重要机会 | 能赚钱/涨粉/提效的事，带难度评估和行动建议 |
| 🎯 | 内容机会 | 正在爆发的内容方向，值不值得跟进 |
| 🎬 | 爆款内容实验室 | 拆解爆款的标题/结构/情绪/传播原因 |
| 🛠 | 今日最佳工作流 | 普通人可复制的自动化方法 |
| 🤖 | AI 能力突破 | 以前很难、现在 AI 能做到的事 |
| 💰 | 商业模式观察 | AI 产品的收费/获客/可复制性分析 |
| 🧠 | 最佳工具发现 | 真正有用的新工具 |
| 📈 | 趋势雷达 | 升温/降温/值得布局的方向 |
| 💡 | 给我的建议 | 今天最值得投入 2 小时的事 |

---

## 🚀 快速开始

### 你需要

- GitHub 账号（免费）
- DeepSeek API Key（platform.deepseek.com，充值 ¥1）
- 飞书机器人 Webhook（免费）

### 三步部署

**1. Fork 模板仓库**

打开 https://github.com/wsjxphz-png/ai-radar → Fork

**2. 设置密钥**

Settings → Secrets → Actions → 添加：
- `DEEPSEEK_API_KEY`
- `FEISHU_WEBHOOK`

**3. 启用运行**

Actions → Run workflow

**✅ 完成。之后每天自动运行。**

---

## 📊 系统架构

```
GitHub Actions（免费定时触发）
  ├── YouTube 原生 RSS  ← 39 个频道的更新
  ├── Reddit 原生 RSS   ← 18 个子版块的热帖
  ├── Hacker News RSS   ← 技术创业信号
  └── Product Hunt RSS  ← AI 新产品
         ↓
    预过滤（去旧·去重·去营销）
         ↓
    DeepSeek API（四维评分：内容价值/商业价值/可复制性/相关度）
         ↓
    飞书卡片消息推送
```

---

## 💰 费用

| 项目 | 月费 |
|------|:--:|
| GitHub Actions | ¥0 |
| DeepSeek API | ~¥1.5 |
| 飞书 | ¥0 |
| **合计** | **~¥1.5** |

---

## 🔧 自定义

### 添加信息源

编辑 `sources.json`：

```json
// YouTube 频道
{"name": "频道名", "channel_id": "UCxxx", "category": "ai-content", "note": "说明"}

// Reddit 子版块
{"subreddit": "ChatGPT", "category": "ai-content", "note": "说明"}
```

类别：`ai-content` / `ai-workflow` / `ai-startup` / `productivity` / `agent`

### 修改推送时间

编辑 `.github/workflows/daily-report.yml`：
- `0 1 * * *` = 北京时间 09:00
- `0 2 * * *` = 北京时间 10:00

### 更换 AI 引擎

支持 DeepSeek V3 / R1 / Claude / Gemini。修改 `main.py` 的 API endpoint 即可。

---

## ❓ 常见问题

**日报为空？** → 检查 API Key 和账户余额，查看 GitHub Actions 日志。

**为什么没有 X/Twitter？** → 2026 年 X 关闭了所有免费通道。可用 self-hosted Nitter 或 rss.app 桥接。

**能加微信公众号吗？** → 支持通过 Wechat2RSS 桥接（标记为可选，不稳定）。

---

## 📂 仓库

| 仓库 | 说明 |
|------|------|
| [ai-radar](https://github.com/wsjxphz-png/ai-radar) | 日报系统源码 |
| [ai-creator-radar-skill](https://github.com/wsjxphz-png/ai-creator-radar-skill) | 本仓库·Claude Code Skill |

---

## 📄 License

MIT © 2026

---

*Made for content creators who don't code.*
