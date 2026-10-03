# Translate API

AI translation / summarization for tweets — faithful Chinese translation or a concise
Chinese key-point summary. Same key and balance as every other endpoint.

**Endpoint:** `POST https://www.tgtcbot.com/api/v1/translate/{action}`
**Actions:** `translate` · `summarize` · **3 credits each** · cache hits (300s) are free · Chinese input free

---

## Request

```json
{
  "text": "Any-language tweet text (1–2000 chars)"
}
```

| Param | Type | Required | Notes |
|---|---|---|---|
| `text` | string | yes | 1–2000 chars; over-length returns `422` without deduction |

## Actions

| Action | Purpose | Deduction | Notes |
|---|---|---|---|
| `translate` | Any language → Chinese, faithful translation | 3 | Chinese input returns as-is, deducts **0** (`ai_called: false`) |
| `summarize` | Concise Chinese key-point summary (≤80 chars) | 3 | Best for long threads / reports |

## Response

```json
{
  "action": "translate",
  "translated_text": "忠实的中文译文…",
  "ai_called": true,
  "disclaimer": "本内容由 AI 生成…"
}
```

For `summarize` the key field is `summary`. Empty text / over-length / unknown action →
`422` (never deducts).

---

## 中文说明 · 翻译 API

推文翻译 / 摘要：任意语言忠实译成中文，或长文提炼中文要点。与其它端点同一把 Key、同一份余额。

**端点**：`POST https://www.tgtcbot.com/api/v1/translate/{action}`
**动作**：`translate` · `summarize` · 各 **3 次** · 300s 缓存命中不扣次 · 中文输入免调

### 请求

```json
{
  "text": "任意语言推文文本（1–2000 字符）"
}
```

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `text` | string | 是 | 1–2000 字符；超长返回 `422` 不扣次 |

### 动作

| 动作 | 用途 | 扣次 | 说明 |
|---|---|---|---|
| `translate` | 任意语言 → 中文，忠实翻译 | 3 | 输入已含中文时原样返回、扣 **0**（`ai_called: false`） |
| `summarize` | 长文中文要点摘要（≤80 字） | 3 | 适合长推文 / 长报告 |

### 响应

```json
{
  "action": "translate",
  "translated_text": "忠实的中文译文…",
  "ai_called": true,
  "disclaimer": "本内容由 AI 生成…"
}
```

`summarize` 的关键字段为 `summary`。空文本 / 超长 / 未知 action → `422`（永不扣次）。
