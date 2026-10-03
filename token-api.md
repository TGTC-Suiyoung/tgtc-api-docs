# Token Data API

All on-chain data endpoints: aggregation (the 78-field due-diligence core), listings, trade
stream, market signals, and wallet analytics. Same key, same balance.

---

## 1. Aggregation — `POST /api/v1/aggregation/token`

One call returns price / liquidity / holders / security / social / supply across six dimensions,
with **RPC on-chain verification** for the security-critical fields.

### Request

```json
{
  "ca": "0xbbc9565a44036007830c10b41d59ce55f3847777",
  "chain": "bsc",
  "categories": ["basic", "structure", "security"]
}
```

| Param | Type | Required | Notes |
|---|---|---|---|
| `ca` | string | yes | Contract address, `0x` + 40 hex |
| `chain` | string | no | Only `bsc` (default `bsc`) |
| `categories` | string[] | no | Default `["basic","structure","security"]`; see table below |
| `fields` | string[] | no | Response pruning; mutually exclusive with `categories`; unknown field → 422 |

### Categories & billing

| Category | Credits | Contents |
|---|---|---|
| `basic` | 1 | Price / mcap / liquidity / taxes / supply / social links |
| `structure` | 0 (free with basic) | Contract checks / pools / volumes / launch timeline |
| `security` | 1 | Mint / LP burn / blacklist / honeypot + **RPC on-chain verification** |
| `holders` | 1 | Holder count / TOP10 / dev holdings / snipers / whales |
| `social` | 1 | Twitter account info + the CA's official tweet |
| `traders` | 2 | Smart-money / KOL in-out wallet counts + net volume |

Cache hits (10s) deduct 0. Examples: default (basic+structure+security) = **2**; only `["basic"]` = 1; only `["security"]` = 2 (base layer + security); full 5 categories = **4**; with `traders` = 6. Field pruning never skips the base layer.

### Response

```json
{
  "address": "0xbbc9565a...",
  "symbol": "SpaceDog",
  "name": "SpaceDog",
  "decimals": 18,
  "price": 3.83e-05,
  "mcap": 38291.8,
  "liquidity": 21206.4,
  "exchange": "pancake_v2",
  "holder_count": 787,
  "top10_holders": 0.03,
  "honeypot": false,
  "mint_renounced": false,
  "lp_burned_ratio": 0.03,
  "buy_tax": 0.03,
  "sell_tax": 0.03,
  "categories": ["basic", "structure", "security"],
  "rpc": { "mint_renounced_rpc": true, "lp_burned_rpc": null, "honeypot_rpc": null, "price_rpc": 3.83e-05 },
  "twitter_info": { "username": "SpaceDog_SPCX", "name": "SpaceDog", "followers": 102456, "blue_verified": false },
  "twitter_tweet": { "id": "1234567890", "username": "SpaceDog_SPCX", "text": "...", "created_at": "2026-09-20 12:00:00", "url": "https://x.com/..." },
  "disclaimer": "本API仅提供数据聚合服务，不构成任何投资建议。..."
}
```

**Contract guarantee:** fields are always present (missing → `null`); field names never change.
Response structure: top-level fields + optional segments `rpc` (security), `twitter_info` +
`twitter_tweet` (social), `disclaimer`. See [field-dictionary.md](field-dictionary.md).

---

## 2. Listings — `POST /api/v1/token/trending` · `POST /api/v1/token/hot`

| Endpoint | Params | Credits | Cache |
|---|---|---|---|
| `/token/trending` | `kind` = new / launch / graduating · `limit` 1–100 | 1 | 120s |
| `/token/hot` | `interval` = 1m / 5m / 1h / 6h / 24h · `limit` 1–100 | 1 | 120s |

Ideal for new-coin radars and monitor tasks. Same balance as aggregation; cache hits free.

## 3. Trade stream — `POST /api/v1/track/trades`

Live smart-money / KOL trades: direction, amount, price, token profile, maker profile
(neutralized labels, no platform names).

| Param | Type | Notes |
|---|---|---|
| `actor` | string | `smartmoney` / `kol` |
| `side` | string | optional, `buy` / `sell` |
| `limit` | int | 1–200 |

1 credit per call · 15s cache.

## 4. Signal stream — `POST /api/v1/market/signals`

21 on-chain signal types (price spikes, ATH, smart-money buys, KOL buys, bundle sells, CTO…);
each signal carries a full trigger-time token snapshot.

| Param | Type | Notes |
|---|---|---|
| `signal_types` | int[] | Types 1–13, 17–21; **20 = KOL buy**; 14–16 are invalid |
| `limit` | int | 1–200 |

1 credit per call · 30s cache.

## 5. Wallet analytics — `POST /api/v1/wallet/{action}`

| Action | Credits | Purpose |
|---|---|---|
| `profile` | 2 | Wallet profile = stats + profits + identity (33 fields) |
| `stats` | 1 | Trade statistics |
| `profits` | 1 | Realized / unrealized P&L |
| `activity` | 1 | Trade history, cursor-paginated |
| `created` | 1 | Tokens created by this wallet (+ best historical record) |
| `balance` | 1 | Balance of a specific token (requires `token`) |

| Param | Type | Notes |
|---|---|---|
| `wallet` | string | required, `0x` + 40 hex |
| `period` | string | `1d` / `7d` / `30d` |
| `limit` / `cursor` | int / string | pagination (activity) |
| `token` | string | required for `balance` |

30s cache · 422 on unknown action never deducts.

## Item field contracts

Every list endpoint returns a stable item contract — fields always present, missing → `null`,
field names never change. Complete field lists: see [field-dictionary.md](field-dictionary.md)
(listings 60 · trade records 26 · signals 45 · wallet profile 34 · wallet activity 17 ·
wallet created 17 · wallet balance 6). Key invariants: `tx_hash` (trades/activity) and
`address` (signals) survive field pruning; maker labels are neutralized; `kol_buyers`
carries no CDN fields.

---

## 中文说明 · 代币数据 API

链上数据全家桶：聚合（78 字段尽调核心）、榜单、交易流、市场信号、钱包分析。同一把 Key、同一份余额。

### 1. 聚合端点 `POST /api/v1/aggregation/token`

一次调用拿到价格 / 流动性 / 持仓 / 安全 / 社交 / 供应六维数据，安全关键字段附 **RPC 链上独立核验**。

**请求**：`ca`（必填）、`chain`（缺省 bsc）、`categories`（缺省 basic+structure+security）、`fields`（裁剪，与 categories 二选一）。

**分类与扣次**：`basic` 1 · `structure` 0（随 basic 免费）· `security` 1 · `holders` 1 · `social` 1 · `traders` 2。
缺省 = 2 次；只查 basic = 1；全量 5 分类 = 4；加 traders = 6。10s 缓存命中不扣次；字段裁剪不省基础层。

**响应**：顶层字段 + 附加段 `rpc`（安全核验）/ `twitter_info` + `twitter_tweet`（社交）/ `disclaimer`。
契约保证：字段恒定输出（缺值 null）、字段名永不改变。

### 2. 榜单 `POST /api/v1/token/trending` · `hot`

trending：`kind` = new / launch / graduating · `limit` 1~100；hot：`interval` = 1m/5m/1h/6h/24h · `limit` 1~100。各 1 次，120s 缓存。

### 3. 交易流 `POST /api/v1/track/trades`

`actor` = smartmoney / kol · `side` 可选 · `limit` 1~200。每次 1 次，15s 缓存。maker 画像已中性化。

### 4. 信号流 `POST /api/v1/market/signals`

`signal_types` = 1~13、17~21（**20 = KOL 买入**；14~16 非法）· `limit` 1~200。每次 1 次，30s 缓存；每条信号含触发时刻完整快照。

### 5. 钱包分析 `POST /api/v1/wallet/{action}`

`profile` 2 次（33 字段画像）/ `stats` / `profits` / `activity` / `created` / `balance` 各 1 次。
参数：`wallet`（必填）、`period`（1d/7d/30d）、`limit` / `cursor`（翻页）、`token`（balance 必填）。30s 缓存；未知 action 422 不扣次。

### Item 字段契约

所有列表端点返回稳定 item 契约——字段恒定输出（缺值 null）、字段名永不改变。完整字段清单见
[field-dictionary.md](field-dictionary.md)（榜单 60 · 交易 26 · 信号 45 · 钱包画像 34 · 钱包流水 17 · 发币 17 · 余额 6）。
关键约定：`tx_hash`（交易/流水）与 `address`（信号）裁剪时恒保留；maker 标签已中性化；`kol_buyers` 不含 CDN 字段。
