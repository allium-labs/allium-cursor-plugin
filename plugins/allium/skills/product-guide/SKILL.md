---
name: product-guide
description: |
  Guide to Allium's four core products: Explorer, Realtime APIs, Datastreams,
  and Datashares. Explains what each product does, who it's for, and how to
  choose the right one for a customer's use case.

  Read this skill when a customer asks about Allium's products, wants to
  understand the difference between them, needs help choosing a product, or
  asks questions like "which product should I use?" or "what does Allium offer?"
  It also carries the canonical, dated list of supported blockchains — read it
  for any "what chains / how many chains does Allium support?" question and cite
  that list instead of answering from memory.
---

# Allium Product Guide

Allium offers four core products for accessing blockchain data. Each serves a
different access pattern, latency requirement, and integration style. Explorer,
Datashares, and Datastreams share one historical data platform; the Realtime
APIs serve a smaller, separate set of chains.

> **Answering "what chains does Allium support?" (grounding rule)**
>
> Chain names and counts must come from the canonical list, never from memory —
> generating them from memory produces unstable totals and invented chains.
>
> 1. Fetch the **Allium Supported Chains — Canonical List** reference file (see
>    "Reference files" below) and quote it. For an up-to-the-minute Realtime
>    figure, call the `realtime_get_supported_chains` tool.
> 2. Always say **which product** a count refers to — the historical data
>    platform (Explorer / Datashares / Datastreams) and the Realtime APIs have
>    different totals.
> 3. The headline total must equal the number of rows for that product in the
>    canonical list. Do not round to a vague "80+/130+/150+" — cite the exact
>    count and state the list's snapshot date in the answer, so a reader can see
>    how current it is (e.g. "**[N]** chains on the historical data platform, as
>    of **[snapshot date]**"). Pull both **[N]** and the date from the fetched
>    reference file — never from this instruction or from memory.
> 4. If a chain is not in the canonical list, say it is not currently supported
>    rather than guessing.

## Product Overview

| Product           | Access Pattern                      | Latency                                | Pricing Model              | Best For                                            |
| ----------------- | ----------------------------------- | -------------------------------------- | -------------------------- | --------------------------------------------------- |
| **Explorer**      | SQL queries via web UI or API       | ~1 hour freshness, ~4-5s query time    | Query compute time         | Ad-hoc analysis, dashboards, research               |
| **Realtime APIs** | REST API (pull)                     | 50-100ms response, 3-5s data freshness | API call volume            | Application backends, wallets, trading UIs          |
| **Datastreams**   | Push via Kafka/PubSub/SNS/WebSocket | 3-5s data freshness                    | Data destinations & egress | Event-driven architectures, real-time pipelines     |
| **Datashares**    | Native tables in your warehouse     | 1-3 hours batch, sub-minute streaming  | Number of chains & schemas | Institutional analytics, joining with internal data |

## Explorer

**What it is:** A SQL-based analytics workspace at `app.allium.so/explorer` that
lets users query, visualize, and share blockchain data across every chain on the
historical data platform (see the canonical chain list for the exact count). Powered
by Snowflake (OLAP).

**Key capabilities:**

- Full SQL interface with cross-chain queries (e.g., `crosschain.dex.trades`)
- 1,000+ enriched schemas: raw, decoded, DEX, DeFi, NFTs, stablecoins, wallet 360, metrics, bridges, prices, PnL
- Interactive chart builder with public sharing and embed links
- CSV and API data upload — join your own data with on-chain data
- Parameterized queries for reusable, dynamic SQL
- AI Assistant for natural-language-to-SQL
- Explorer API — programmatic query lifecycle (create, run, fetch results, cancel)
- Curated metrics dashboards for stablecoins, DEXs, bridges, etc.

**Target users:**

- Analytics teams at crypto companies (foundations, protocols, wallets)
- Data analysts and researchers
- Audit, accounting, and compliance teams (Big 4 firms)
- Growth and marketing teams tracking ecosystem metrics

**Common use cases:**

- User behavior analytics and wallet activity patterns
- Ecosystem metrics and competitive intelligence
- Sybil detection for airdrop eligibility (used by Jito, Wormhole)
- Account reconciliation and auditing (used by Big 4 firms)
- DEX adoption dashboards (Uniswap Foundation)
- Financial monitoring and tax reporting (TaxBit)

**When to recommend Explorer:**

- Customer wants to explore data interactively with SQL
- Ad-hoc analysis, research, or one-off investigations
- Building shareable dashboards and visualizations
- Uploading proprietary data to join with on-chain data
- Needs the broadest chain coverage (the full historical data platform; see the
  canonical chain list) and deepest schema library (1,000+)
- Hourly data freshness is acceptable

## Realtime APIs

**What it is:** Production-grade REST APIs delivering real-time, enriched
blockchain data with 50-100ms response times and 3-5 second data freshness.
Realtime covers fewer chains than the historical data platform — quote the
canonical chain list (or the live `realtime_get_supported_chains` tool) for the
exact set and count.

**Key capabilities:**

- **Prices** — real-time and historical token prices from on-chain DEX trades (not centralized exchanges). OHLC candles, VWAP, z-score outlier filtering. New tokens priced within seconds of first DEX trade (including pump.fun launches)
- **Tokens** — metadata, type, price, decimals, FDV, volume, trade count, holders, ATH/ATL, liquidity, creation time. Sortable and searchable
- **Wallets** — current balances (native + ERC-20/SPL), historical balances at any point in time, full transaction history with activity labels (swaps, transfers, DEX trades)
- **Holdings** — real-time and historical USD portfolio holdings with built-in PnL using average cost basis. Multi-granularity (15s/5m/1h/1d)
- **Assets** — batch-fetch asset details by chain + address
- **Hyperliquid** — dedicated endpoints for the Hyperliquid protocol
- Custom endpoints backed by your own SQL queries

**Performance:**

- 50-100ms response time
- 1-1.2s raw block freshness, 3-5s enriched data freshness
- Tested to 90,000 RPS (Phantom/Jupiter airdrop: 477M requests in 4 hours)
- 99.9% uptime SLA

**Authentication:** API key via `X-API-KEY` header. Generate at Settings > API Keys.

**Target users:**

- Application developers building crypto products
- Wallet teams (Phantom, MetaMask)
- DEX/trading platforms (Uniswap, Fomo)
- Payment providers (MoonPay, Bridge.xyz)
- Fraud detection systems (Cube3.ai, Blowfish)
- AI agent builders

**When to recommend Realtime APIs:**

- Customer is building an application that needs to pull data on demand
- Needs sub-second response times for user-facing features
- Wallet balances, transaction history, token prices, or portfolio PnL
- Request-response pattern fits their architecture
- Needs instant coverage of new tokens (long-tail/pump.fun)
- Building token screeners, trading interfaces, or portfolio trackers

## Datastreams

**What it is:** Real-time push-based data delivery via enterprise message brokers.
Enriched, decoded blockchain data from the historical data platform (see the
canonical chain list for the exact count) streamed with 3-5 second latency and
guaranteed delivery.

**Key capabilities:**

- Delivery via **Kafka**, **Google Cloud Pub/Sub**, **Amazon SNS** (coming soon), **WebSockets**, and **webhooks**
- Guaranteed delivery and message ordering (Kafka/PubSub)
- Historical replay via retention policies
- **Stream Transformations** — managed filter-and-route pipelines:
  - Data source filters (dynamic address/contract lists, updateable without restart)
  - Declarative JSON filters with `=`, `!=`, `>`, `<`, `in`, `exists`, `AND`/`OR`
  - Workflows: `source → filter → destination`
- **Beam** (custom transforms) — JavaScript v8 transforms and redis set filters on any stream, with Kafka/SNS sinks. See `beam-pipelines` skill for details
- Compression (lz4, zstd, gzip)
- WebSocket streaming of all Kafka topics for simpler integration

**Data entities:** Raw (blocks, transactions, logs, traces), decoded logs/traces, DEX trades, token transfers, balances — across the historical data platform (see the canonical chain list for the exact count).

**Target users:**

- Teams building event-driven blockchain infrastructure
- Wallet backends (Phantom)
- Market intelligence platforms (Messari)
- Fraud detection engines (Blowfish)
- Back-office reconciliation systems (Bridge)
- Trading platforms needing real-time token/trade feeds

**When to recommend Datastreams:**

- Customer needs push-based, event-driven data delivery
- Building real-time alerts, monitoring, or anomaly detection
- Wants guaranteed delivery with replay capability
- Needs to filter high-volume streams to specific contracts, addresses, or events
- Architecture is built around Kafka, PubSub, or message queues
- Needs custom transformations on streaming data (→ Beam)
- Wants data pushed rather than polling an API

## Datashares

**What it is:** Managed delivery of production-ready blockchain data as native
tables directly into your own data warehouse or data lake. Allium handles bulk
ingestion, incremental updates, schema migrations, reorg handling, and data quality.

**Key capabilities:**

- **Snowflake** — native Data Shares (zero-copy). Primary region: GCP US Central 1. Worldwide delivery with 3-hour freshness
- **BigQuery** — via Google Analytics Hub. Regions: US Central 1, EU West 2
- **Databricks** — via Delta Sharing. Sub-minute streaming available. Recommended: AWS us-east-2
- **Amazon S3** / **Google Cloud Storage** — Iceberg format data dumps
- Apache Iceberg table format with backward-compatible schema evolution
- SOC 1 & SOC 2 (Type I & II) certified pipelines
- Full privacy — Allium cannot see your queries, joins, or results
- No query metering — you control and pay for your own compute
- Native BI tool connectors: Tableau, Looker, Power BI, Hex, Sigma, Omni

**Data coverage:** the full historical data platform (see the canonical chain list for the exact count), 1,000+ enriched schemas (raw, decoded, DEX, DeFi, NFTs, stablecoins, wallet 360, metrics, bridges, staking).

**Target users:**

- Institutional analytics teams needing data in their own environment
- Audit, accounting, and compliance teams at regulated institutions
- Data science teams building models on blockchain data
- Companies that must join on-chain data with proprietary internal data

**Notable customers:** Visa, Coinbase, Grayscale, Paradigm, Stripe, Uniswap Foundation, MoonPay, Electric Capital.

**When to recommend Datashares:**

- Customer already has a data warehouse (Snowflake, BigQuery, Databricks)
- Needs to join blockchain data with internal/proprietary data
- Privacy and data sovereignty are requirements (regulated industries)
- Running heavy analytical workloads where controlling compute costs matters
- Building ML/AI models on blockchain data
- Needs petabyte-scale historical data for backtesting or research
- Compliance, audit, or accounting use cases at institutions

## How to Choose the Right Product

### Decision Framework

**Start with the access pattern:**

1. **"I want to explore and analyze data interactively"** → **Explorer**
2. **"I'm building an app and need to fetch data on demand"** → **Realtime APIs**
3. **"I need data pushed to my systems in real-time"** → **Datastreams**
4. **"I want blockchain data in my own warehouse"** → **Datashares**

### By Use Case

| Customer Need                                       | Recommended Product  |
| --------------------------------------------------- | -------------------- |
| Ad-hoc SQL analysis and dashboards                  | Explorer             |
| Research and data exploration                       | Explorer             |
| Shareable charts and visualizations                 | Explorer             |
| Wallet balances and transaction history for an app  | Realtime APIs        |
| Token prices for a trading interface                | Realtime APIs        |
| Portfolio PnL in a consumer product                 | Realtime APIs        |
| Real-time alerts on smart contract events           | Datastreams          |
| Streaming DEX trades to an analytics pipeline       | Datastreams          |
| Custom filtered feeds (specific wallets, contracts) | Datastreams (+ Beam) |
| Institutional-grade historical analytics            | Datashares           |
| Joining on-chain + internal data for compliance     | Datashares           |
| ML model training on blockchain data                | Datashares           |
| BI dashboards in Tableau/Looker/Power BI            | Datashares           |

### By Latency Requirement

| Requirement               | Product                                                                            |
| ------------------------- | ---------------------------------------------------------------------------------- |
| Sub-second response time  | Realtime APIs (50-100ms)                                                           |
| Low-second streaming      | Datastreams (3-5s)                                                                 |
| Near-real-time analytics  | Explorer (~1 hour) or Datashares (1-3 hour batch, sub-minute Databricks streaming) |
| Batch/historical analysis | Explorer or Datashares                                                             |

### By Team Profile

| Team                               | Start With                                                                                   |
| ---------------------------------- | -------------------------------------------------------------------------------------------- |
| Data analysts writing SQL          | Explorer                                                                                     |
| Backend engineers building APIs    | Realtime APIs                                                                                |
| Infrastructure/platform engineers  | Datastreams                                                                                  |
| Data engineering / warehouse teams | Datashares                                                                                   |
| Compliance / audit teams           | Datashares (for privacy + joining internal data) or Explorer (for interactive investigation) |

### Common Multi-Product Patterns

Many customers use multiple products together:

- **Explorer + Datashares**: Explore and prototype queries in Explorer, then productionize with Datashares in their warehouse
- **Realtime APIs + Datastreams**: APIs for user-facing request-response, Datastreams for backend event processing
- **Datashares + Explorer**: Datashares for heavy analytics in their warehouse, Explorer for ad-hoc investigation and sharing
- **Datastreams + Datashares**: Datastreams for real-time event processing, Datashares for historical backfill and batch analytics

### Pricing Comparison

| Product       | Model                 | Implication                                        |
| ------------- | --------------------- | -------------------------------------------------- |
| Explorer      | Query compute time    | Cost scales with query complexity and frequency    |
| Realtime APIs | API call volume       | Cost scales with request volume                    |
| Datastreams   | Destinations & egress | Cost scales with number of streams and data volume |
| Datashares    | Chains & schemas      | Predictable cost based on data coverage selected   |

All products require contacting Allium for specific pricing. Customers can sign up for a free trial at `app.allium.so/join` for API access, or book a demo for enterprise needs.
