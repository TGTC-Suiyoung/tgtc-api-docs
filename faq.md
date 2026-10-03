# FAQ

The questions every developer hits in the first week. Contract-grade answers.

---

## Q: My key leaked. What now?

DM the Bot → **My API** → **Rotate key**. The old key dies immediately (`401`); your balance
is preserved. Store the key server-side only.

## Q: When does my balance reset?

It doesn't. Credits are **long-lived, never reset across days**. Top up when empty
(10U = 10,000 credits, SpaceDog +10%).

## Q: Why do some fields return `null`?

Data is missing or temporarily unavailable (e.g. `price_rpc` is `null` for tokens without a
PancakeSwap V2 pool). **Field names never change — `null` is not an error**, filter for it
explicitly.

## Q: `fields` vs `categories`?

- `categories` buys a whole data product and returns all its fields.
- `fields` prunes the response to the fields you name.
- Both deduct the same real fetches (the base layer always counts) — pruning saves
  **traffic, not credits**.

## Q: Why does a single-field request still cost credits?

The aggregation `basic` layer is one real upstream call in every request — even
`fields:["price"]`. Credit = actual fetches, transparent and auditable.

## Q: What does 429 mean?

Your **balance is exhausted** — not a rate limit. Top up and it recovers automatically.
The server's burst buffer is a separate queue, never an error you must retry.

## Q: Does the CA need to be lowercase?

No. `0x` + 40 hex in either case is accepted; normalized internally.

## Q: Which chains are supported?

**Only BSC** (`chain` = `bsc`, the default). More chains are on the roadmap.

## Q: Are cards / reports investment advice?

No. The service aggregates on-chain and X data with a fixed disclaimer; **you decide**.
RPC verification is a second source of truth, not an audit guarantee.

## Q: How do I check my balance and call history?

Every successful response carries `X-RateLimit-Remaining` (credits left). Full balance and
top-up live in the Bot's **My API**; call-log reconciliation can be exported via DM.

---

## 中文说明 · 常见问题

### Q：Key 丢了 / 泄露了怎么办？

私聊 Bot →「🔑 我的 API」→「🔄 轮换 Key」，旧 Key 立即作废（`401`），余额保留。Key 务必只放服务端。

### Q：额度什么时候重置？

**不重置**。次数长期有效、跨天不清零；用完充值（10U = 10,000 次起，SpaceDog 支付 +10%）。

### Q：为什么有些字段返回 null？

数据缺失或暂不可用（如无 PancakeSwap V2 池时 `price_rpc` 为 null）。**字段名恒定，null 不代表异常**，请显式过滤。

### Q：`fields` 和 `categories` 有什么区别？

`categories` 按数据产品整包购买、返回全字段；`fields` 按字段裁剪响应。两者扣次相同（基础层恒计入）——**裁剪省流量、不省扣次**。

### Q：只请求一个字段为什么也扣次？

聚合 `basic` 层是每次请求的必要上游调用，即使 `fields:["price"]` 也扣 1 次——扣次严格等于实际获取次数，透明可对账。

### Q：429 是什么？

**余额用完**（不是限流）——充值后自动恢复。服务端突发缓冲是独立排队，永远不会作为错误返回、无需重试。

### Q：CA 必须小写吗？

不用。接受 `0x` 开头 40 位十六进制（大小写均可），内部统一归一化。

### Q：支持哪些链？

**仅 BSC**（`chain` 缺省即 bsc）。更多链在规划中。

### Q：返回结果是投资建议吗？

不是。服务仅聚合链上与 X 数据，自带固定免责声明——**决策由你**。RPC 核验是独立第二数据源，不是审计保证。

### Q：怎么看余额和调用记录？

每次成功响应头 `X-RateLimit-Remaining` 即剩余次数；完整余额与充值在 Bot「🔑 我的 API」；需要调用流水对账可私聊导出。
