# Service Level — Honest Commitments

**TGTC sells a data contract, not an uptime promise.** This page is the honest version of an
SLA: what we commit to, how degradation is visible, when maintenance happens, and how billing
disputes are resolved.

---

## 1. Service level

- **Best effort, no hard uptime number.** We are a small team with external data
  dependencies — availability is affected by factors outside our control. Promising "99.9%"
  would be fiction.
- **What we do commit to — the data contract:**
  1. Field names never change once released; missing values return `null` (never fabricated).
  2. Billing is transparent: credits = actual data fetches; cache hits deduct 0; 422 never
     deducts; every response carries `X-RateLimit-Remaining`.
  3. Degradation is **explicit** — the response tells you which source is down (below).
  4. Billing errors are **refunded** (below).
- **What this means in practice:** the service can be slow or briefly down; it will never
  pretend to be healthy when it isn't, and it will never refuse to own a billing mistake.

## 2. Degradation markers — transparent by design

Every composite response (aggregation, sentiment) carries two meta-fields:

| Field | Meaning |
|---|---|
| `data_delay_sec` | How stale the served data is in seconds: `0` = freshly fetched; on a cache hit it is the cache age |
| `degraded_sources` | Which upstream source was unavailable while assembling this response: `[]` = all healthy |

Source names are our own product data dimensions: `token` (token data) · `security` (safety
audit) · `holders` · `traders` · `twitter` (X data) · `chain_rpc` (on-chain verification) ·
`ai` (AI analysis).

**Read it this way:** a `null` field means its source is down — that's the signal, not an
anomaly. `degraded_sources: ["twitter"]` + `twitter_info: null` tells you exactly which part
you can trust. The Twitter proxy is single-source: its failure surfaces as `500` (see
[errors.md](errors.md)) — there is no partial data to mark.

## 3. Maintenance windows

- **Planned maintenance:** announced at least **24 hours ahead** on the
  [status page](https://www.tgtcbot.com/status.zh.html) and in the Bot announcement channel.
- **Emergency fixes** may ship without 24h notice but are still posted as soon as possible.
- Requests that fail **inside a declared maintenance window** are eligible for a refund (below).

## 4. Billing disputes & refunds

**How to report:** DM [@TG_TC_BOT](https://t.me/TG_TC_BOT) → "Billing dispute", and include
the request time, the CA/action, and the response headers (`X-RateLimit-Used`).

| Case | Verdict |
|---|---|
| `500` caused by a **TGTC-side service fault** (not the upstream) | **Refunded** |
| Failure inside a declared **maintenance window** | **Refunded** |
| Double deduction / obvious billing error | **Refunded** |
| `500` where the data fetch genuinely ran (not a TGTC-side fault) | Not refunded — the fetch actually happened |
| `422` (validation) | Nothing was deducted; no refund involved |
| Balance spent on your own retries / misconfigured loops | Not refunded |

Disputes are reviewed against the call log; verified refunds are credited back to the same balance.

---

## 中文说明 · 服务承诺（尽力而为，诚实透明）

**TGTC 卖的是数据契约，不是不停机承诺。** 本页是 SLA 的诚实版：承诺什么、降级怎么可见、
何时维护、计费争议怎么解决。

### 1. 服务级别

- **尽力而为，不承诺硬性可用率数字**——小团队、依赖外部数据通道，可用性受外部因素影响。
  写「99.9%」是自欺欺人。
- **真正承诺的是数据契约**：
  1. 字段名发布后永不改变；缺值返回 `null`（绝不编造）。
  2. 计费透明：扣次 = 实际数据获取次数；缓存命中扣 0；422 永不扣；每次响应带 `X-RateLimit-Remaining`。
  3. 降级**显式可见**——响应明确告诉你哪个源挂了（见下）。
  4. 计费错误**退**（见下）。
- **人话**：服务可能慢、可能短暂不可用；但**绝不假装正常，绝不拒认计费错误**。

### 2. 降级标记 —— 状态透明可见

所有复合端点（聚合、舆情）响应携带两个元字段：

| 字段 | 含义 |
|---|---|
| `data_delay_sec` | 本次返回数据的陈旧秒数：`0` = 新鲜拉取；缓存命中时为缓存年龄 |
| `degraded_sources` | 拼装本次响应时不可用的数据维度：`[]` = 全部健康 |

源名即我们的产品数据维度：`token`（代币数据）· `security`（安全审计）· `holders`（持仓）·
`traders`（动向）· `twitter`（X 数据）· `chain_rpc`（链上核验）· `ai`（AI 分析）。

**读法**：字段 `null` = 对应数据源当前不可用——这是信号，不是异常。`degraded_sources: ["twitter"]`
+ `twitter_info: null` 精确告诉你哪部分可信。推特代理是单源端点：故障以 `500` 呈现（见
[errors.md](errors.md)）——没有可标记的部分数据。

### 3. 维护窗口

- **计划维护**：至少提前 **24 小时**在[状态页](https://www.tgtcbot.com/status.zh.html)与 Bot 公告频道通知。
- **紧急修复**：不保证 24h 预告，但会尽快公告。
- 声明维护窗口内失败的请求**可申请返还**（见下）。

### 4. 计费争议与返还

**申诉方式**：私聊 [@TG_TC_BOT](https://t.me/TG_TC_BOT) →「计费申诉」，附上请求时间、CA/动作、
响应头（`X-RateLimit-Used`）。

| 情形 | 判定 |
|---|---|
| 我方服务故障导致的 `500`（非外部原因） | **返还** |
| 声明维护窗口内失败 | **返还** |
| 双扣 / 明显计费错误 | **返还** |
| 数据获取确实产生消耗的 `500` | 不退——拉取确实发生了 |
| `422`（参数校验） | 本就未扣，无返还 |
| 自己重试 / 循环写错烧掉的次数 | 不退 |

争议按调用日志核实，确认后返还到同一余额。
