# Field Dictionary

The data layer returns a stable, neutral contract: **fields are always present, missing
values are `null`, field names never change** once released. All lists below are the
canonical field sets of each endpoint family.

---

## Aggregation `POST /api/v1/aggregation/token`

### 基础信息

`address` string · `symbol` string · `name` string · `decimals` number · `standard` string (erc20) ·
`contract_verified` bool · `description` string

### 价格 / 市值 / 涨跌

`price` · `mcap` · `ath_price` · `price_1m` · `price_5m` · `price_1h` · `price_6h` · `price_24h` ·
`chg_5m_pct` · `chg_1h_pct` · `chg_24h_pct` · `buy_sell_ratio_24h` — 均 number（USD / %）

### 流动性 / 池子

`liquidity` · `pool_liquidity` · `exchange` string (pancake_v2…) · `quote_symbol` ·
`pool_address` · `initial_liquidity` · `base_reserve` · `quote_reserve`

### 交易量 / 活跃度

`volume_1h` · `volume_24h` · `buys_24h` · `sells_24h` · `buy_volume_24h` · `sell_volume_24h` ·
`swaps_24h` · `trade_fee`

### 持仓 / 筹码结构

`holder_count` · `top10_holders`（建议 <40%）· `dev_hold_pct`（<10%）· `creator_hold_pct` ·
`creator_status` string · `sniper_ratio` · `vault_ratio` · `smart_wallets` · `fresh_wallets` ·
`sniper_wallets` · `whale_wallets` · `bundler_wallets` · `rat_wallets` · `bundle_ratio` ·
`rug_risk` · `bot_ratio` · `fresh_wallet_ratio` · `fishing_ratio` · `locked_ratio`

### 安全审计

`honeypot` bool（关键）· `blacklist` bool · `mint_renounced` bool（关键）· `renounced` bool ·
`sellable` · `lp_burned_ratio`（关键）· `buy_tax`（关键）· `sell_tax`（关键）· `avg_tax` ·
`open_source` bool

### 发行 / 时间线

`created_at` · `opened_at` · `migrated_at` · `age_hours` · `launchpad` string (flap…) ·
`launchpad_progress` (0-1) · `creator_address` · `community_takeover` bool

### 社交

`twitter` string（纯句柄）· `website` string · `telegram` string

### 供应量

`circulating_supply` · `total_supply` · `max_supply`

### `rpc` 段（security 分类附带 · 链上独立核验）

| Field | Type | Meaning |
|---|---|---|
| `mint_renounced_rpc` | bool \| null | Owner renounced to a dead address — verified directly on BSC RPC |
| `lp_burned_rpc` | bool \| null | Dead-address LP ratio reaches the threshold (null = undeterminable) |
| `honeypot_rpc` | bool \| null | RPC honeypot check (currently null, needs state_override nodes) |
| `price_rpc` | number \| null | USD price derived from PancakeSwap reserves (null = no V2 pool) |

RPC verification is an independent second source — it raises confidence, **it is not an audit**.

### `twitter_info` 段（social 分类附带）

`username` · `name` · `followers` · `following` · `tweets_count` · `blue_verified` ·
`created_at` · `description`

### `twitter_tweet` 段（social 分类附带）

`id` · `username` · `text`（前 200 字）· `created_at` · `url` · `invalid` bool
（true = 已删帖 / 账号注销）。这是**该 CA 指定的官方宣传推文**（按 id 精确拉取），并非账号最新推文。

### `traders` 段（动向，2 credits）

`smart_buy` / `smart_sell` — 聪明钱上下车**钱包数** · `smart_net` — 净买入额 USD ·
`kol_buy` / `kol_sell` / `kol_net` · `kol_count` · `kol_names`

**官方号深层画像**（聚合 basic 附带）：`twitter_rename_count`（改名次数）·
`twitter_deleted_tweet_count`（删帖数）· `twitter_created_token_count`（发币数）——页面本身看不到的历史。

**单位**：上车 / 下车是**钱包地址数**，不是交易次数。

---

## Listings item — `POST /api/v1/token/trending` · `hot`（60 字段恒定）

`address` · `symbol` / `name` · `creation_tool` · `launchpad` · `price` · `mcap` · `ath_mcap` ·
`liquidity` · `initial_liquidity` · `chg_1m_pct` / `chg_5m_pct` / `chg_1h_pct` · `volume_24h` ·
`buys_24h` / `sells_24h` · `swaps_24h` · `net_buy_24h` · `holder_count` · `top10_holder_pct` ·
`smart_trader_count` · `bot_trader_count` / `bot_trader_ratio` · `sniper_count` / `sniper_ratio` ·
`fresh_wallet_ratio` · `bundler_ratio` · `suspicious_trader_ratio` · `insider_hold_pct` ·
`dev_hold_pct` / `creator_hold_pct` · `rug_risk` · `wash_trading` bool · `honeypot` bool ·
`mint_renounced` bool · `open_source` bool · `lp_burned` bool · `burn_ratio` / `lp_locked_pct` ·
`buy_tax` / `sell_tax` · `total_buy_tax` / `total_sell_tax` / `total_fee` · `community_takeover` ·
`creator_closed` · `dev_burn_ratio` · `created_at` / `opened_at` / `completed_at` ·
`launchpad_progress` · `creator_address` · `creator_status` · `twitter` · `twitter_followers` /
`twitter_following` · `telegram` / `website` · `total_supply` · `rank`（hot 热榜排名）

trending 无涨跌幅 / ATH 市值 / 热榜排名；hot 无净买入 / 毕业时间 / 发射进度 / 粉丝数。

---

## Trade record item — `POST /api/v1/track/trades`（26 字段恒定）

`id` / `tx_hash` · `side` (buy/sell) · `price` / `price_now` · `price_change_pct` ·
`amount_usd` / `cost_usd` / `buy_cost_usd` · `base_amount` / `quote_amount` · `token_amount` /
`balance` · `timestamp` · `is_open_or_close` · `launchpad` / `migrated_exchange` ·
`token_address` / `token_symbol` · `token_total_supply` / `token_created_at` / `token_opened_at` ·
`maker_address` / `maker_name` · `maker_twitter` · `maker_tags`（已剔除平台名）

`tx_hash` 裁剪时恒保留。

---

## Signal item — `POST /api/v1/market/signals`（45 字段恒定）

信号元数据：`id` · `signal_type` · `signal_name` · `trigger_at` · `trigger_mcap` ·
`first_trigger_mcap` · `signal_times` / `signal_times_by_type` ·
`kol_buyers`（type-20 触发 KOL，每位 `wallet` + `twitter_username`，最多 20 个，不含 CDN 字段）

触发时刻代币快照：`address` / `symbol` / `name` · `mcap` / `ath_mcap` · `price` / `liquidity` ·
`holder_count` / `top10_holder_pct` · `smart_trader_count` / `bot_trader_count` /
`bot_trader_ratio` / `sniper_count` · `bundler_ratio` / `suspicious_trader_ratio` /
`creator_hold_pct` · `rug_risk` / `buy_tax` / `sell_tax` / `total_fee` ·
`volume_1h` / `buys_1h` / `sells_1h` / `net_buy_1h` · `volume_24h` / `buys_24h` / `sells_24h` /
`swaps_24h` / `net_buy_24h` · `launchpad` / `launchpad_progress` · `creator_address` /
`creator_status` · `created_at` / `opened_at` / `total_supply` / `dev_burn_ratio`

`address` 裁剪时恒保留。

---

## Wallet — `POST /api/v1/wallet/{action}`

### `profile`（34 字段恒定 = stats + profits + 身份）

`wallet` / `period` · `native_balance` · `buy_count` / `sell_count` · `realized_profit` /
`realized_profit_pnl` · `bought_cost` / `bought_fee` / `sold_income` / `sold_fee` / `total_cost` ·
`last_trade_at` · `token_count` / `winrate` / `avg_holding_period` ·
`pnl_lt_nd5` / `pnl_nd5_0x` / `pnl_0x_2x` / `pnl_2x_5x` / `pnl_gt_5x`（盈亏分布笔数）·
`unrealized_profit` / `unrealized_profit_cost` / `total_realized_profit` /
`total_realized_profit_cost` / `total_profit` · `name` / `twitter` / `twitter_followers` /
`blue_verified` · `tags`（已中性化）/ `ens` / `created_at` / `creator_token_count`

### `activity` 流水 item（17 字段恒定，cursor 翻页）

`tx_hash` / `event_type` / `timestamp` · `token_address` / `token_symbol` /
`token_total_supply` · `token_amount` / `quote_amount` / `quote_symbol` · `price` / `cost_usd` /
`buy_cost_usd` · `is_open_or_close` / `launchpad` · `from_address` / `to_address` / `gas_usd`

### `created` 发币 item（17 字段恒定 + best_record）

`token_address` / `symbol` / `create_timestamp` · `is_open` / `is_pump` /
`community_takeover` · `market_cap` / `ath_mcap` / `holders` · `pool_liquidity` /
`biggest_pool` · `swap_1h` / `volume_1h` / `bundler_ratio` / `total_fee` ·
`coin_creator_fee` / `coin_creator_fee_claimable`

响应另附 `best_record`（历史最佳战绩代币）与 `inner_count` / `open_count` / `open_ratio` 汇总。

### `balance` 单币余额（6 字段恒定）

`wallet` / `token`（必填参数，缺失 422）· `balance` / `decimal` · `block_height` / `tx_index`

---

## 中文说明 · 字段字典

数据层返回稳定、中性的契约：**字段恒定输出（缺值 null）、字段名永不改变**。以下为各端点族的完整字段集。

### 聚合端点

**基础**：`address` · `symbol` · `name` · `decimals` · `standard`（erc20）· `contract_verified` · `description`

**价格/市值/涨跌**：`price` · `mcap` · `ath_price` · `price_1m/5m/1h/6h/24h` · `chg_5m_pct` · `chg_1h_pct` · `chg_24h_pct` · `buy_sell_ratio_24h`

**流动性/池子**：`liquidity` · `pool_liquidity` · `exchange` · `quote_symbol` · `pool_address` · `initial_liquidity` · `base_reserve` · `quote_reserve`

**交易量/活跃**：`volume_1h` · `volume_24h` · `buys_24h` · `sells_24h` · `buy_volume_24h` · `sell_volume_24h` · `swaps_24h` · `trade_fee`

**持仓/筹码**：`holder_count` · `top10_holders` · `dev_hold_pct` · `creator_hold_pct` · `creator_status` · `sniper_ratio` · `vault_ratio` · `smart_wallets` · `fresh_wallets` · `sniper_wallets` · `whale_wallets` · `bundler_wallets` · `rat_wallets` · `bundle_ratio` · `rug_risk` · `bot_ratio` · `fresh_wallet_ratio` · `fishing_ratio` · `locked_ratio`

**安全**：`honeypot` · `blacklist` · `mint_renounced` · `renounced` · `sellable` · `lp_burned_ratio` · `buy_tax` · `sell_tax` · `avg_tax` · `open_source`

**发行/时间线**：`created_at` · `opened_at` · `migrated_at` · `age_hours` · `launchpad` · `launchpad_progress` · `creator_address` · `community_takeover`

**社交**：`twitter`（纯句柄）· `website` · `telegram`　**供应**：`circulating_supply` · `total_supply` · `max_supply`

**rpc 段**（security 附带）：`mint_renounced_rpc` · `lp_burned_rpc` · `honeypot_rpc` · `price_rpc`——链上独立核验，**不是审计级保证**。

**twitter_info 段**：`username` · `name` · `followers` · `following` · `tweets_count` · `blue_verified` · `created_at` · `description`

**twitter_tweet 段**：`id` · `username` · `text` · `created_at` · `url` · `invalid`（已删帖/注销）——该 CA 指定官方宣传推文。

**traders 段**：`smart_buy/smart_sell/smart_net` · `kol_buy/kol_sell/kol_net` · `kol_count` · `kol_names`——上下车为**钱包数**。
官方号深层画像：`twitter_rename_count` · `twitter_deleted_tweet_count` · `twitter_created_token_count`。

### 榜单 item（60 字段）

`address` · `symbol/name` · `creation_tool` · `launchpad` · `price` · `mcap` · `ath_mcap` · `liquidity` · `initial_liquidity` · `chg_1m/5m/1h_pct` · `volume_24h` · `buys_24h/sells_24h` · `swaps_24h` · `net_buy_24h` · `holder_count` · `top10_holder_pct` · `smart_trader_count` · `bot_trader_count/ratio` · `sniper_count/ratio` · `fresh_wallet_ratio` · `bundler_ratio` · `suspicious_trader_ratio` · `insider_hold_pct` · `dev/creator_hold_pct` · `rug_risk` · `wash_trading` · `honeypot` · `mint_renounced` · `open_source` · `lp_burned` · `burn_ratio/lp_locked_pct` · `buy/sell_tax` · `total_buy/sell_tax` · `total_fee` · `community_takeover` · `creator_closed` · `dev_burn_ratio` · `created/opened/completed_at` · `launchpad_progress` · `creator_address` · `creator_status` · `twitter` · `twitter_followers/following` · `telegram/website` · `total_supply` · `rank`

### 交易 item（26 字段）

`id/tx_hash` · `side` · `price/price_now` · `price_change_pct` · `amount_usd/cost_usd/buy_cost_usd` · `base/quote_amount` · `token_amount/balance` · `timestamp` · `is_open_or_close` · `launchpad/migrated_exchange` · `token_address/symbol` · `token_total_supply/created_at/opened_at` · `maker_address/name` · `maker_twitter` · `maker_tags`（中性化）

### 信号 item（45 字段）

信号元数据：`id` · `signal_type/name` · `trigger_at/mcap/first_trigger_mcap` · `signal_times/by_type` · `kol_buyers`（type-20，wallet + twitter_username，最多 20，无 CDN）
快照：`address/symbol/name` · `mcap/ath_mcap` · `price/liquidity` · `holder_count/top10_holder_pct` · `smart/bot/sniper` 画像 · `bundler/suspicious/creator_hold` 占比 · `rug_risk/buy_tax/sell_tax/total_fee` · `volume/buys/sells/swaps/net_buy`（1h + 24h）· `launchpad/progress` · `creator_address/status` · `created_at/opened_at/total_supply/dev_burn_ratio`

### 钱包

**profile（34）**：`wallet/period` · `native_balance` · `buy/sell_count` · `realized_profit(+pnl)` · `bought_cost/bought_fee/sold_income/sold_fee/total_cost` · `last_trade_at` · `token_count/winrate/avg_holding_period` · `pnl_lt_nd5/pnl_nd5_0x/pnl_0x_2x/pnl_2x_5x/pnl_gt_5x` · `unrealized_profit(+cost)/total_realized_profit(+cost)/total_profit` · `name/twitter/twitter_followers/blue_verified` · `tags/ens/created_at/creator_token_count`

**activity（17）**：`tx_hash/event_type/timestamp` · `token_address/symbol/total_supply` · `token/quote_amount` · `quote_symbol` · `price/cost_usd/buy_cost_usd` · `is_open_or_close/launchpad` · `from/to_address` · `gas_usd`

**created（17 + best_record）**：`token_address/symbol/create_timestamp` · `is_open/is_pump/community_takeover` · `market_cap/ath_mcap/holders` · `pool_liquidity/biggest_pool` · `swap_1h/volume_1h/bundler_ratio/total_fee` · `coin_creator_fee(+claimable)`

**balance（6）**：`wallet/token` · `balance/decimal` · `block_height/tx_index`
