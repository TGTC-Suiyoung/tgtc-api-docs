# Python SDK

**`tgtc-sdk`** — the official Python client for the TGTC data API. Install it, hand it your key,
and every product endpoint becomes a one-line method call with the billing numbers surfaced on
the result object.

[中文说明 ↓](#中文说明)

---

## Install

```bash
pip install tgtc-sdk
```

Source + issues: [TGTC-Suiyoung/tgtc-sdk-python](https://github.com/TGTC-Suiyoung/tgtc-sdk-python)

## Quick start

```python
from tgtc import TGTC

client = TGTC(api_key="sk_live_...")   # from Bot → My API

res = client.token("0xbbc9565a44036007830c10b41d59ce55f3847777",
                   categories=["basic", "security"])

print(res.symbol, res.price, res.honeypot)
print(res.remaining)   # credits left after this call
print(res.used)        # credits this call cost
```

## Endpoints

| Method | Covers |
| --- | --- |
| `token(ca, categories=..., fields=...)` | Token aggregation (quotes / structure / security / holders / socials / smart money) |
| `trending(kind=..., limit=...)` | Token rankings (new / launch / graduating) |
| `hot(interval=..., limit=...)` | Hot search rankings |
| `trades(actor=..., side=..., limit=...)` | Smart-money / KOL live trades |
| `signals(signal_types=..., limit=...)` | Market signals |
| `wallet(action, wallet, period=..., ...)` | Wallet analysis (profile / stats / profits / activity / created / balance) |
| `twitter(action, username=..., ...)` | Twitter (user.info / tweets / timeline / followers / search / tweet.*) |
| `sentiment(ca)` | CA sentiment: heat rating + AI insight |
| `translate(action, text)` | AI translate / summarize (outputs Chinese) |

## Billing transparency

Every call returns a `Result` object carrying the billing truth:

| Field | Meaning |
| --- | --- |
| `remaining` | Credits left after this call |
| `used` | Credits this call cost (0 on cache hit) |
| `cache_hit` | True when served from cache (free) |
| `delay_sec` | Data freshness (0 = live) |

## Errors & retry

- 5xx / network failures: automatic exponential backoff with jitter (default up to 2 retries)
- **429 = out of credits** — never retried (it cannot succeed); raises `TGTCQuotaError` with `e.remaining`
- 400 / 422 / 404: not retried; 422 raises `TGTCParamError`

See the SDK README for the full exception table and retry policy.

---

## 中文说明

**`tgtc-sdk`** 是 TGTC 数据 API 的官方 Python 客户端。装上它、传上 Key，全部产品端点变成一行方法调用，
计费数字直接挂在返回对象上。

### 安装

```bash
pip install tgtc-sdk
```

源码与 Issue：[TGTC-Suiyoung/tgtc-sdk-python](https://github.com/TGTC-Suiyoung/tgtc-sdk-python)

### 快速开始

```python
from tgtc import TGTC

client = TGTC(api_key="sk_live_...")   # Bot → 「我的 API」

res = client.token("0xbbc9565a44036007830c10b41d59ce55f3847777",
                   categories=["basic", "security"])

print(res.symbol, res.price, res.honeypot)
print(res.remaining)   # 本次调用后剩余次数
print(res.used)        # 本次扣次
```

### 端点一览

| 方法 | 覆盖 |
| --- | --- |
| `token(ca, categories=..., fields=...)` | 代币聚合（行情 / 持仓 / 安全 / 持有人 / 社交 / 聪明钱） |
| `trending(kind=..., limit=...)` | 代币榜单（新创建 / 新发射 / 即将毕业） |
| `hot(interval=..., limit=...)` | 热门搜索榜单 |
| `trades(actor=..., side=..., limit=...)` | 聪明钱 / KOL 实时交易流 |
| `signals(signal_types=..., limit=...)` | 市场信号流 |
| `wallet(action, wallet, period=..., ...)` | 钱包分析（profile / stats / profits / activity / created / balance） |
| `twitter(action, username=..., ...)` | 推特检测（user.info / tweets / timeline / followers / search / tweet.*） |
| `sentiment(ca)` | CA 舆情：热度评级 + AI 解读 |
| `translate(action, text)` | AI 翻译 / 摘要（输出中文） |

### 计费透明

每次调用返回 `Result` 信封，携带计费真相：

| 字段 | 含义 |
| --- | --- |
| `remaining` | 本次调用后剩余次数 |
| `used` | 本次扣次（缓存命中为 0） |
| `cache_hit` | 是否命中缓存（免费） |
| `delay_sec` | 数据新鲜度（0 = 实时） |

### 错误与重试

- 5xx / 网络错误：自动指数退避重试（默认最多 2 次）
- **429 = 余额不足**——不重试（重试也不会成功）；抛 `TGTCQuotaError` 并携带 `e.remaining`
- 400 / 422 / 404：不重试；422 抛 `TGTCParamError`

完整异常表与重试策略见 SDK README。
