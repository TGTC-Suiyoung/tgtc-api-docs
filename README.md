<p align="center">
  <img src="https://www.tgtcbot.com/logo.png" width="96" alt="TGTC">
</p>

# 📘 TGTC API Docs

**Developer documentation for the TGTC data layer — on-chain due diligence + X sentiment
for high-frequency BSC meme scenarios.**

TGTC is not a consensus layer, and it is not a signal-pump engine. It is a data layer for
**BSC on-chain safety + X sentiment** — the same key, the same contract, for humans and for
programs. This repo is the open, versioned contract behind the live docs at
[tgtcbot.com](https://www.tgtcbot.com).

[中文说明 ↓](#中文说明)

---

## 📚 Documentation

| File | Contents | Status |
|---|---|---|
| [getting-started.md](getting-started.md) | Get a key → first call (curl / Python / JS) | 🔨 in progress |
| [authentication.md](authentication.md) | `X-API-Key`, 401 / 429, top-up packs, key rotation | 🔨 in progress |
| [billing.md](billing.md) | Master deduction table, cache-hit rules, 422 / 500 semantics | 🔨 in progress |
| [token-api.md](token-api.md) | Aggregation / listings / trade stream / signals / wallet | 🔨 in progress |
| [twitter-api.md](twitter-api.md) | 12 tweet endpoints, pagination billing | 🔨 in progress |
| [translate-api.md](translate-api.md) | translate / summarize, Chinese-input free | 🔨 in progress |
| [sentiment-api.md](sentiment-api.md) | CA sentiment scan, dual-channel heat rating | 🔨 in progress |
| [field-dictionary.md](field-dictionary.md) | Field-level dictionary: source, nullability, category, billing | ⏳ planned |
| [errors.md](errors.md) | Error codes, retry backoff | 🔨 in progress |
| [rate-limits.md](rate-limits.md) | Scheduling tiers, burst buffer | ⏳ planned |
| [changelog.md](changelog.md) | API version history | ⏳ planned |

## 🚀 Quick Start

1. DM [@TG_TC_BOT](https://t.me/TG_TC_BOT) on Telegram → top up → **My API** → generate a
   `sk_live_` key (new users get **500 free credits**).
2. Call any endpoint with `X-API-Key`:

```bash
curl -s -X POST https://www.tgtcbot.com/api/v1/aggregation/token \
  -H "Content-Type: application/json" -H "X-API-Key: sk_live_..." \
  -d '{"ca":"0xbbc9565a44036007830c10b41d59ce55f3847777","categories":["basic","structure","security","traders"]}'
```

3. Read `X-Cache: HIT` (cache hit, 0 credits) and `X-RateLimit-Remaining` in every response.

## 🔗 Related

- **Extension (open source):** [TGTC-Suiyoung/tgtc-extension](https://github.com/TGTC-Suiyoung/tgtc-extension)
- **Live site:** https://www.tgtcbot.com
- **Status:** https://www.tgtcbot.com/status.zh.html

## 🛡 Document Charter

- **Black-box by design:** this repo documents only the public contract — endpoint paths,
  request / response fields, billing, errors. It never describes upstream data sources,
  internal pipelines, or supplier names.
- **Stable contract:** field names never change once released; missing values return `null`.
- **Billing truth:** every number in this repo mirrors what the service actually deducts.

## 📄 License

MIT — see [LICENSE](LICENSE). Docs only; the TGTC service is operated by tgtcbot.com.

---

## 中文说明

**TGTC — BSC 代币尽调与 X 舆情数据层** 的开发者文档仓库。

TGTC 不是共识层，也不是喊单引擎。它是 **BSC 高频 meme 场景下的链上安全 + X 舆情数据层**——
给人和给程序用的是同一把 Key、同一份契约。本仓库是线上文档（[tgtcbot.com](https://www.tgtcbot.com)）
的开源契约版，随版本演进、可提 Issue / PR 共建。

### 文档目录

| 文件 | 内容 | 状态 |
|---|---|---|
| [getting-started.md](getting-started.md) | 拿 Key → 第一次调用（curl / Python / JS） | 🔨 编写中 |
| [authentication.md](authentication.md) | `X-API-Key`、401 / 429、充值包、Key 轮换 | 🔨 编写中 |
| [billing.md](billing.md) | 全接口扣费总表、缓存命中规则、422 / 500 口径 | 🔨 编写中 |
| [token-api.md](token-api.md) | 聚合 / 榜单 / 交易流 / 信号流 / 钱包 | 🔨 编写中 |
| [twitter-api.md](twitter-api.md) | 推特 12 端点、翻页计费 | 🔨 编写中 |
| [translate-api.md](translate-api.md) | translate / summarize、中文免调 | 🔨 编写中 |
| [sentiment-api.md](sentiment-api.md) | CA 舆情扫描、双通道热度评级 | 🔨 编写中 |
| [field-dictionary.md](field-dictionary.md) | 字段级字典：来源 / 可空 / 分类 / 扣次 | ⏳ 待建 |
| [errors.md](errors.md) | 错误码表、重试退避 | 🔨 编写中 |
| [rate-limits.md](rate-limits.md) | 调度等级、突发缓冲 | ⏳ 待建 |
| [changelog.md](changelog.md) | API 版本历史 | ⏳ 待建 |

### 快速开始

1. Telegram 私聊 [@TG_TC_BOT](https://t.me/TG_TC_BOT) → 充值 → 「🔑 我的 API」生成 `sk_live_` Key（新用户赠送 **500 次**）
2. 任意端点请求头带 `X-API-Key`（示例见上方英文区）
3. 每次响应读 `X-Cache: HIT`（命中缓存，0 次）与 `X-RateLimit-Remaining`（剩余次数）

### 文档宪章

- **黑盒原则**：只写对外契约——端点路径 / 请求响应字段 / 计费 / 错误码；**不描述任何上游数据源、内部管线或供应商名称**。
- **稳定契约**：已发布字段名永不改变；缺值返回 `null`。
- **计费真相**：本仓库每个数字与服务实际扣次严格一致（以服务端配置为准）。

### 相关链接

- **浏览器扩展（开源）**：[TGTC-Suiyoung/tgtc-extension](https://github.com/TGTC-Suiyoung/tgtc-extension)
- **官网**：https://www.tgtcbot.com
- **服务状态**：https://www.tgtcbot.com/status.zh.html

### License

MIT。文档仅供参考，TGTC 服务由 tgtcbot.com 运营。
