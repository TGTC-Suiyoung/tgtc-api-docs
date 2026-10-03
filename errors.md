# Errors &amp; Retry

Every TGTC endpoint returns the same error contract. **HTTP status is the contract** —
the body is a `{"detail": "..."}` message for humans.

---

## Status codes

<table style="width:100%;border-collapse:collapse">
  <thead>
    <tr><th style="text-align:left;padding:6px 14px">Status</th><th style="text-align:left;padding:6px 14px">Meaning</th><th style="text-align:left;padding:6px 14px">Deducted?</th><th style="text-align:left;padding:6px 14px">How to react</th></tr>
  </thead>
  <tbody>
    <tr><td style="padding:6px 14px"><code>200</code></td><td style="padding:6px 14px">OK</td><td style="padding:6px 14px">yes (see <a href="billing.md">billing</a>)</td><td style="padding:6px 14px">—</td></tr>
    <tr><td style="padding:6px 14px"><code>400</code></td><td style="padding:6px 14px">Malformed request (bad CA format, unsupported chain)</td><td style="padding:6px 14px">no</td><td style="padding:6px 14px">Fix the input</td></tr>
    <tr><td style="padding:6px 14px"><code>401</code></td><td style="padding:6px 14px">Missing or invalid <code>X-API-Key</code></td><td style="padding:6px 14px">no</td><td style="padding:6px 14px">Check the header; get a key from <a href="https://t.me/TG_TC_BOT">@TG_TC_BOT</a></td></tr>
    <tr><td style="padding:6px 14px"><code>404</code></td><td style="padding:6px 14px">Not found — token doesn't exist / deleted tweet / unknown user / not a token</td><td style="padding:6px 14px">no</td><td style="padding:6px 14px">The resource doesn't exist; verify the address</td></tr>
    <tr><td style="padding:6px 14px"><code>422</code></td><td style="padding:6px 14px">Validation failure — missing param, unknown <code>action</code> / <code>field</code>, over-length text, category conflict</td><td style="padding:6px 14px"><b>no</b></td><td style="padding:6px 14px">Fix the request</td></tr>
    <tr><td style="padding:6px 14px"><code>429</code></td><td style="padding:6px 14px"><b>Insufficient balance</b> (not rate limiting)</td><td style="padding:6px 14px">no</td><td style="padding:6px 14px">Top up; recovers automatically</td></tr>
    <tr><td style="padding:6px 14px"><code>500</code></td><td style="padding:6px 14px">Upstream data fetch failed (timeout / upstream error)</td><td style="padding:6px 14px"><b>yes</b> — not refunded</td><td style="padding:6px 14px">Retry with backoff</td></tr>
  </tbody>
</table>

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

<table style="width:100%;border-collapse:collapse">
  <thead>
    <tr><th style="text-align:left;padding:6px 14px">状态</th><th style="text-align:left;padding:6px 14px">含义</th><th style="text-align:left;padding:6px 14px">扣次？</th><th style="text-align:left;padding:6px 14px">处理</th></tr>
  </thead>
  <tbody>
    <tr><td style="padding:6px 14px"><code>200</code></td><td style="padding:6px 14px">成功</td><td style="padding:6px 14px">是（见 <a href="billing.md">billing</a>）</td><td style="padding:6px 14px">—</td></tr>
    <tr><td style="padding:6px 14px"><code>400</code></td><td style="padding:6px 14px">请求格式错误（CA 格式 / chain 不支持）</td><td style="padding:6px 14px">否</td><td style="padding:6px 14px">修正入参</td></tr>
    <tr><td style="padding:6px 14px"><code>401</code></td><td style="padding:6px 14px">Key 缺失或无效</td><td style="padding:6px 14px">否</td><td style="padding:6px 14px">检查请求头；去 <a href="https://t.me/TG_TC_BOT">@TG_TC_BOT</a> 获取</td></tr>
    <tr><td style="padding:6px 14px"><code>404</code></td><td style="padding:6px 14px">数据不存在（代币不存在 / 推文已删 / 用户不存在 / 非代币）</td><td style="padding:6px 14px">否</td><td style="padding:6px 14px">核对地址是否真实存在</td></tr>
    <tr><td style="padding:6px 14px"><code>422</code></td><td style="padding:6px 14px">参数校验失败（缺参 / 未知 action·field / 超长 / 类别冲突）</td><td style="padding:6px 14px"><b>否</b></td><td style="padding:6px 14px">修正请求</td></tr>
    <tr><td style="padding:6px 14px"><code>429</code></td><td style="padding:6px 14px"><b>余额不足</b>（不是限流）</td><td style="padding:6px 14px">否</td><td style="padding:6px 14px">充值后自动恢复</td></tr>
    <tr><td style="padding:6px 14px"><code>500</code></td><td style="padding:6px 14px">上游数据获取失败（超时 / 上游异常）</td><td style="padding:6px 14px"><b>是</b>——已扣不退</td><td style="padding:6px 14px">按退避重试</td></tr>
  </tbody>
</table>

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
