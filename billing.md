# Billing — Master Deduction Table

**Every number on this page is what the service actually deducts.** One key, one balance,
across the Bot and every data API. Cache hits always deduct **0**; validation failures
(422) deduct **0**; data-fetch failures (500) are **not refunded** (the upstream channel was
already consumed); insufficient balance returns **429**.

---

## Deduction master table

| Endpoint | Per call | Cache hit | Notes |
|---|---|---|---|
| Token aggregation `POST /api/v1/aggregation/token` | `2+` | 0 (10s) | basic 1 + security 1; structure free |
| Listings `POST /api/v1/token/trending` · `hot` | `1` | 0 (120s) | — |
| Trade stream `POST /api/v1/track/trades` | `1` | 0 (15s) | smartmoney / kol |
| Signal stream `POST /api/v1/market/signals` | `1` | 0 (30s) | 21 signal types |
| Wallet `POST /api/v1/wallet/{action}` | `2 / 1` | 0 (30s) | profile 2; others 1 |
| CA sentiment `POST /api/v1/twitter/sentiment` | `10` | 0 (600s) | search + AI |
| Twitter `POST /api/v1/twitter/{action}` | `5` | 0 (120s) | 12 endpoints; pagination 5/page |
| Translate `POST /api/v1/translate/{action}` | `3` | 0 (300s) | Chinese input free |
| Bot `/checkX` · `/check` | `4+` | — | on-chain 4; +twitter/translate |

**Notes detail**

- **Token aggregation** — `holders` / `social` add +1 each, `traders` +2; field pruning never skips the base layer.
- **Twitter** — every pagination page bills 5 (a different `cursor` is a new request).
- **Translate** — Chinese input returns as-is and deducts 0 (`ai_called: false`).
- **Bot** — in-group `/checkX` bills the group owner; private `/check` is self-funded.

## Rules

1. **Cache-hit semantics** — same parameters within the TTL window return from cache with
   header `X-Cache: HIT` and deduct **0**. A different `cursor` (pagination) is a different
   request and never hits cache.
2. **422 never deducts** — missing params, unknown action, unknown field, over-length text,
   bad CA format.
3. **500 is not refunded** — the data channel was actually consumed; retry with backoff.
4. **Base layer always counts** — the aggregation `basic` layer is one upstream call in every
   request, even when you prune fields to `["price"]`. This is "credit = actual fetches".
5. **Balance is long-lived** — 10U = 10,000 credits · 100U = 120,000 · 200U = 300,000;
   SpaceDog payments get +10%; new users receive **500 free credits**.

---

## 中文说明 · 全接口扣费总表

**本页每个数字 = 服务实际扣次。** 一把 Key、一份余额，Bot 与全部数据 API 通用。缓存命中一律
扣 **0**；参数校验失败（422）不扣；数据获取失败（500）已扣不退（上游通道已产生消耗）；余额不足
返回 429。

### 扣费总表

| 接口 | 每次扣次 | 缓存命中 | 说明 |
|---|---|---|---|
| 代币聚合 `POST /api/v1/aggregation/token` | `2 起` | 10s 内 0 次 | basic 1 + security 1；structure 免费 |
| 榜单 `POST /api/v1/token/trending` · `hot` | `1` | 120s 内 0 次 | — |
| 交易流 `POST /api/v1/track/trades` | `1` | 15s 内 0 次 | smartmoney / kol |
| 信号流 `POST /api/v1/market/signals` | `1` | 30s 内 0 次 | 21 种信号 |
| 钱包 `POST /api/v1/wallet/{action}` | `2 / 1` | 30s 内 0 次 | profile 2；其余各 1 |
| CA 舆情 `POST /api/v1/twitter/sentiment` | `10` | 600s 内 0 次 | 搜索 + AI 分析 |
| 推特 `POST /api/v1/twitter/{action}` | `5` | 120s 内 0 次 | 12 端点统一；翻页每页 5 |
| 翻译 `POST /api/v1/translate/{action}` | `3` | 300s 内 0 次 | 中文输入免调 |
| Bot `/checkX` · `/check` | `4 起` | — | 链上 4；含推特/翻译按实际叠加 |

**说明细节**

- **代币聚合**——加 `holders` / `social` 各 +1、`traders` +2；字段裁剪不省基础层。
- **推特**——翻页每页独立计费 5（不同 `cursor` = 新请求）。
- **翻译**——中文输入原样返回、扣 0 次（`ai_called: false`）。
- **Bot**——群内 `/checkX` 扣群主；私聊 `/check` 个人自费。

### 规则

1. **缓存命中**：TTL 窗口内同参数命中缓存 → 头 `X-Cache: HIT`、扣 0 次；不同 `cursor`（翻页）视为不同请求、不命中缓存。
2. **422 永不扣**：缺参 / 未知 action / 未知字段 / 文本超长 / CA 格式错误。
3. **500 不退**：数据通道已产生实际消耗，请按退避重试。
4. **基础层恒扣**：聚合 `basic` 层是每次请求的必要上游调用，即使裁剪到 `fields:["price"]` 也扣 1 次——扣次严格等于实际获取次数。
5. **余额长期有效**：10U = 10,000 次 · 100U = 120,000 · 200U = 300,000；SpaceDog 支付 +10%；新用户注册赠送 **500 次**。
