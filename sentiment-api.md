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

| Param | Type | Required | Notes |
|---|---|---|---|
| `ca` | string | yes | Contract address, `0x` + 40 hex (lowercase accepted) |
| `chain` | string | no | Only `bsc` (default `bsc`) |

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

| Field | Type | Notes |
|---|---|---|
| `tweets_count` | int | Mentions found by the search (up to 50 pulled) |
| `max_views` / `total_views` | int | Top-10 tweet views |
| `top_tweets` | array | Up to 5 items; `text` is clean (CA and links stripped server-side), `url` links to the original X post |
| `ai_text` | string | Heat rating + one-line reason + ≤80-char summary + up to 3 key signals |

## Heat rating — dual-channel thresholds, take the higher

| | High 🔴 | Mid 🟡 | Low ⚪ |
|---|---|---|---|
| **X-tweet channel** | ≥10 mentions AND (15K+ total views OR 5K+ top view) | ≥15 mentions, OR (≥5 mentions AND (5K+ total views OR 1.5K+ top view)) | otherwise |
| **On-chain channel** | 24h volume ≥ $5M, OR (market cap ≥ $5M AND 3K+ holders) | 24h volume ≥ $500K, OR market cap ≥ $500K, OR 1K+ holders | otherwise |

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

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `ca` | string | 是 | 合约地址，`0x` + 40 位十六进制 |
| `chain` | string | 否 | 仅 `bsc`（缺省即 bsc） |

错误：CA 格式错误 → `400` · 非代币或暂不可用 → `404` · 余额不足 → `429`。

### 响应字段

| 字段 | 类型 | 说明 |
|---|---|---|
| `tweets_count` | int | 搜索到的提及推文数（最多拉取 50 条） |
| `max_views` / `total_views` | int | top10 推文的最高 / 总阅读量 |
| `top_tweets` | array | 最多 5 条；`text` 已净化（服务端去除 CA 与链接），`url` 直达 X 原帖 |
| `ai_text` | string | 热度评级 + 一句话理由 + ≤80 字摘要 + 至多 3 条关键信号 |

### 热度评级 —— 双通道阈值，取高

| | 高 🔴 | 中 🟡 | 低 ⚪ |
|---|---|---|---|
| **X 推文通道** | 提及 ≥10 且（总阅读 ≥15K 或单条 ≥5K） | 提及 ≥15，或（提及 ≥5 且（总阅读 ≥5K 或单条 ≥1.5K）） | 其余 |
| **链上通道** | 24h 成交 ≥$5M，或（市值 ≥$5M 且持有人 ≥3K） | 成交 ≥$500K，或市值 ≥$500K，或持有人 ≥1K | 其余 |

两通道取高——链上热但推文阅读量低的币不会被误判。AI 只写理由 / 摘要 / 信号，评级由数据
锁定；阈值可在服务端配置调整。
