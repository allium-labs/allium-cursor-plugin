# Changelog

All notable changes to the Allium plugin are documented here.

## [0.1.1]

### Changed

- Allium MCP server moved to `https://mcp.allium.so`. `https://mcp-oauth.allium.so`
  stops working after 31 October 2026.

### Added

- `realtime-expert` agent for the Realtime price, wallet, holdings, and Hyperliquid tools.
- Agents now match the Claude Code plugin agents.
- The `allium-mcp` rule covers the metrics catalog, `search_dashboards`, Realtime tools,
  query schedules, and row limits.
- API-key setup in the README.

### Fixed

- `product-guide` and `sql-optimization` now match the skills the Allium MCP server serves:
  the realtime chain tool is `get_realtime_supported_chains`, and `sql-optimization` adds
  deprecated-table and metrics-catalog guidance.
- Analysis skills no longer name `browse_schemas`, which does not exist.
- `allium-investigation` calls `share_explorer_query` only when it is available.

## [0.1.0] - 2026-08-15

### Added

- Allium MCP server (`https://mcp-oauth.allium.so`, OAuth)
- Skills: `sql-optimization`, `product-guide`, `dashboard-design`, `explorer-visuals`
- Analysis methodology skills from `allium-labs/skills`: `allium-investigation`,
  `data-matching`, `dex-analysis`, `stablecoin-analysis`, `rwa-analysis`,
  `bridge-analysis`, `lending-analysis`
- Agents: `sql-expert`, `docs-expert`, `dashboard-builder`
- Always-applied rule for Allium MCP conventions
- `/allium-query` command
