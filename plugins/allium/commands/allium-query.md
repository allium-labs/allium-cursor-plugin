---
name: allium-query
description: Answer a blockchain data question with SQL against Allium.
---

Answer this question with Allium data: $ARGUMENTS

1. Call `get_skill(name="sql-optimization")`.
2. Find the tables with `search_schemas`.
3. Write the query, defaulting to a 7-day window on `block_timestamp` unless the question implies otherwise.
4. Run it with `run_sql_query`, fetch rows with `get_query_run_results`.
5. Report the answer with the query and the row count behind it.
