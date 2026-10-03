# Rate Limits & Scheduling

TGTC shares one data channel across the whole platform. The server queues and rate-limits
internally — you are never "punished" with errors for normal bursts; you only need to handle
balance (`429`) and upstream failures (`500`).

---

## Scheduling tiers

Your balance tier automatically raises your scheduling priority and burst buffer — no application needed.

| Tier | Priority | Best for |
|---|---|---|
| Starter (any balance) | Standard scheduling · shared buffer | Light calls / integration testing |
| Standard (balance ≥ 5,000) | Priority scheduling · larger burst buffer | Regular production |
| Pro (balance ≥ 60,000) | High priority · batch tasks first · dedicated channel on request | Bulk collection / follower-network export |

Extremely high throughput (bulk follower export, deep historical pulls) can get a dedicated
channel via DM — custom quotas on request.

## Two different "429s"

- **API `429` = balance exhausted.** Top up and it recovers automatically. Nothing else to do.
- **Server burst buffer is not an error.** High concurrency is queued server-side; clients never
  see it as a failure and must not retry it.

## Client backoff

On `429` (balance) or `500`, wait `1s → 2s → 4s` (cap 30s), at most **3 retries**.
Pagination: a new `cursor` is a new request — just re-issue with the new cursor, don't retry
the old parameters.

---

## 中文说明 · 限流与调度

全站共享同一数据通道，服务端统一排队与限速——正常并发突发**不会**被惩罚报错；你只需要处理
余额不足（`429`）与上游失败（`500`）。

### 调度等级

余额档位自动提升调度优先级与突发缓冲，无需申请：

| 等级 | 优先级 | 适合场景 |
|---|---|---|
| 入门（任意余额） | 标准调度 · 共享缓冲 | 轻量调用 / 联调测试 |
| 标准（余额 ≥ 5,000 次） | 优先调度 · 更大突发缓冲 | 常规生产 |
| 专业（余额 ≥ 60,000 次） | 高优先级 · 批量任务优先 · 可申请专属通道 | 批量采集 / 粉丝网络导出 |

极高吞吐（批量导出粉丝网络、历史深挖）可私聊开通专属通道，按需定制。

### 两种「429」

- **API `429` = 余额用完**：充值后自动恢复，无需其它操作。
- **服务端突发缓冲不是错误**：高并发在服务端排队处理，客户端永远不会收到这个失败、也无需重试。

### 客户端退避

遇到 `429`（余额）或 `500` 后等待 `1s → 2s → 4s`（上限 30s），最多 **3 次**。翻页：新
`cursor` 是新请求——直接用新 cursor 重发，不要重试旧参数。
