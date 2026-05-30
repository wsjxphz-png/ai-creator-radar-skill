---
name: ai-creator-radar
description: |
  搭建 AI 创作者机会雷达日报系统。当用户提到以下任何关键词时使用此 skill：
  AI日报、创作者日报、飞书日报、AI机会雷达、内容创作者AI助手、AI选题工具、
  AI工具发现、自动化日报、YouTube监控、Reddit监控、DeepSeek日报。
  此 skill 帮助非技术背景的内容创作者搭建一套全自动的 AI 日报系统，
  每天从 YouTube/Reddit/HackerNews/ProductHunt 抓取内容，通过 DeepSeek
  筛选评分后推送到飞书。
---

# AI 创作者机会雷达

帮助非技术背景的内容创作者搭建全自动 AI 日报系统。

## 系统功能

每天早上自动抓取 50+ 信息源，通过 DeepSeek API 筛选/评分/翻译/汇总，
生成中文日报推送到飞书。全程零代码，一次部署永久运行。

## 日报栏目

1. 🔥 今日最重要机会——能赚钱/涨粉/提效/扩大影响力
2. 🎯 内容机会——正在爆发的内容方向
3. 🎬 爆款内容实验室——拆解爆款内容的标题/结构/情绪
4. 🛠 今日最佳工作流——普通人可复制的自动化
5. 🤖 AI 能力突破——以前很难，现在 AI 能做到的事
6. 💰 商业模式观察——可复制的 AI 创业/变现案例
7. 🧠 最佳工具发现——真正有用的新工具
8. 📈 趋势雷达——升温/降温/值得布局
9. 💡 给我的建议——今天最值得投入 2 小时研究什么

## 部署步骤

### 前置条件

部署前确认用户是否已有：
- GitHub 账号（免费注册）
- DeepSeek API Key（platform.deepseek.com，充值 ¥1 即可）
- 飞书机器人 Webhook（飞书中搜索"飞书机器人"创建）

### Step 1: Fork 模板仓库

帮用户 Fork `https://github.com/wsjxphz-png/ai-radar` 到他们的 GitHub 账号。
如果用户没有 GitHub 账号，先帮他们注册。

### Step 2: 设置 GitHub Secrets

在 Fork 后的仓库中 Settings → Secrets and variables → Actions 添加：
- `DEEPSEEK_API_KEY`: DeepSeek API Key
- `FEISHU_WEBHOOK`: 飞书机器人 Webhook 地址

用 `gh secret set` 命令完成：
```bash
gh secret set DEEPSEEK_API_KEY --repo <user>/ai-radar --body "sk-xxx"
gh secret set FEISHU_WEBHOOK --repo <user>/ai-radar --body "https://open.feishu.cn/..."
```

### Step 3: 启用 Actions

仓库 → Actions → Enable workflow → 手动 Run workflow 一次测试。

### Step 4: 自定义信息源

编辑 `sources.json` 添加或修改信息源。格式：
```json
{"name": "频道名", "channel_id": "UCxxx", "category": "ai-content", "note": "说明"}
```

可用的 category: `ai-content`, `ai-workflow`, `ai-startup`, `productivity`, `agent`

### Step 5: 验证

检查飞书是否收到日报。如果推送成功，系统已就绪。
之后每天北京时间 09:00 自动运行，10:00 前收到日报。

## 常见问题

### 日报内容为空
1. 检查 DeepSeek API Key 是否有效、是否有余额
2. 检查 GitHub Actions 日志是否有报错
3. 信息源可能当天没有新内容（极少见）

### 想修改推送时间
编辑 `.github/workflows/daily-report.yml`：
```yaml
- cron: '0 1 * * *'  # UTC 01:00 = 北京时间 09:00
```

### 想换 AI 引擎
在 GitHub Secrets 中修改：
- 换用 DeepSeek R1: 添加 `DEEPSEEK_MODEL` = `deepseek-reasoner`
- 换用 Claude: 修改 `main.py` 中的 API endpoint 和 model

### YouTube 频道 ID 怎么找
打开 YouTube 频道页面 → 查看页面源代码 → 搜索 `externalId` → 复制 `UC...` 开头的值。

### RSSHub 公共实例死掉了怎么办
系统不依赖 RSSHub。YouTube 使用原生 RSS (`youtube.com/feeds/videos.xml`)，
Reddit 使用原生 RSS (`.rss`)，HN/ProductHunt 自带 RSS。
只有微信公众号需要第三方桥接（Wechat2RSS），标记为可选。

## 系统架构

```
GitHub Actions (免费, 每天自动触发)
  ├── YouTube 原生 RSS ← 频道更新
  ├── Reddit 原生 RSS ← 子版块热帖
  ├── Hacker News RSS ← 技术创业信号
  └── Product Hunt RSS ← AI 新产品
         ↓
    预过滤（时间去重/营销过滤）
         ↓
    DeepSeek API（四维评分 + 机会化改写）
         ↓
    飞书卡片消息推送
```

## 安全提示

- DeepSeek API Key 和飞书 Webhook 存储在 GitHub Secrets 中，不会泄露
- 不收集任何用户数据
- 代码完全开源，可审计

## 相关文件

- `scripts/main.py` — 主脚本（抓取→过滤→AI→推送）
- `references/sources.json` — 信息源配置模板
- `references/daily-report.yml` — GitHub Actions 工作流
