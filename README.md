# daily-news

每日世界新闻整理(跨源 cross-check + 事件归类)。

## 数据源
12 个权威新闻站首页: AP / Reuters / BBC / NYT / Guardian / SCMP / Al Jazeera / France 24 / DW / Nikkei Asia

## 文件
- `raw-YYYYMMDD-HHMM.json` — 12 站首页 raw extract(总 ~76 万字)
- `events-YYYYMMDD-HHMM.json` — 事件归类 + 跨源 cross-check + 重要度排序
- `world-YYYYMMDD-HHMM.json` — 旧版 Tavily search 拼凑(过期,可不看)

## 方法
1. Tavily `extract` API 拿 12 站首页 server-render HTML
2. agent (Ciao) 读 JSON 做关键词桶分类(伊朗 / 俄乌 / 以黎 / 美国政治 / AI / Ebola / 教皇 / 油价 ...)
3. 跨多源报道的事件 = 重要事件
4. 严格 word-boundary regex(避免假命中,例 "ai" 误中 "main"/"wait")
5. 时效过滤:NYT URL 路径 `/2026/06/07/` + Reuters ISO 日期 + AP/BBC "hours ago" markers

## 已知坑
- Tavily `extract` 拿的是站首页 server-render 内容,真实但需自己整理
- Tavily `search` news topic 不一定 24h 内,可能混过去 1 周
- AI answer 会编造事件细节(胡诌日期/数字),**不能直接信**
- "整理事实" = 跨多源交叉验证 + 时效过滤 + 事件归类,这是 **agent** 的工作,Tavily 不帮你做

## 工具
- 由 Ciao (OpenClaw assistant) 自动维护
- 抓取:Tavily Extract API
- 整理:Llama 4 (agent reasoning)
- 推送:`gh` CLI with PAT
