# Twitter Data API

12 read endpoints for users / timelines / follower networks / search / replies / quotes /
retweets / threads. One key, one balance, uniform billing.

**Endpoint:** `POST https://www.tgtcbot.com/api/v1/twitter/{action}`
**Billing:** **5 credits per call** for every action · cache hits (120s) free · 422 never deducts

---

## Actions & params

| Action | Purpose | Key params |
|---|---|---|
| `user.info` | User profile (followers, blue check, created…) | `username` / `user_id` |
| `user.tweets` | Recent tweets | `username` / `user_id`, `count` (1–100) |
| `user.timeline` | Home timeline | `username` / `user_id`, `count` |
| `user.followers` | Follower list | `username` / `user_id`, `count`, `cursor` |
| `user.followings` | Following list | `username` / `user_id`, `count`, `cursor` |
| `user.search` | Search users by keyword | `query`, `count` |
| `tweet.search` | Advanced tweet search | `query`, `count`, `sort` |
| `tweet.detail` | Single tweet | `tweet_id` |
| `tweet.replies` | Replies to a tweet | `tweet_id`, `count` |
| `tweet.quotes` | Quote tweets | `tweet_id`, `count` |
| `tweet.retweets` | Retweeters | `tweet_id`, `count` |
| `tweet.thread` | Full tweet thread | `tweet_id` |

### Common params

| Param | Type | Notes |
|---|---|---|
| `username` / `user_id` | string | at least one for user actions |
| `query` | string | search keyword |
| `tweet_id` / `tweet_ids` | string | single / batch detail |
| `count` | int | 1–100 |
| `cursor` | string | pagination — a new `cursor` is a **new request** (5 credits, no cache hit) |
| `include_replies` | bool | user.tweets |
| `sort` | string | tweet.search |

## Pagination warning

List endpoints (`followers` / `followings`) and deep search cost real money when pages add up:
every page bills **5 credits** and never hits cache. **100k results ≈ 15,000 credits** — estimate
the volume before pulling.

## Response contract

**Tweet object** (element of `tweets` arrays) — always present, missing → `null`:

`id` string · `text` string（前 500 字）· `created_at` string (UTC) · `author` object
（`id / username / name / verified`）· `retweet_count` / `like_count` / `reply_count` /
`quote_count` / `view_count` number · `language` string · `media` object[]（至多 4 个，
仅 `type / duration_ms`，不暴露原站 CDN 链接）· `url` string

**User object** (`user.info`) — `id / username / name / bio / bio_link / location / protected /
verified / blue_verified / verified_type / verified_category / followers_count /
following_count / tweets_count / media_count / likes_count / created_at / can_dm /
is_automated / automated_by / pinned_tweet_ids / website`

**List responses** carry a `pagination` segment (cursor) — a new `cursor` is a new request
(5 credits, no cache hit). Deleted tweets / unknown users → `404`. Fields are neutralized
(no platform names, no CDN links, no internal fingerprints).

---

## 中文说明 · 推特数据 API

12 个读端点：用户 / 时间线 / 粉丝网络 / 搜索 / 回复 / 引用 / 转发 / 推文串。一把 Key、统一计费。

**端点**：`POST https://www.tgtcbot.com/api/v1/twitter/{action}`
**计费**：所有 action 统一 **5 次/次** · 120s 缓存命中不扣次 · 422 不扣

### 动作与参数

| 动作 | 用途 | 关键参数 |
|---|---|---|
| `user.info` | 用户资料（粉丝 / 蓝V / 注册时间…） | `username` / `user_id` |
| `user.tweets` | 最近推文 | `username` / `user_id`、`count`（1~100） |
| `user.timeline` | 主页时间线 | `username` / `user_id`、`count` |
| `user.followers` | 粉丝列表 | `username` / `user_id`、`count`、`cursor` |
| `user.followings` | 关注列表 | `username` / `user_id`、`count`、`cursor` |
| `user.search` | 按关键词搜用户 | `query`、`count` |
| `tweet.search` | 高级搜索推文 | `query`、`count`、`sort` |
| `tweet.detail` | 单条推文 | `tweet_id` |
| `tweet.replies` | 推文回复 | `tweet_id`、`count` |
| `tweet.quotes` | 引用推文 | `tweet_id`、`count` |
| `tweet.retweets` | 转发用户 | `tweet_id`、`count` |
| `tweet.thread` | 完整推文串 | `tweet_id` |

### 通用参数

`username` / `user_id`（用户类动作至少其一）、`query`（搜索词）、`tweet_id` / `tweet_ids`（单条/批量详情）、
`count`（1~100）、`cursor`（翻页——新 cursor = **新请求**，5 次、不命中缓存）、`include_replies`（user.tweets）、`sort`（tweet.search）。

### 翻页成本警告

列表类（followers / followings）与深翻搜索按页计费：**每页 5 次**、永不命中缓存。
**10 万条结果 ≈ 15,000 次**——拉取前先评估量级。

### 响应契约

**推文对象**（tweets 数组元素，恒定字段、缺值 null）：

`id` string · `text` string（前 500 字）· `created_at` string（UTC）· `author` object（`id / username / name / verified`）·
`retweet_count` / `like_count` / `reply_count` / `quote_count` / `view_count` number · `language` string ·
`media` object[]（至多 4 个，仅 `type / duration_ms`，不暴露原站 CDN 链接）· `url` string

**用户对象**（user.info）：`id / username / name / bio / bio_link / location / protected / verified /
blue_verified / verified_type / verified_category / followers_count / following_count / tweets_count /
media_count / likes_count / created_at / can_dm / is_automated / automated_by / pinned_tweet_ids / website`

**列表响应**统一携带 `pagination` 段（cursor 翻页）——新 cursor = 新请求（5 次、不命中缓存）。
推文已删 / 用户不存在 → `404`。字段已中性化（无平台名、无 CDN 链接、无内部指纹）。
