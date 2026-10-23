---
name: sql-optimization
description: |
  **Required reading before writing or running any SQL.** Patterns for Snowflake
  query performance and chain-specific pitfalls.

  Covers: never querying deprecated tables, default time ranges, partition-pruning rules
  (literal dates, not subqueries), CTE filtering, `QUALIFY` for dedup, `UNION ALL`
  over `UNION`, `APPROX_COUNT_DISTINCT` for exploration, why `LIMIT` doesn't cut the
  scan, daily balance views, filtering before `FLATTEN`, rolling windows,
  pre-aggregated `*.metrics.*` tables vs raw aggregation,
  unadjusted DEX volume, EVM address lowercasing (and when not to), per-chain vs
  `crosschain.*` tables, verifying guessed categorical filter values with
  `SELECT DISTINCT`, and Solana voting/non-voting optimized views.

  Call `get_skill(name="sql-optimization")` before writing SQL —
  especially for large tables, long time ranges, or crosschain queries.
---

# SQL Optimization for Snowflake

Apply these patterns when writing or optimizing SQL against Allium's Snowflake
warehouse, especially for multi-billion-row tables (`*.raw.transactions`,
`*.assets.erc20_token_transfers`, `*.raw.logs`, `*.dex.trades`) and views over them
(`*.assets.fungible_balances_daily`).

## Verify Guessed Categorical Filters

Before committing to guessed categorical values in a `WHERE` clause, run a
quick exploration query to verify the values present in the table. Use schema
descriptions to choose the table and column; use data exploration when the
exact stored value is not already confirmed.

**For categorical/text columns** you plan to filter on (e.g., `coin`,
`market_type`, `pair`, `status`, `type`, `dex_name`, `chain`):

- Run `SELECT DISTINCT <column> FROM <table> [WHERE <recent_timestamp_filter>] LIMIT 50` first
- On large tables, scope with a recent timestamp filter (e.g., `WHERE timestamp >= DATEADD('day', -7, CURRENT_DATE())`) to keep exploration cheap
- For small metadata/dimension tables, a bare `SELECT DISTINCT` is fine

If schema content or sample rows show the exact stored value, use that spelling.
If they do not, verify the value from the table before using it as a filter.

**If a query unexpectedly returns 0 rows, check categorical filters before
concluding the data is missing.** Re-run with `SELECT DISTINCT` or a `LIKE
'%term%'` pattern.

## Never Query a Deprecated Table

A `search_schemas` hit with `deprecated: true` is retired. It still returns
rows, but the data is frozen or incomplete, so the query succeeds and the
answer is wrong. A query that runs cleanly does not prove the table is
current.

- Call `search_docs` on the table name to get the reason the table was retired
  and the table that replaced it. Most deprecated tables have no
  `replacement_table` value, so the docs are usually the only place the
  current table is named. Use `replacement_table` when the hit has one. The
  [deprecation and migration guide](https://docs.allium.so/changelog/deprecated-schemas)
  lists every retired schema with its replacement.
- Check every table in the query, including the tables in `JOIN`s. If a table
  did not come from a `search_schemas` hit in this session, look it up with
  `search_schemas(id="<db>.<schema>.<table>")` before you use it.
- If the user asks for a deprecated table, tell them it is retired and why,
  then query the replacement.

## SQL Pitfalls (Always Apply)

**Date-filter every query on large tables.** Most large tables
(`*.raw.transactions`, `*.assets.erc20_token_transfers`, `*.dex.trades`, `*.raw.logs`) are
clustered on `block_timestamp::date` or `timestamp::date`. Without a date
filter, queries scan the entire table — billions of rows, slow and expensive.
Always include a bounded date range (e.g., `WHERE block_timestamp >=
'2026-01-01'`). If the user didn't specify a range, pick a default (last 30
days for activity, last 1 day for current state) and state the assumption.

**Default to recent time windows when the user is silent:**

- **7 days** (1 week) — for exploratory queries, quick analysis
- **30 days** (1 month) — for trend analysis, aggregations

```sql
-- 1 week (default for exploration)
WHERE block_timestamp >= DATEADD(day, -7, CURRENT_DATE())

-- 1 month (for trends)
WHERE block_timestamp >= DATEADD(day, -30, CURRENT_DATE())
```

If the user needs data beyond 30 days: confirm first ("This query covers a
large time range and may be slow. Is that okay?"), consider breaking into
smaller batches (e.g., monthly), or use more aggressive filtering on other
columns.

**Lowercase EVM addresses before filtering or joining — but only EVM.** Allium
stores EVM addresses (`0x...` hex) lowercase, while users and block explorers
often paste them checksummed. `WHERE from_address = '0xAbC123...'` on an EVM
table silently returns 0 rows; apply `LOWER()` to user-supplied EVM addresses.
Lowercase the value, never the column: `WHERE address = LOWER('0xAbC123...')`.
The column is already lowercase, and wrapping a large table's column in a
function, as in `LOWER(address) IN (...)`, stops Snowflake from using pruning
and search optimization on it. Lowercasing the small side of a join is fine.
**Do NOT lowercase non-EVM addresses.** Solana, Tron, Sui, and other non-EVM
chains store addresses in their native case-sensitive form (e.g., Solana
base58 `HyperSPG8w4j...`); lowercasing changes the address's meaning. In
`crosschain.*` tables, key on `chain` to decide whether to lowercase the value
you compare against.

**Prefer per-chain tables; crosschain joins need `(chain, address)`.** The
`crosschain.*` schemas are convenient but significantly slower than per-chain
tables, especially when Solana is involved. When the user's question targets a
single chain, use the per-chain table (e.g., `ethereum.dex.trades`) instead of
`crosschain.dex.trades`. Reach for `crosschain.*` only when actually comparing
across 3+ chains. When you do use crosschain tables, the same address (e.g.,
USDC) exists on many chains with different state — always join on `(chain,
address)`, not address alone, and lowercase the address only when the row's
chain is EVM.

**Price tokens by `(chain, address)`, not by symbol.** Many tokens share a
symbol, and spam tokens with a popular symbol such as `usdt` carry absurd
prices. A filter on `symbol` alone mixes their prices with the price of the
real token. In `common.prices.hourly`, filter or join on `chain` and the
contract address of the token. Lowercase the EVM address value, not the
`address` column. For stablecoins, get the address from
`stablecoins.registry.deployments`. Use the row where `is_native = true`,
which is the deployment of the issuer. Some chains have only bridged
deployments, such as USDT on `arbitrum`, `polygon`, and `bsc`. On those
chains, select the bridged row by `product_id`, such as `usdt0_layerzero` or
`usdt_binance_peg`. Do not go back to a filter on `symbol`. For the gas token
of a chain, such as ETH on `ethereum` or SOL on `solana`, filter on `chain`
and `is_native = true` in `common.prices.hourly`. Do not use one fixed
address for all chains, because the gas token address is different on some
chains, such as `zksync` and `solana`. For a wrapped token such as WETH,
price the native token instead.

```sql
SELECT timestamp, price
FROM common.prices.hourly
WHERE chain = 'ethereum'
  AND address = '0xdac17f958d2ee523a2206206994597c13d831ec7'
  AND timestamp >= DATEADD(day, -7, CURRENT_DATE())
```

## Prefer Pre-Aggregated Metrics Tables

When a question can be answered from a `*.metrics.*` table, use it instead of
aggregating from raw. Daily aggregates (e.g., `hyperliquid.metrics.overview`,
`crosschain.metrics.overview`) are orders of
magnitude faster and cheaper than `SUM(...) GROUP BY day` over raw trades,
transfers, or transactions.

When the user asks for a metric — volume, TVL, trade count, active users,
fees, market cap, open interest, etc. — `search_schemas` for a `metrics` table
first. Only fall back to raw tables when the metrics table lacks the dimension
or granularity the question needs (e.g., per-wallet breakdown, sub-daily
resolution).

DEX volume, whether summed from `*.dex.trades` or read from `dex_volume_usd` in
a metrics table, includes wash trading and trades priced off manipulated
tokens, so it can be several times what public trackers such as DefiLlama
report. When you report DEX volume, say that it is unadjusted.

One layer above that, the metrics catalog answers some questions with no SQL at
all. If `list_catalog_metrics` is available, call it before writing a query
about stablecoins, RWAs, issuers, lending, DEXes or payments, and read any
matching metric with `get_catalog_metric`. Write SQL when the catalog has no
metric for the subject, or when the question needs a dimension, granularity or
join the catalog does not expose.

## Snowflake Performance Patterns

Apply these patterns when writing queries against multi-billion-row tables.

### Filter in CTEs before joining

Pre-reduce each table in its own CTE before the join. Joining two
already-filtered datasets is vastly cheaper than filtering the joined result.
Especially effective when filtering on the timestamp column.

```sql
-- bad: query engine *might not* push block_timestamp down to table_b
with
cte_a as (select * from table_a where block_timestamp between '2025-01-01' and '2025-01-02'),
cte_b as (select * from table_b)
select * from cte_a join cte_b using (block_timestamp);

-- good: filter both sides explicitly
with
cte_a as (select * from table_a where block_timestamp between '2025-01-01' and '2025-01-02'),
cte_b as (
  select * from table_b
  where block_timestamp between '2025-01-01' and '2025-01-02'
)
select * from cte_a join cte_b using (block_timestamp);
```

### Partition pruning does NOT work with subquery values

Snowflake's docs say it does not prune on a predicate with a subquery, even
when the subquery returns a single constant
([snowflake docs](https://docs.snowflake.com/en/user-guide/tables-clustering-micropartitions#query-pruning)).
Some warehouses work out the bound while the query runs and prune anyway, but
that depends on the warehouse type and the Snowflake release, so a query
written this way can run fast for months and then slow down by orders of
magnitude. In one customer case, a 3-day window on
`arbitrum.assets.fungible_balances_daily` written as
`date >= (select max(date) - interval '3 day' from my_table)` read
about 73 billion rows. The same window as a literal read about 390 million.

This applies to every time-filtered table and view, whatever the column is
called (`block_timestamp`, `timestamp`, `date`). Views such as
`*.assets.fungible_balances_daily` show no clustering key in `search_schemas`,
but they still need a literal date filter. Expressions on constants such as
`DATEADD(day, -7, CURRENT_DATE())` are fine.

When the cutoff depends on data, run the lookup as its own query first, then
put the result into the main query as a literal:

```sql
-- bad: pruning doesn't fire
select * from table_a where block_timestamp >= (select min(timestamp) from table_b);

-- good: run `select min(timestamp) from table_b` first, then inline the result
select * from table_a where block_timestamp >= '2025-01-01';
```

Some views prune on a column other than `block_timestamp`. In
`stablecoins.intelligence`, `organic_activity_classification` and
`payment_categorization` are clustered on `activity_date`,
`enriched_transfers` on `token_symbol, block_timestamp::date`, and
`central_exchange_flows_classification` on `block_timestamp::date`. Filter
each on its own key.

### Join the big table once

When looking up multiple address sets against a large table, `UNION ALL` the
lookups into a single CTE first, then join the big table only once — rather
than running multiple joins or repeated queries.

### Use `QUALIFY` for latest-per-group / dedup

Snowflake's `QUALIFY` filters window function results inline — no wrapper CTE
needed, often faster:

```sql
SELECT token_address, block_timestamp, price
FROM ethereum.dex.trades
WHERE block_timestamp >= DATEADD('day', -7, CURRENT_DATE())
QUALIFY ROW_NUMBER() OVER (PARTITION BY token_address ORDER BY block_timestamp DESC) = 1;
```

### `UNION ALL`, not `UNION`

Plain `UNION` runs an implicit `DISTINCT` across the full result — an
expensive shuffle. Use `UNION ALL` unless you specifically need to dedupe
across the two sides.

### `APPROX_COUNT_DISTINCT` for exploration

Exact `COUNT(DISTINCT ...)` on billions of rows is slow (full shuffle). HLL
approximation is orders of magnitude faster with ~1% error — fine for
exploratory analysis where a ballpark is enough.

### Batch large time ranges

When querying a large time range, concurrent queries of smaller time intervals
might work better — e.g., when querying a year's data, 12 batches of 1 month
each is probably faster than one yearly query.

### Clustering and join conditions

Most time-based tables are clustered by `block_timestamp::date`, so it's
useful to have `block_timestamp` in join conditions. Filter on the clustered
column first:

```sql
-- good: filters on clustered column first
SELECT * FROM ethereum.raw.transactions
WHERE block_timestamp >= '2024-01-01'
  AND from_address = '0x...';
```

### Select only the columns you need

Avoid `SELECT *` on wide tables. Snowflake is columnar — pruning unread
columns directly reduces scan cost.

On views that add USD values (`usd_*` columns), the price join can run at query
time and scan the whole price table even when the date filter prunes the main
table. Leave the `usd_*` columns out when the question doesn't need them, so
Snowflake can skip the price join.

### `LIMIT` does not reduce the scan

`LIMIT` caps the rows returned, not the rows read. `SELECT * FROM <view> LIMIT
10` with no date filter can still scan the full history and time out. To sample
a large table or view, filter on its date column first. To look up specific
wallets, inner-join an explicit address list.

### Daily balance views are expensive

`*.assets.fungible_balances_daily` and the Solana `sol_balances_daily` and
`spl_token_balances_daily` are views that forward-fill a row for every wallet,
token and day from end-of-day snapshots, then add USD prices. Without a tight
date filter they rebuild the full history, so:

- Filter `date` with a literal: a single date, or the shortest range that
  answers the question.
- Inner-join an explicit address list when the question is about specific
  wallets.
- Select only the columns you need (see above).

The Solana views are slower still, even for a single day and a fixed address
list. Tell the user the query may take a long time before running one.

## Ecosystem-specific tips

### Solana

1. When querying raw entities such as transactions, instructions,
   inner_instructions, if you do not need voting data, use one of the
   corresponding optimized views:

   - transactions:
     - `success_nonvoting_transactions`
     - `nonvoting_transactions`
   - instructions:
     - `success_nonvoting_instructions`
     - `nonvoting_instructions`
   - inner_instructions:
     - `success_nonvoting_inner_instructions`
     - `nonvoting_inner_instructions`
   - inner_outer_instructions:
     - `success_nonvoting_inner_outer_instructions`
     - `nonvoting_inner_outer_instructions`

2. Filter out failed/success and voting/nonvoting records:

   - for transactions, use `success` and `is_voting`
   - for (inner)instructions, use `parent_tx_success` and `is_voting`

3. For transaction meta columns, the rpc returns pre/post token/native
   balances. We have reformatted these cols to be more user-friendly in the
   columns `sol_amounts`, `mint_to_decimals`, `token_accounts` for your
   convenience (only available for `success_nonvoting_transactions`). For
   more info, refer to [transaction-level columns](https://docs.allium.so/historical-data/supported-blockchains/solana#transaction-level-columns).

4. For transaction fees, see [solana.raw.fees](https://docs.allium.so/historical-data/supported-blockchains/solana/raw/fees#fees);
   they are also available in [transaction-level columns](https://docs.allium.so/historical-data/supported-blockchains/solana#transaction-level-columns)
   for convenience.

## More Snowflake patterns

### Join on clustered columns

When a query `JOIN`s two tables, prefer joining on **clustered columns**. Most
time-based tables are clustered on `block_timestamp` (or `timestamp`), so
including that column in the join condition lets Snowflake prune partitions on
both sides. The **Clustered by** line in a table's `search_schemas` entry names
its clustering key; check it before choosing a join key.

### Prefer `ASOF JOIN` for nearest-time / padding joins

When joining two tables on the nearest preceding value (e.g. attaching the most
recent price to each trade, or filling padding values across time), use
Snowflake's `ASOF JOIN`. It is simpler and usually more efficient than
hand-rolled window-function or range-partitioning approaches.

### Avoid recursive CTEs

Avoid recursion in SQL. Recursive queries are hard to reason about without
knowledge of table cluster mappings and tend to perform poorly. Re-express the
logic with ordinary (non-recursive) CTEs instead.

### Filter and aggregate before `FLATTEN`

`FLATTEN` returns one row per array element, so filter and aggregate first,
then flatten. Use `RECURSIVE => TRUE` only on rows already filtered.

### Compute rolling windows in one pass

For a trailing window on every day of a range, such as each wallet's 30-day
volume, scan the range plus the window once, aggregate to one row per wallet
per day, then use a window function. Don't rerun the lookback per day.

### Convert hex strings to numbers with the secure UDFs

To convert a hex string to a number in Snowflake:

1. Strip the `0x` prefix from the hex string.
2. Use `COMMON.UDFS.JS_HEXTOINT_SECURE(<varchar>)` to convert it.
3. If the value is **little-endian** encoded, use
   `COMMON.UDFS.JS_HEXTOINT_LITTLEENDIAN_SECURE(<varchar>)` instead.

### dbt incremental models and partition pruning

Snowflake partition pruning does **not** fire on values from a subquery. In a
dbt model with `materialization='incremental'`, don't put
`max(timestamp) from {{ this }}` directly in the incremental `WHERE` — precompute
the timestamp into a literal first (e.g. a macro that queries `{{ this }}` and
returns a single value), then use that literal everywhere a time filter applies.

## SQL style conventions

These keep generated SQL correct and readable.

### Always specify `NULLS LAST` in `ORDER BY`

Snowflake sorts `NULL`s first by default for descending order, which surfaces
empty rows at the top. Add `NULLS LAST` to `ORDER BY` clauses so nulls don't
lead the result set:

```sql
ORDER BY usd_amount DESC NULLS LAST
```

### Comment every CTE

For each CTE, add a short comment at the top describing its purpose, the data it
processes, and any important transformations or filters. This makes multi-CTE
queries maintainable:

```sql
with
-- recent_trades: last 7 days of ethereum DEX trades, filtered early on the
-- clustered block_timestamp column to keep the scan small
recent_trades as (
    select * from ethereum.dex.trades
    where block_timestamp >= dateadd(day, -7, current_date())
)
select * from recent_trades;
```
