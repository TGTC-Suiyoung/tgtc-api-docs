# Authentication

Every TGTC endpoint uses the same key, the same balance, and the same auth header.

---

## The header

Send your key on every request:

```
X-API-Key: sk_live_...
```

| Case | Status | Deducted? |
|---|---|---|
| Missing / invalid key | `401` | no |
| Key disabled or rotated | `401` | no |
| Insufficient balance | `429` | no — top up to recover |

## Where the key comes from

DM [@TG_TC_BOT](https://t.me/TG_TC_BOT) → top up → **My API** → generate `sk_live_...`.
The key is tied to your balance; it works across the Bot and every data API — one key, one balance.

## Top-up packs

| Pack | Credits | With SpaceDog payment (+10%) |
|---|---|---|
| 10U | 10,000 | 11,000 |
| 100U | 120,000 | 132,000 |
| 200U | 300,000 | 330,000 |

New users get **500 free credits** at signup. Credits are long-lived and never expire across days.

## Key management

- **Regenerate** in the Bot's My API panel anytime; the old key stops working immediately (`401`).
- **Keep it server-side** — a key in client-side code can be taken and burned by anyone.
- Never share your key; the balance is consumed by whoever holds it.

---

## 中文说明 · 鉴权

所有 TGTC 端点使用同一把 Key、同一份余额、同一个鉴权头。

### 请求头

每次请求携带：

```
X-API-Key: sk_live_...
```

| 情况 | 状态 | 扣次？ |
|---|---|---|
| Key 缺失 / 无效 | `401` | 否 |
| Key 已停用 / 已轮换 | `401` | 否 |
| 余额不足 | `429` | 否——充值后自动恢复 |

### Key 从哪里来

Telegram 私聊 [@TG_TC_BOT](https://t.me/TG_TC_BOT) → 充值 → 「🔑 我的 API」生成 `sk_live_...`。
Key 绑定你的余额，Bot 与全部数据 API 通用——一把 Key、一份余额。

### 充值包

| 档位 | 次数 | SpaceDog 支付（+10%） |
|---|---|---|
| 10U | 10,000 | 11,000 |
| 100U | 120,000 | 132,000 |
| 200U | 300,000 | 330,000 |

新用户注册赠送 **500 次**；次数长期有效、跨天不清零。

### Key 管理

- **随时可重新生成**：Bot「我的 API」一键轮换，旧 Key 立即失效（`401`）。
- **务必放服务端**：放在前端/客户端代码里的 Key 会被任何人拿去消耗你的余额。
- 切勿分享 Key——谁拿到谁花你的余额。
