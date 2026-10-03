# Billing — Master Deduction Table

**Every number on this page is what the service actually deducts.** One key, one balance,
across the Bot and every data API. Cache hits always deduct **0**; validation failures
(422) deduct **0**; data-fetch failures (500) are **not refunded** (the upstream channel was
already consumed); insufficient balance returns **429**.

---

## Deduction master table

<table style="width:100%;border-collapse:collapse">
  <thead>
    <tr><th style="text-align:left;padding:8px 16px;white-space:nowrap;width:40%">Endpoint</th><th style="text-align:left;padding:8px 16px;white-space:nowrap;width:9%">Per call</th><th style="text-align:left;padding:8px 16px;white-space:nowrap;width:11%">Cache hit</th><th style="text-align:left;padding:8px 16px;white-space:nowrap;width:40%">Notes</th></tr>
  </thead>
  <tbody>
    <tr><td style="padding:8px 16px;white-space:nowrap">Token aggregation <code>POST /api/v1/aggregation/token</code></td><td style="padding:8px 16px;white-space:nowrap"><code>2+</code></td><td style="padding:8px 16px;white-space:nowrap">0 (10s)</td><td style="padding:8px 16px;white-space:nowrap">basic 1 + security 1; structure free</td></tr>
    <tr><td style="padding:8px 16px;white-space:nowrap">Listings <code>POST /api/v1/token/trending</code> · <code>hot</code></td><td style="padding:8px 16px;white-space:nowrap"><code>1</code></td><td style="padding:8px 16px;white-space:nowrap">0 (120s)</td><td style="padding:8px 16px;white-space:nowrap">—</td></tr>
    <tr><td style="padding:8px 16px;white-space:nowrap">Trade stream <code>POST /api/v1/track/trades</code></td><td style="padding:8px 16px;white-space:nowrap"><code>1</code></td><td style="padding:8px 16px;white-space:nowrap">0 (15s)</td><td style="padding:8px 16px;white-space:nowrap">smartmoney / kol</td></tr>
    <tr><td style="padding:8px 16px;white-space:nowrap">Signal stream <code>POST /api/v1/market/signals</code></td><td style="padding:8px 16px;white-space:nowrap"><code>1</code></td><td style="padding:8px 16px;white-space:nowrap">0 (30s)</td><td style="padding:8px 16px;white-space:nowrap">21 signal types</td></tr>
    <tr><td style="padding:8px 16px;white-space:nowrap">Wallet <code>POST /api/v1/wallet/{action}</code></td><td style="padding:8px 16px;white-space:nowrap"><code>2 / 1</code></td><td style="padding:8px 16px;white-space:nowrap">0 (30s)</td><td style="padding:8px 16px;white-space:nowrap">profile 2; others 1</td></tr>
    <tr><td style="padding:8px 16px;white-space:nowrap">CA sentiment <code>POST /api/v1/twitter/sentiment</code></td><td style="padding:8px 16px;white-space:nowrap"><code>10</code></td><td style="padding:8px 16px;white-space:nowrap">0 (600s)</td><td style="padding:8px 16px;white-space:nowrap">search + AI</td></tr>
    <tr><td style="padding:8px 16px;white-space:nowrap">Twitter <code>POST /api/v1/twitter/{action}</code></td><td style="padding:8px 16px;white-space:nowrap"><code>5</code></td><td style="padding:8px 16px;white-space:nowrap">0 (120s)</td><td style="padding:8px 16px;white-space:nowrap">12 endpoints; pagination 5/page</td></tr>
    <tr><td style="padding:8px 16px;white-space:nowrap">Translate <code>POST /api/v1/translate/{action}</code></td><td style="padding:8px 16px;white-space:nowrap"><code>3</code></td><td style="padding:8px 16px;white-space:nowrap">0 (300s)</td><td style="padding:8px 16px;white-space:nowrap">Chinese input free</td></tr>
    <tr><td style="padding:8px 16px;white-space:nowrap">Bot <code>/checkX</code> · <code>/check</code></td><td style="padding:8px 16px;white-space:nowrap"><code>4+</code></td><td style="padding:8px 16px;white-space:nowrap">—</td><td style="padding:8px 16px;white-space:nowrap">on-chain 4; +twitter/translate</td></tr>
  </tbody>
</table>

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

<table style="width:100%;border-collapse:collapse">
  <thead>
    <tr><th style="text-align:left;padding:8px 16px;white-space:nowrap;width:40%">接口</th><th style="text-align:left;padding:8px 16px;white-space:nowrap;width:9%">每次扣次</th><th style="text-align:left;padding:8px 16px;white-space:nowrap;width:11%">缓存命中</th><th style="text-align:left;padding:8px 16px;white-space:nowrap;width:40%">说明</th></tr>
  </thead>
  <tbody>
    <tr><td style="padding:8px 16px;white-space:nowrap">代币聚合 <code>POST /api/v1/aggregation/token</code></td><td style="padding:8px 16px;white-space:nowrap"><code>2 起</code></td><td style="padding:8px 16px;white-space:nowrap">10s 内 0 次</td><td style="padding:8px 16px;white-space:nowrap">basic 1 + security 1；structure 免费</td></tr>
    <tr><td style="padding:8px 16px;white-space:nowrap">榜单 <code>POST /api/v1/token/trending</code> · <code>hot</code></td><td style="padding:8px 16px;white-space:nowrap"><code>1</code></td><td style="padding:8px 16px;white-space:nowrap">120s 内 0 次</td><td style="padding:8px 16px;white-space:nowrap">—</td></tr>
    <tr><td style="padding:8px 16px;white-space:nowrap">交易流 <code>POST /api/v1/track/trades</code></td><td style="padding:8px 16px;white-space:nowrap"><code>1</code></td><td style="padding:8px 16px;white-space:nowrap">15s 内 0 次</td><td style="padding:8px 16px;white-space:nowrap">smartmoney / kol</td></tr>
    <tr><td style="padding:8px 16px;white-space:nowrap">信号流 <code>POST /api/v1/market/signals</code></td><td style="padding:8px 16px;white-space:nowrap"><code>1</code></td><td style="padding:8px 16px;white-space:nowrap">30s 内 0 次</td><td style="padding:8px 16px;white-space:nowrap">21 种信号</td></tr>
    <tr><td style="padding:8px 16px;white-space:nowrap">钱包 <code>POST /api/v1/wallet/{action}</code></td><td style="padding:8px 16px;white-space:nowrap"><code>2 / 1</code></td><td style="padding:8px 16px;white-space:nowrap">30s 内 0 次</td><td style="padding:8px 16px;white-space:nowrap">profile 2；其余各 1</td></tr>
    <tr><td style="padding:8px 16px;white-space:nowrap">CA 舆情 <code>POST /api/v1/twitter/sentiment</code></td><td style="padding:8px 16px;white-space:nowrap"><code>10</code></td><td style="padding:8px 16px;white-space:nowrap">600s 内 0 次</td><td style="padding:8px 16px;white-space:nowrap">搜索 + AI 分析</td></tr>
    <tr><td style="padding:8px 16px;white-space:nowrap">推特 <code>POST /api/v1/twitter/{action}</code></td><td style="padding:8px 16px;white-space:nowrap"><code>5</code></td><td style="padding:8px 16px;white-space:nowrap">120s 内 0 次</td><td style="padding:8px 16px;white-space:nowrap">12 端点统一；翻页每页 5</td></tr>
    <tr><td style="padding:8px 16px;white-space:nowrap">翻译 <code>POST /api/v1/translate/{action}</code></td><td style="padding:8px 16px;white-space:nowrap"><code>3</code></td><td style="padding:8px 16px;white-space:nowrap">300s 内 0 次</td><td style="padding:8px 16px;white-space:nowrap">中文输入免调</td></tr>
    <tr><td style="padding:8px 16px;white-space:nowrap">Bot <code>/checkX</code> · <code>/check</code></td><td style="padding:8px 16px;white-space:nowrap"><code>4 起</code></td><td style="padding:8px 16px;white-space:nowrap">—</td><td style="padding:8px 16px;white-space:nowrap">链上 4；含推特/翻译按实际叠加</td></tr>
  </tbody>
</table>

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
