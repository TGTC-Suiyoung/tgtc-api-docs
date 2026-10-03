# API Changelog

Version history of the TGTC data APIs. Each entry documents contract changes a developer
must care about (fields, billing, endpoints).

---

| Version | Date | Notes |
|---|---|---|
| `v1.6.0` | 2026-10-04 | **Official Python SDK released** (`tgtc-sdk` v0.2.0 on PyPI): all 9 product endpoints as one-line methods, transparent billing on every result, typed errors, automatic retry — see [sdk-python.md](sdk-python.md) |
| `v1.5.x` | 2026-10-02 | **Moves category** (`traders`): smart-money / KOL wallet in-out counts + net volume (2 credits); KOL wallets aligned with the official source; "cannot sell" false positive fixed; Bot full CA query = 4 on-chain + 4 Twitter + 3 translate = 11 credits |
| `v1.4.0` | 2026-09-24 | Trade stream `POST /api/v1/track/trades` (smart-money/KOL live trades, neutralized maker profiles) + signal stream `POST /api/v1/market/signals` (21 signal types with trigger-time snapshots); 1 credit = 1 data fetch each |
| `v1.3.x` | 2026-09-23 | Wallet analytics `POST /api/v1/wallet/{action}` (profile 33 fields, cursor pagination for activity) |
| `v1.2.x` | 2026-09-23 | Twitter API: 12 read endpoints, stable contract + per-call billing + 120s cache |
| `v1.1.0` | 2026-09-22 | **Output contract unified: 78 neutral fields, keys always present (missing → `null`)** |
| `v1.0.0` | 2026-09-21 | First release: aggregation endpoint (basic + security) |

---

## 中文说明 · API 版本历史

TGTC 数据 API 的版本记录——字段、计费、端点的契约变更都在这里。

| 版本 | 日期 | 说明 |
|---|---|---|
| `v1.6.0` | 2026-10-04 | **官方 Python SDK 发布**（`tgtc-sdk` v0.2.0，PyPI 上线）：9 个产品端点全部一行方法、每次返回计费明细、类型化异常、自动重试——见 [sdk-python.md](sdk-python.md) |
| `v1.5.x` | 2026-10-02 | **动向类别（traders）**：聪明钱 / KOL 上下车钱包数 + 净额（2 次）；KOL 钱包对齐官方数据源；修复「不可卖出」误报；Bot 完整 CA 查询 = 链上 4 + 推特 4 + 翻译 3 = 11 次 |
| `v1.4.0` | 2026-09-24 | 交易流 `POST /api/v1/track/trades`（聪明钱/KOL 实时交易，maker 画像中性化）+ 信号流 `POST /api/v1/market/signals`（21 种信号，含触发时刻快照）；各 1 次 = 1 次数据获取 |
| `v1.3.x` | 2026-09-23 | 钱包分析 `POST /api/v1/wallet/{action}`（profile 33 字段、activity 游标翻页） |
| `v1.2.x` | 2026-09-23 | 推特 API：12 个读端点，稳定契约 + 按次计费 + 120s 缓存 |
| `v1.1.0` | 2026-09-22 | **输出契约统一：78 字段中性命名、键恒存在（缺值输出 null）** |
| `v1.0.0` | 2026-09-21 | 首发：聚合端点（basic + security） |
