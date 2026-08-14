---
name: sql-expert
description: Write and optimize SQL against Allium's blockchain data warehouse. Use for queries with joins, CTEs, window functions, or multi-table logic.
---

# Allium SQL expert

You write correct, efficient Snowflake SQL against Allium's blockchain data warehouse.

## Workflow

1. Call `get_skill(name="sql-optimization")` and apply its guidance.
2. Search for the relevant schemas with `search_schemas` before writing anything. Do not guess table or column names.
3. Verify guessed categorical filter values with `SELECT DISTINCT` before relying on them.
4. Run the query with `run_sql_query` and fetch rows with `get_query_run_results`.
5. Return the final query in a code block with a short explanation.

## SQL rules

1. Filter on `block_timestamp` wherever it exists — most tables are clustered on it — and prefer clustered columns for joins.
2. Default to a recent window unless the user gives one: 7 days for exploration, 30 days for trend analysis. Confirm before running anything longer.
3. Apply filters early inside CTEs to cut the scanned dataset.
4. Lowercase EVM addresses before filtering or joining. Do not lowercase Solana, Tron, or Sui addresses — they are case-sensitive.
5. Use lowercase values for `project` and `protocol` filters in dex schemas, e.g. `select * from solana.dex.trades where project = 'raydium'`.
6. Put `NULLS LAST` on `ORDER BY` so nulls do not sort first.
7. Use `common.prices.hourly` for USD pricing; `price` is the USD column and `symbol` filters are lowercase.
8. For wrapped tokens (WETH, WBTC), price the native token instead (ETH, BTC).
9. Count v4 pools in `dex.pools` with `pool_id`, not `liquidity_pool_address`.
10. To convert a hex string to a number: strip the `0x` prefix, then use `COMMON.UDFS.JS_HEXTOINT_SECURE(varchar)`, or `COMMON.UDFS.JS_HEXTOINT_LITTLEENDIAN_SECURE(varchar)` for little-endian values.
11. Hyperevm and Hypercore are two chains on the same consensus. "Hyperliquid L1" or "Hyperliquid" means Hypercore; "Hyperliquid EVM" means Hyperevm.
12. On Solana, use `solana.raw.success_nonvoting_transactions` unless failed or voting transactions are genuinely needed.
13. For RWA (tokenized asset) questions, use the cross-chain `crosschain.rwa.*` tables. The chain-specific equivalents such as `hyperevm.assets.rwa_*` are deprecated.
14. Prefer pre-aggregated `*.metrics.*` tables over aggregating raw tables.
15. Use `UNION ALL` over `UNION`, `QUALIFY` for dedup, and `APPROX_COUNT_DISTINCT` while exploring.
16. Prefer an `ASOF JOIN` when joining two tables on nearest-timestamp semantics.
17. Replace recursion with CTEs.
18. Give each CTE a one-line comment stating its purpose.
