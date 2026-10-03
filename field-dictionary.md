# Field Dictionary

The aggregation endpoint returns a stable, neutral contract: **fields are always present,
missing values are `null`, field names never change** once released. This page maps the core
fields by category. Full canonical list mirrors the live doc at
[tgtcbot.com/doc.zh.html](https://www.tgtcbot.com/doc.zh.html).

---

## Top-level (always present)

| Field | Type | Category | Notes |
|---|---|---|---|
| `address` | string | basic | Contract address (lowercase) |
| `symbol` / `name` | string | basic | Ticker / display name |
| `decimals` | int | basic | Token decimals |
| `price` | number | basic | Current price (USD) |
| `mcap` | number | basic | Market cap (USD) |
| `liquidity` | number | basic | Pool liquidity (USD) |
| `exchange` | string | structure | DEX (e.g. pancake_v2) |
| `holder_count` | int | holders | Holder count |
| `top10_holders` | number | holders | Top-10 share (ratio) |
| `honeypot` | boolean | security | Honeypot flag (RPC-verified when available) |
| `mint_renounced` | boolean | security | Mint ownership renounced |
| `lp_burned_ratio` | number | security | LP burned ratio |
| `buy_tax` / `sell_tax` | number | security | Buy / sell tax (ratio) |
| `volume_24h` | number | structure | 24h volume (USD) |
| `categories` | string[] | — | Echo of the categories actually applied |

## `rpc` segment (security)

| Field | Notes |
|---|---|
| `mint_renounced_rpc` | Mint verified directly on BSC RPC |
| `lp_burned_rpc` | LP burn verified on RPC (null when not resolvable) |
| `honeypot_rpc` | Honeypot verified on RPC |
| `price_rpc` | Price cross-checked on RPC |

RPC verification is an independent second source — it raises confidence, it is not an audit.

## `twitter_info` segment (social)

`username` · `name` · `followers` · `blue_verified` — the token's linked Twitter account.
Deep profile: `twitter_rename_count` (renames) · `twitter_deleted_tweet_count` (deleted tweets) ·
`twitter_created_token_count` (tokens created) — history not visible on the profile page.

## `twitter_tweet` segment (social)

`id` · `username` · `text` · `created_at` · `url` — the CA's designated official tweet.

## `traders` segment (moves, 2 credits)

`smart_buy` / `smart_sell` — smart-money wallet counts in / out ·
`smart_net` — net buy volume (USD) · `kol_buy` / `kol_sell` / `kol_net` — same for KOL ·
`kol_count` · `kol_names`.

**Units:** in / out are **wallet address counts**, not trade counts.

## Structure & holders extras

`initial_liquidity` · `base_reserve` / `quote_reserve` · `swaps_24h` · `buys_24h` / `sells_24h` ·
`age_hours` · `circulating_supply` / `total_supply` · `launchpad` · `launchpad_progress` ·
`dev_hold_pct` · `creator_hold_pct` · `sniper_ratio` · `vault_ratio` · `smart_wallets` ·
`fresh_wallets` · `sniper_wallets` · `whale_wallets` · `fishing_ratio` · `bot_ratio` ·
`bundler_ratio` · `contract_verified` · `open_source` · `community_takeover` ·
`buy_sell_ratio_24h` · `ath_price` · `chg_1h_pct` · `chg_24h_pct`.

---

## 中文说明 · 字段字典

聚合端点返回稳定、中性的契约：**字段恒定输出（缺值 null）、字段名永不改变**。本页按分类列出核心字段；
完整权威清单以 [tgtcbot.com/doc.zh.html](https://www.tgtcbot.com/doc.zh.html) 为准。

### 顶层字段（恒存在）

`address`（合约地址）· `symbol` / `name` · `decimals` · `price`（USD）· `mcap`（市值）· `liquidity`（流动性）·
`exchange`（DEX）· `holder_count`（持有人数）· `top10_holders`（前10占比）· `honeypot`（貔貅，RPC 核验可用时）·
`mint_renounced`（Mint 是否放弃）· `lp_burned_ratio`（LP 烧毁比例）· `buy_tax` / `sell_tax`（买卖税）·
`volume_24h`（24h 成交）· `categories`（实际生效分类回显）。

### `rpc` 段（security 分类）

`mint_renounced_rpc`（BSC RPC 直接核验 Mint）· `lp_burned_rpc`（LP 烧毁核验，不可解析时 null）·
`honeypot_rpc`（貔貅核验）· `price_rpc`（价格交叉核验）。

RPC 核验是独立第二数据源——提升置信度，**不是审计级保证**。

### `twitter_info` 段（social 分类）

`username` · `name` · `followers` · `blue_verified`——代币关联推特账号。
深层画像：`twitter_rename_count`（改名次数）· `twitter_deleted_tweet_count`（删帖数）·
`twitter_created_token_count`（发币数）——页面本身看不到的历史。

### `twitter_tweet` 段（social 分类）

`id` · `username` · `text` · `created_at` · `url`——该 CA 指定的官方宣传推文。

### `traders` 段（动向，2 次）

`smart_buy` / `smart_sell`（聪明钱上下车**钱包数**）· `smart_net`（净买入额 USD）·
`kol_buy` / `kol_sell` / `kol_net`（KOL 同款）· `kol_count` · `kol_names`。

**单位说明**：上车 / 下车是**钱包地址数**，不是交易次数。

### 结构 / 持仓扩展字段

`initial_liquidity` · `base_reserve` / `quote_reserve` · `swaps_24h` · `buys_24h` / `sells_24h` ·
`age_hours` · `circulating_supply` / `total_supply` · `launchpad` · `launchpad_progress` ·
`dev_hold_pct` · `creator_hold_pct` · `sniper_ratio` · `vault_ratio` · `smart_wallets` ·
`fresh_wallets` · `sniper_wallets` · `whale_wallets` · `fishing_ratio` · `bot_ratio` ·
`bundler_ratio` · `contract_verified` · `open_source` · `community_takeover` ·
`buy_sell_ratio_24h` · `ath_price` · `chg_1h_pct` · `chg_24h_pct`。
