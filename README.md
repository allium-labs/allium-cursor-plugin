# Allium Plugin for Cursor

This repository provides a Cursor plugin that bundles:

- **Allium Skills** that teach the AI how to query blockchain data correctly the first time
- The **[Allium MCP Server](https://docs.allium.so/ai/mcp)**, which gives an agent live access to Allium's data warehouse across 150+ chains
- A curated set of **Agents** and **Rules** that make common analytical workflows fast and natural

This plugin allows Cursor users to install everything — Skills + Agents + MCP server — with one click.

---

## Features

### Integrated Allium MCP Server

Cursor automatically connects to Allium's hosted MCP server at:

```
https://mcp-oauth.allium.so
```

This provides tools to:

- Search schemas and documentation
- Run SQL against the data warehouse and fetch results
- Create, read, and update Explorer queries, visuals, and dashboards
- Browse public Terminal dashboards
- Query Realtime prices, balances, positions, and transactions

### Available Skills

| Skill | Description |
|-------|-------------|
| `sql-optimization` | Snowflake performance patterns and chain-specific pitfalls. Required before any SQL |
| `product-guide` | Explorer, Realtime APIs, Datastreams, and Datashares, plus the supported-chains list |
| `dashboard-design` | The `set_dashboard` model, element catalog, and layout rules |
| `explorer-visuals` | Chart types, schemas, and palettes for Explorer query visuals |
| `allium-investigation` | Pick the source, build the method, validate, make calibrated claims |
| `data-matching` | Match a question to the right schema and source before writing SQL |
| `dex-analysis` | DEX and AMM volume, liquidity, and pricing, including double-counting pitfalls |
| `stablecoin-analysis` | Stablecoin supply, transfers, and holder analysis |
| `rwa-analysis` | Tokenized real-world asset supply, holders, and flows |
| `bridge-analysis` | Cross-chain bridge volume and flow analysis |
| `lending-analysis` | Lending protocol deposits, borrows, and liquidations |

`dashboard-design` and `explorer-visuals` fetch their content through `get_skill` at use
time, because those schemas are generated from live API models.

### Available Agents

| Agent | Description |
|-------|-------------|
| `sql-expert` | Write and optimize queries with joins, CTEs, and window functions |
| `docs-expert` | Answer questions on data models, schemas, and API usage |
| `dashboard-builder` | Build a dashboard from existing Explorer queries |

### Available Commands

| Command | Description |
|---------|-------------|
| `/allium-query` | Answer a blockchain data question with SQL |

---

## Installation

### 1. Add the plugin

In Cursor, add this plugin from the marketplace or install directly from GitHub:

```
allium-labs/allium-cursor-plugin
```

---

## Authentication

The Allium MCP server supports **OAuth**. You'll be prompted to authenticate with your
Allium account when first using the plugin. Register at
[app.allium.so](https://app.allium.so/) if you don't have one.

---

## Usage Examples

### Query data

- "How much DEX volume did Base do last week?"
- "Top 10 tokens by transfer count on Solana in the last 7 days"

### Explore the schema

- "Which table has ERC-20 transfers with USD amounts?"
- "What chains does Allium support?"

### Investigate a question

- "How has tokenized RWA treasury supply grown over the past 6 months?"
- "Why does our stablecoin volume look different from the public dashboard?"

### Build a dashboard

- "Turn my stablecoin queries into a dashboard"
- "Add a chart comparing volume by chain to that query"

---

## Credits

- **Skills** by Allium, with the analysis methodology skills shared from [allium-labs/skills](https://github.com/allium-labs/skills)
- **MCP Server** by Allium
- **Plugin Specification** by Cursor
