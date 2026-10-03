# CA Sentiment API

One-click X sentiment scan for a BSC contract: search X mentions of the CA, rank by views,
let the AI summarize — with a **data-driven heat rating** that the AI cannot override.

**Endpoint:** `POST https://www.tgtcbot.com/api/v1/twitter/sentiment`
**Billing:** 10 credits per scan · cache hits (600s) are free · 422 never deducts

---

## Request

```json
{
  "ca": "0xbbc9565a44036007830c10b41d59ce55f3847777",
  "chain": "bsc"
}
```

<table style="width:100%;border-collapse:collapse">
  <thead>
    <tr><th style="text-align:left;padding:6px 14px">Param</th><th style="text-align:left;padding:6px 14px">Type</th><th style="text-align:left;padding:6px 14px">Required</th><th style="text-align:left;padding:6px 14px">Notes</th></tr>
  </thead>
  <tbody>
    <tr><td style="padding:6px 14px"><code>ca</code></td><td style="padding:6px 14px">string</td><td style="padding:6px 14px">yes</td><td style="padding:6px 14px">Contract address, <code>0x</code> + 40 hex (lowercase accepted)</td></tr>
    <tr><td style="padding:6px 14px"><code>chain</code></td><td style="padding:6px 14px">string</td><td style="padding:6px 14px">no</td><td style="padding:6px 14px">Only <code>bsc</code> (default <code>bsc</code>)</td></tr>
  </tbody>
</table>

Errors: bad CA format → `400` · not a token / unavailable → `404` · insufficient balance → `429`.

## Response

```json
{
  "ca": "0xbbc9565a...",
  "chain": "bsc",
  "tweets_count": 20,
  "max_views": 3414,
  "total_views": 18715,
  "top_tweets": [
    { "views": 3414, "likes": 23, "text": "clean text...", "url": "https://x.com/user/status/123" }
  ],
  "ai_text": "热度评级：高\n一句话理由：...\n\n舆情摘要：...\n\n关键信号：\n1. ...\n2. ...\n3. ...",
  "disclaimer": "本API仅提供数据聚合服务，不构成任何投资建议。..."
}
```

<table style="width:100%;border-collapse:collapse">
  <thead>
    <tr><th style="text-align:left;padding:6px 14px">Field</th><th style="text-align:left;padding:6px 14px">Type</th><th style="text-align:left;padding:6px 14px">Notes</th></tr>
  </thead>
  <tbody>
    <tr><td style="padding:6px 14px"><code>tweets_count</code></td><td style="padding:6px 14px">int</td><td style="padding:6px 14px">Mentions found by the search (up to 50 pulled)</td></tr>
    <tr><td style="padding:6px 14px"><code>max_views</code> / <code>total_views</code></td><td style="padding:6px 14px">int</td><td style="padding:6px 14px">Top-10 tweet views</td></tr>
    <tr><td style="padding:6px 14px"><code>top_tweets</code></td><td style="padding:6px 14px">array</td><td style="padding:6px 14px">Up to 5 items; <code>text</code> is clean (CA and links stripped server-side), <code>url</code> links to the original X post</td></tr>
    <tr><td style="padding:6px 14px"><code>ai_text</code></td><td style="padding:6px 14px">string</td><td style="padding:6px 14px">Heat rating + one-line reason + ≤80-char summary + up to 3 key signals</td></tr>
    <tr><td style="padding:6px 14px"><code>data_delay_sec</code></td><td style="padding:6px 14px">int</td><td style="padding:6px 14px">Data staleness in seconds (0 = fresh) — see <a href="sla.md">sla.md</a></td></tr>
    <tr><td style="padding:6px 14px"><code>degraded_sources</code></td><td style="padding:6px 14px">string[]</td><td style="padding:6px 14px">Sources down while assembling (<code>twitter</code> / <code>aggregate</code> / <code>ai</code>)</td></tr>
  </tbody>
</table>

## Heat rating — dual-channel thresholds, take the higher

<table style="width:100%;border-collapse:collapse">
  <thead>
    <tr><th style="text-align:left;padding:6px 14px"></th><th style="text-align:left;padding:6px 14px">High 🔴</th><th style="text-align:left;padding:6px 14px">Mid 🟡</th><th style="text-align:left;padding:6px 14px">Low ⚪</th></tr>
  </thead>
  <tbody>
    <tr><td style="padding:6px 14px"><b>X-tweet channel</b></td><td style="padding:6px 14px">≥10 mentions AND (15K+ total views OR 5K+ top view)</td><td style="padding:6px 14px">≥15 mentions, OR (≥5 mentions AND (5K+ total views OR 1.5K+ top view))</td><td style="padding:6px 14px">otherwise</td></tr>
    <tr><td style="padding:6px 14px"><b>On-chain channel</b></td><td style="padding:6px 14px">24h volume ≥ $5M, OR (market cap ≥ $5M AND 3K+ holders)</td><td style="padding:6px 14px">24h volume ≥ $500K, OR market cap ≥ $500K, OR 1K+ holders</td><td style="padding:6px 14px">otherwise</td></tr>
  </tbody>
</table>

The higher of the two channels wins — a hot token with low tweet reach is no longer
misjudged. The AI writes only the reason / summary / signals; the rating is fixed by the
data. Thresholds are configurable server-side.

---

## 中文说明 · CA 舆情端点

一键生成某 BSC 合约的 X 舆情速览：按 CA 搜索提及推文 → 阅读量 top10 → AI 分析，热度评级
由数据硬判定、AI 无权改级。

**端点**：`POST https://www.tgtcbot.com/api/v1/twitter/sentiment`
**计费**：每次 10 次 · 600s 缓存命中不扣次 · 422 不扣

### 请求

```json
{
  "ca": "0xbbc9565a44036007830c10b41d59ce55f3847777",
  "chain": "bsc"
}
```

<table style="width:100%;border-collapse:collapse">
  <thead>
    <tr><th style="text-align:left;padding:6px 14px">参数</th><th style="text-align:left;padding:6px 14px">类型</th><th style="text-align:left;padding:6px 14px">必填</th><th style="text-align:left;padding:6px 14px">说明</th></tr>
  </thead>
  <tbody>
    <tr><td style="padding:6px 14px"><code>ca</code></td><td style="padding:6px 14px">string</td><td style="padding:6px 14px">是</td><td style="padding:6px 14px">合约地址，<code>0x</code> + 40 位十六进制</td></tr>
    <tr><td style="padding:6px 14px"><code>chain</code></td><td style="padding:6px 14px">string</td><td style="padding:6px 14px">否</td><td style="padding:6px 14px">仅 <code>bsc</code>（缺省即 bsc）</td></tr>
  </tbody>
</table>

错误：CA 格式错误 → `400` · 非代币或暂不可用 → `404` · 余额不足 → `429`。

### 响应字段

<table style="width:100%;border-collapse:collapse">
  <thead>
    <tr><th style="text-align:left;padding:6px 14px">字段</th><th style="text-align:left;padding:6px 14px">类型</th><th style="text-align:left;padding:6px 14px">说明</th></tr>
  </thead>
  <tbody>
    <tr><td style="padding:6px 14px"><code>tweets_count</code></td><td style="padding:6px 14px">int</td><td style="padding:6px 14px">搜索到的提及推文数（最多拉取 50 条）</td></tr>
    <tr><td style="padding:6px 14px"><code>max_views</code> / <code>total_views</code></td><td style="padding:6px 14px">int</td><td style="padding:6px 14px">top10 推文的最高 / 总阅读量</td></tr>
    <tr><td style="padding:6px 14px"><code>top_tweets</code></td><td style="padding:6px 14px">array</td><td style="padding:6px 14px">最多 5 条；<code>text</code> 已净化（服务端去除 CA 与链接），<code>url</code> 直达 X 原帖</td></tr>
    <tr><td style="padding:6px 14px"><code>ai_text</code></td><td style="padding:6px 14px">string</td><td style="padding:6px 14px">热度评级 + 一句话理由 + ≤80 字摘要 + 至多 3 条关键信号</td></tr>
    <tr><td style="padding:6px 14px"><code>data_delay_sec</code></td><td style="padding:6px 14px">int</td><td style="padding:6px 14px">数据陈旧秒数（0 = 新鲜）——见 <a href="sla.md">sla.md</a></td></tr>
    <tr><td style="padding:6px 14px"><code>degraded_sources</code></td><td style="padding:6px 14px">string[]</td><td style="padding:6px 14px">拼装时不可用的源（<code>twitter</code> / <code>aggregate</code> / <code>ai</code>）</td></tr>
  </tbody>
</table>

### 热度评级 —— 双通道阈值，取高

<table style="width:100%;border-collapse:collapse">
  <thead>
    <tr><th style="text-align:left;padding:6px 14px"></th><th style="text-align:left;padding:6px 14px">高 🔴</th><th style="text-align:left;padding:6px 14px">中 🟡</th><th style="text-align:left;padding:6px 14px">低 ⚪</th></tr>
  </thead>
  <tbody>
    <tr><td style="padding:6px 14px"><b>X 推文通道</b></td><td style="padding:6px 14px">提及 ≥10 且（总阅读 ≥15K 或单条 ≥5K）</td><td style="padding:6px 14px">提及 ≥15，或（提及 ≥5 且（总阅读 ≥5K 或单条 ≥1.5K））</td><td style="padding:6px 14px">其余</td></tr>
    <tr><td style="padding:6px 14px"><b>链上通道</b></td><td style="padding:6px 14px">24h 成交 ≥$5M，或（市值 ≥$5M 且持有人 ≥3K）</td><td style="padding:6px 14px">成交 ≥$500K，或市值 ≥$500K，或持有人 ≥1K</td><td style="padding:6px 14px">其余</td></tr>
  </tbody>
</table>

两通道取高——链上热但推文阅读量低的币不会被误判。AI 只写理由 / 摘要 / 信号，评级由数据
锁定；阈值可在服务端配置调整。
