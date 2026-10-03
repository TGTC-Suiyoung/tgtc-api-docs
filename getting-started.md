# Getting Started

Five minutes from zero to your first API call.

---

## 1. Get a key

1. DM [@TG_TC_BOT](https://t.me/TG_TC_BOT) on Telegram.
2. Top up (10U = 10,000 credits · 100U = 120,000 · 200U = 300,000; SpaceDog payments +10%).
3. Open **My API** → generate a `sk_live_...` key.
4. New users get **500 free credits** at signup — try everything before paying.

## 2. Make your first call

```bash
curl -s -X POST https://www.tgtcbot.com/api/v1/aggregation/token \
  -H "Content-Type: application/json" -H "X-API-Key: sk_live_..." \
  -d '{"ca":"0xbbc9565a44036007830c10b41d59ce55f3847777","categories":["basic","security"]}'
```

Python:

```python
import requests
r = requests.post(
    "https://www.tgtcbot.com/api/v1/aggregation/token",
    headers={"X-API-Key": "sk_live_..."},
    json={"ca": "0xbbc9565a44036007830c10b41d59ce55f3847777",
          "categories": ["basic", "security"]})
d = r.json()
print(d["price"], d["mcap"], d["honeypot"])
print("剩余次数:", r.headers.get("X-RateLimit-Remaining"))
```

JavaScript:

```js
const r = await fetch("https://www.tgtcbot.com/api/v1/aggregation/token", {
  method: "POST",
  headers: { "Content-Type": "application/json", "X-API-Key": "sk_live_..." },
  body: JSON.stringify({
    ca: "0xbbc9565a44036007830c10b41d59ce55f3847777",
    categories: ["basic", "security"],
  }),
});
const d = await r.json();
console.log(d.price, d.mcap, d.honeypot);
```

## 3. Read the response headers

| Header | Meaning |
|---|---|
| `X-Cache: HIT` | Cache hit — this call deducted **0** credits |
| `X-RateLimit-Remaining` | Credits left |
| `X-RateLimit-Used` | Credits used by this call |

---

## 中文说明 · 快速开始

五分钟从零到第一次调用。

### 1. 获取 Key

1. Telegram 私聊 [@TG_TC_BOT](https://t.me/TG_TC_BOT)
2. 充值（10U = 10,000 次 · 100U = 120,000 · 200U = 300,000；SpaceDog 支付 +10%）
3. 「🔑 我的 API」生成 `sk_live_...` Key
4. 新用户注册赠送 **500 次**——先免费试完所有端点再付费

### 2. 第一次调用

```bash
curl -s -X POST https://www.tgtcbot.com/api/v1/aggregation/token \
  -H "Content-Type: application/json" -H "X-API-Key: sk_live_..." \
  -d '{"ca":"0xbbc9565a44036007830c10b41d59ce55f3847777","categories":["basic","security"]}'
```

Python：

```python
import requests
r = requests.post(
    "https://www.tgtcbot.com/api/v1/aggregation/token",
    headers={"X-API-Key": "sk_live_..."},
    json={"ca": "0xbbc9565a44036007830c10b41d59ce55f3847777",
          "categories": ["basic", "security"]})
d = r.json()
print(d["price"], d["mcap"], d["honeypot"])
print("剩余次数:", r.headers.get("X-RateLimit-Remaining"))
```

JavaScript：

```js
const r = await fetch("https://www.tgtcbot.com/api/v1/aggregation/token", {
  method: "POST",
  headers: { "Content-Type": "application/json", "X-API-Key": "sk_live_..." },
  body: JSON.stringify({
    ca: "0xbbc9565a44036007830c10b41d59ce55f3847777",
    categories: ["basic", "security"],
  }),
});
const d = await r.json();
console.log(d.price, d.mcap, d.honeypot);
```

### 3. 读响应头

| 头 | 含义 |
|---|---|
| `X-Cache: HIT` | 命中缓存——本次扣 **0 次** |
| `X-RateLimit-Remaining` | 剩余次数 |
| `X-RateLimit-Used` | 本次消耗次数 |
