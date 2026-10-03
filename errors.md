# Errors &amp; Retry

Every TGTC endpoint returns the same error contract. **HTTP status is the contract** —
the body is a `{"detail": "..."}` message for humans.

---

## Status codes

| Status | Meaning | Deducted? | How to react |
|---|---|---|---|
| `200` | OK | yes (see [billing](billing.md)) | — |
| `400` | Malformed request (bad CA format, unsupported chain) | no | Fix the input |
| `401` | Missing or invalid `X-API-Key` | no | Check the header; get a key from [@TG_TC_BOT](https://t.me/TG_TC_BOT) |
| `404` | Not found — token doesn't exist / deleted tweet / unknown user / not a token | no | The resource doesn't exist; verify the address |
| `422` | Validation failure — missing param, unknown `action` / `field`, over-length text, category conflict | **no** | Fix the request |
| `429` | **Insufficient balance** (not rate limiting) | no | Top up; recovers automatically |
| `500` | Upstream data fetch failed (timeout / upstream error) | **yes** — not refunded | Retry with backoff |

## Two different "429s" — don't confuse them

- **API `429` = your balance is exhausted.** Top up and it recovers automatically.
- **Server burst buffer is not an error.** The service queues high concurrency internally;
  clients never receive it as a failure and must not retry it.

## Retry with exponential backoff

Recommended: after `429` (balance) or `500`, wait `1s → 2s → 4s` (cap 30s), at most **3
retries**. A different pagination `cursor` is a different request — never cache-misses
retried blindly; just re-issue with the new cursor.

## Headers every response carries

| Header | Meaning |
|---|---|
| `X-Cache: HIT` | Cache hit — **0 credits** deducted for this call |
| `X-RateLimit-Remaining` | Credits left after this call |
| `X-RateLimit-Used` | Credits used by this call (cache hits: `0`) |

---

## 中文说明 · 错误码与重试

所有 TGTC 端点共用同一套错误契约。**HTTP 状态码就是契约**，响应体 `{"detail": "..."}` 仅供人读。

### 状态码表

| 状态 | 含义 | 扣次？ | 处理 |
|---|---|---|---|
| `200` | 成功 | 是（见 [billing](billing.md)） | — |
| `400` | 请求格式错误（CA 格式 / chain 不支持） | 否 | 修正入参 |
| `401` | Key 缺失或无效 | 否 | 检查请求头；去 [@TG_TC_BOT](https://t.me/TG_TC_BOT) 获取 |
| `404` | 数据不存在（代币不存在 / 推文已删 / 用户不存在 / 非代币） | 否 | 核对地址是否真实存在 |
| `422` | 参数校验失败（缺参 / 未知 action·field / 超长 / 类别冲突） | **否** | 修正请求 |
| `429` | **余额不足**（不是限流） | 否 | 充值后自动恢复 |
| `500` | 上游数据获取失败（超时 / 上游异常） | **是**——已扣不退 | 按退避重试 |

### 两种「429」别搞混

- **API `429` = 你的余额用完了**：充值后自动恢复。
- **服务端突发缓冲不是错误**：服务内部对高并发排队处理，客户端永远不会收到这个失败、也无需重试。

### 指数退避重试

建议：遇到 `429`（余额）或 `500` 后等待 `1s → 2s → 4s`（上限 30s），最多 **3 次**。翻页换
`cursor` 是新的请求——不要盲目重试旧参数，直接用新 cursor 重发。

### 每次响应都带的头

| 头 | 含义 |
|---|---|
| `X-Cache: HIT` | 命中缓存——本次扣 **0 次** |
| `X-RateLimit-Remaining` | 本次调用后剩余次数 |
| `X-RateLimit-Used` | 本次消耗次数（缓存命中为 `0`） |
