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

<table style="width:100%;border-collapse:collapse">
  <thead>
    <tr><th style="text-align:left;padding:6px 14px">File</th><th style="text-align:left;padding:6px 14px">Contents</th><th style="text-align:left;padding:6px 14px">Status</th></tr>
  </thead>
  <tbody>
    <tr><td style="padding:6px 14px"><a href="getting-started.md">getting-started.md</a></td><td style="padding:6px 14px">Get a key → first call (curl / Python / JS)</td><td style="padding:6px 14px">✅ done</td></tr>
    <tr><td style="padding:6px 14px"><a href="sdk-python.md">sdk-python.md</a></td><td style="padding:6px 14px">Official Python SDK: install, endpoints, transparent billing</td><td style="padding:6px 14px">✅ done</td></tr>
    <tr><td style="padding:6px 14px"><a href="authentication.md">authentication.md</a></td><td style="padding:6px 14px"><code>X-API-Key</code>, 401 / 429, top-up packs, key rotation</td><td style="padding:6px 14px">✅ done</td></tr>
    <tr><td style="padding:6px 14px"><a href="billing.md">billing.md</a></td><td style="padding:6px 14px">Master deduction table, cache-hit rules, 422 / 500 semantics</td><td style="padding:6px 14px">✅ done</td></tr>
    <tr><td style="padding:6px 14px"><a href="token-api.md">token-api.md</a></td><td style="padding:6px 14px">Aggregation / listings / trade stream / signals / wallet</td><td style="padding:6px 14px">✅ done</td></tr>
    <tr><td style="padding:6px 14px"><a href="twitter-api.md">twitter-api.md</a></td><td style="padding:6px 14px">12 tweet endpoints, pagination billing</td><td style="padding:6px 14px">✅ done</td></tr>
    <tr><td style="padding:6px 14px"><a href="translate-api.md">translate-api.md</a></td><td style="padding:6px 14px">translate / summarize, Chinese-input free</td><td style="padding:6px 14px">✅ done</td></tr>
    <tr><td style="padding:6px 14px"><a href="sentiment-api.md">sentiment-api.md</a></td><td style="padding:6px 14px">CA sentiment scan, dual-channel heat rating</td><td style="padding:6px 14px">✅ done</td></tr>
    <tr><td style="padding:6px 14px"><a href="field-dictionary.md">field-dictionary.md</a></td><td style="padding:6px 14px">Field-level dictionary: source, nullability, category, billing</td><td style="padding:6px 14px">✅ done</td></tr>
    <tr><td style="padding:6px 14px"><a href="errors.md">errors.md</a></td><td style="padding:6px 14px">Error codes, retry backoff</td><td style="padding:6px 14px">✅ done</td></tr>
    <tr><td style="padding:6px 14px"><a href="sla.md">sla.md</a></td><td style="padding:6px 14px">Service level, degradation markers, maintenance, billing disputes</td><td style="padding:6px 14px">✅ done</td></tr>
    <tr><td style="padding:6px 14px"><a href="faq.md">faq.md</a></td><td style="padding:6px 14px">Frequently asked questions (contract-grade answers)</td><td style="padding:6px 14px">✅ done</td></tr>
    <tr><td style="padding:6px 14px"><a href="rate-limits.md">rate-limits.md</a></td><td style="padding:6px 14px">Scheduling tiers, burst buffer</td><td style="padding:6px 14px">✅ done</td></tr>
    <tr><td style="padding:6px 14px"><a href="changelog.md">changelog.md</a></td><td style="padding:6px 14px">API version history</td><td style="padding:6px 14px">✅ done</td></tr>
  </tbody>
</table>

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

- **Python SDK (open source):** [TGTC-Suiyoung/tgtc-sdk-python](https://github.com/TGTC-Suiyoung/tgtc-sdk-python)
- **Extension (open source):** [TGTC-Suiyoung/tgtc-extension](https://github.com/TGTC-Suiyoung/tgtc-extension)
- **Live site:** https://www.tgtcbot.com
- **Status:** https://www.tgtcbot.com/status.zh.html

## 🛡 Document Charter

- **Black-box by design:** this repo documents only the public contract — endpoint paths,
  request / response fields, billing, errors. It never describes upstream data sources,
  internal pipelines, or supplier names.
- **Stable contract:** field names never change once released; missing values return `null`.
- **Billing truth:** every number in this repo mirrors what the service actually deducts.
- **Concise commits:** commit titles stay short — one clear topic per commit.

## 📄 License

MIT — see [LICENSE](LICENSE). Docs only; the TGTC service is operated by tgtcbot.com.

---

## 中文说明

**TGTC — BSC 代币尽调与 X 舆情数据层** 的开发者文档仓库。

TGTC 不是共识层，也不是喊单引擎。它是 **BSC 高频 meme 场景下的链上安全 + X 舆情数据层**——
给人和给程序用的是同一把 Key、同一份契约。本仓库是线上文档（[tgtcbot.com](https://www.tgtcbot.com)）
的开源契约版，随版本演进、可提 Issue / PR 共建。

### 文档目录

<table style="width:100%;border-collapse:collapse">
  <thead>
    <tr><th style="text-align:left;padding:6px 14px">文件</th><th style="text-align:left;padding:6px 14px">内容</th><th style="text-align:left;padding:6px 14px">状态</th></tr>
  </thead>
  <tbody>
    <tr><td style="padding:6px 14px"><a href="getting-started.md">getting-started.md</a></td><td style="padding:6px 14px">拿 Key → 第一次调用（curl / Python / JS）</td><td style="padding:6px 14px">✅ 完成</td></tr>
    <tr><td style="padding:6px 14px"><a href="sdk-python.md">sdk-python.md</a></td><td style="padding:6px 14px">官方 Python SDK：安装 / 端点 / 计费透明</td><td style="padding:6px 14px">✅ 完成</td></tr>
    <tr><td style="padding:6px 14px"><a href="authentication.md">authentication.md</a></td><td style="padding:6px 14px"><code>X-API-Key</code>、401 / 429、充值包、Key 轮换</td><td style="padding:6px 14px">✅ 完成</td></tr>
    <tr><td style="padding:6px 14px"><a href="billing.md">billing.md</a></td><td style="padding:6px 14px">全接口扣费总表、缓存命中规则、422 / 500 口径</td><td style="padding:6px 14px">✅ 完成</td></tr>
    <tr><td style="padding:6px 14px"><a href="token-api.md">token-api.md</a></td><td style="padding:6px 14px">聚合 / 榜单 / 交易流 / 信号流 / 钱包</td><td style="padding:6px 14px">✅ 完成</td></tr>
    <tr><td style="padding:6px 14px"><a href="twitter-api.md">twitter-api.md</a></td><td style="padding:6px 14px">推特 12 端点、翻页计费</td><td style="padding:6px 14px">✅ 完成</td></tr>
    <tr><td style="padding:6px 14px"><a href="translate-api.md">translate-api.md</a></td><td style="padding:6px 14px">translate / summarize、中文免调</td><td style="padding:6px 14px">✅ 完成</td></tr>
    <tr><td style="padding:6px 14px"><a href="sentiment-api.md">sentiment-api.md</a></td><td style="padding:6px 14px">CA 舆情扫描、双通道热度评级</td><td style="padding:6px 14px">✅ 完成</td></tr>
    <tr><td style="padding:6px 14px"><a href="field-dictionary.md">field-dictionary.md</a></td><td style="padding:6px 14px">字段级字典：来源 / 可空 / 分类 / 扣次</td><td style="padding:6px 14px">✅ 完成</td></tr>
    <tr><td style="padding:6px 14px"><a href="errors.md">errors.md</a></td><td style="padding:6px 14px">错误码表、重试退避</td><td style="padding:6px 14px">✅ 完成</td></tr>
    <tr><td style="padding:6px 14px"><a href="sla.md">sla.md</a></td><td style="padding:6px 14px">服务承诺、降级标记、维护窗口、计费申诉</td><td style="padding:6px 14px">✅ 完成</td></tr>
    <tr><td style="padding:6px 14px"><a href="faq.md">faq.md</a></td><td style="padding:6px 14px">常见问题（契约级回答）</td><td style="padding:6px 14px">✅ 完成</td></tr>
    <tr><td style="padding:6px 14px"><a href="rate-limits.md">rate-limits.md</a></td><td style="padding:6px 14px">调度等级、突发缓冲</td><td style="padding:6px 14px">✅ 完成</td></tr>
    <tr><td style="padding:6px 14px"><a href="changelog.md">changelog.md</a></td><td style="padding:6px 14px">API 版本历史</td><td style="padding:6px 14px">✅ 完成</td></tr>
  </tbody>
</table>

### 快速开始

1. Telegram 私聊 [@TG_TC_BOT](https://t.me/TG_TC_BOT) → 充值 → 「🔑 我的 API」生成 `sk_live_` Key（新用户赠送 **500 次**）
2. 任意端点请求头带 `X-API-Key`（示例见上方英文区）
3. 每次响应读 `X-Cache: HIT`（命中缓存，0 次）与 `X-RateLimit-Remaining`（剩余次数）

### 文档宪章

- **黑盒原则**：只写对外契约——端点路径 / 请求响应字段 / 计费 / 错误码；**不描述任何上游数据源、内部管线或供应商名称**。
- **稳定契约**：已发布字段名永不改变；缺值返回 `null`。
- **计费真相**：本仓库每个数字与服务实际扣次严格一致（以服务端配置为准）。
- **简洁提交**：commit 标题保持简短，一个主题一个提交。

### 相关链接

- **Python SDK（开源）**：[TGTC-Suiyoung/tgtc-sdk-python](https://github.com/TGTC-Suiyoung/tgtc-sdk-python)
- **浏览器扩展（开源）**：[TGTC-Suiyoung/tgtc-extension](https://github.com/TGTC-Suiyoung/tgtc-extension)
- **官网**：https://www.tgtcbot.com
- **服务状态**：https://www.tgtcbot.com/status.zh.html

### License

MIT。文档仅供参考，TGTC 服务由 tgtcbot.com 运营。
