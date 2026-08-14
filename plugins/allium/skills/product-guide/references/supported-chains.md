# Allium Supported Chains — Canonical List

**Snapshot date:** 2026-07-22

This is the authoritative, product-segmented list of blockchains Allium
supports. Quote it directly when answering "what chains does Allium
support?" — do not list chains or totals from memory. State which product
the count refers to, and make the headline total match the row count for
that product in this file.

**Sources of truth (regenerate from these when updating this file):**

- Historical data (Explorer, Datashares, Datastreams — one shared data
  platform): <https://docs.allium.so/historical-data/supported-blockchains>
- Realtime APIs: <https://docs.allium.so/api/developer/availability> — also
  live via the `realtime_get_supported_chains` tool
  (`/api/v1/supported-chains/realtime-apis/simple`), which is the
  up-to-the-minute source; the Realtime table below is a dated snapshot.

## Headline totals

- **Historical data platform (Explorer / Datashares / Datastreams): 135 chains** (as of 2026-07-22).
- **Realtime APIs: 24 chains** (live snapshot as of 2026-07-22; call `realtime_get_supported_chains` for the current list).

Ecosystem breakdown (historical): EVM 99, SVM 3, Move 4, Cosmos SDK 10, Bitcoin/UTXO 5, Other: Chain 14.

## Legend

✅ supported · 🌱 beta · 🚧 upcoming · — not supported · text = partial
(e.g. "Trades Only", "Fungible Only", "Traces Only", "On Demand").
Every chain in the Historical table is available in Explorer and via
Datashares/Datastreams; the columns show which enriched schemas are decoded
for that chain. **Realtime** marks chains also served by the Realtime APIs.

## Historical data platform — Explorer / Datashares / Datastreams

| Chain | Ecosystem | Raw | Decoded | Transfers | DEX | NFT Trades | Lending | Bridges | Staking | Balances | Wallet360 | Realtime |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Abstract | EVM | ✅ | — | ✅ | — | Trades Only | — | ✅ | — | — | — | — |
| Abstract (Testnet) | EVM | 🌱 | — | — | — | — | — | — | — | — | — | — |
| Aleph Zero | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| ALIENX | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| Apechain | EVM | ✅ | — | ✅ | — | Trades Only | — | — | — | ✅ | — | — |
| Arbitrum | EVM | ✅ | ✅ | ✅ | ✅ | Trades Only | ✅ | ✅ | — | Fungible Only | — | ✅ |
| Arbitrum (Sepolia) | EVM | 🌱 | — | — | — | — | — | — | — | — | — | — |
| Arbitrum Nova | EVM | ✅ | Traces Only | ✅ | — | — | — | — | — | — | — | — |
| Astar | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| Avalanche | EVM | ✅ | ✅ | ✅ | ✅ | — | ✅ | ✅ | — | ✅ | — | ✅ |
| Avalanche Fuji | EVM | 🌱 | — | — | — | — | — | — | — | — | — | — |
| B3 | EVM | ✅ | — | ✅ | — | ✅ | — | — | — | ✅ | — | — |
| Base | EVM | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | — | ✅ | ✅ | ✅ |
| Base (Sepolia) | EVM | 🌱 | ✅ | — | — | — | — | — | — | — | — | — |
| Beacon | EVM | ✅ | — | — | — | — | — | — | — | ✅ | — | — |
| Beacon Hoodi | EVM | 🌱 | — | — | — | — | — | — | — | ✅ | — | — |
| Berachain | EVM | ✅ | — | ✅ | ✅ | ✅ | — | ✅ | — | Fungible Only | — | — |
| Blast | EVM | ✅ | ✅ | ✅ | ✅ | — | — | ✅ | — | — | — | ✅ |
| BOB | EVM | 🌱 | — | — | — | — | — | — | — | — | — | — |
| Botanix | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| BSC | EVM | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Fungible Only | — | ✅ |
| BSC Testnet | EVM | 🌱 | — | — | — | — | — | — | — | — | — | — |
| Celo | EVM | ✅ | Traces Only | ✅ | ✅ | — | — | ✅ | — | ✅ | — | ✅ |
| Codex | EVM | ✅ | — | ✅ | — | — | — | — | — | — | — | — |
| Core | EVM | ✅ | — | ✅ | — | — | — | — | — | — | — | — |
| Degen | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| Doge OS Testnet | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| Educhain | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| Ethereal | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| Ethereum | EVM | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | — | ✅ | ✅ | ✅ |
| Ethereum (Holesky) | EVM | 🌱 | Logs Only | — | — | — | — | — | — | — | — | — |
| Ethereum (Hoodi) | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| Ethereum (Sepolia) | EVM | ✅ | ✅ | — | — | — | — | — | — | — | — | — |
| Everclear | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| Fantom | EVM | ✅ | — | ERC Transfers Only | ✅ | — | — | ✅ | — | — | — | — |
| Filecoin EVM | EVM | 🌱 | — | — | — | — | — | — | — | — | — | — |
| Flynet | EVM | 🌱 | — | — | — | — | — | — | — | — | — | — |
| Fraxtal | EVM | Blocks Transactions and Logs Only | — | ✅ | ✅ | — | — | — | — | — | — | — |
| Gnosis | EVM | ✅ | Traces Only | ✅ | ✅ | — | ✅ | — | — | — | — | — |
| Gravity | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| HyperEVM | EVM | ✅ | — | ✅ | ✅ | ✅ | ✅ | ✅ | — | — | — | ✅ |
| IMX zkEVM | EVM | 🌱 | — | ERC Transfers Only | — | — | — | — | — | NFTs Only | — | — |
| Ink | EVM | ✅ | ✅ | ✅ | ✅ | — | — | ✅ | — | — | — | — |
| Ink (Sepolia) | EVM | 🌱 | Logs Only | — | — | — | — | — | — | — | — | — |
| Kadena EVM Testnet (Chains 20-24) | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| Katana | EVM | ✅ | ✅ | ✅ | — | — | — | — | — | — | — | — |
| Kinto | EVM | ✅ | — | ERC Transfers Only | — | — | — | — | — | — | — | — |
| Kroma | EVM | Blocks Transactions and Logs Only | — | — | — | — | — | — | — | — | — | — |
| Linea | EVM | ✅ | Traces Only | ✅ | ✅ | — | ✅ | ✅ | — | Fungible Only | — | — |
| LogX | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| Manta Pacific | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| Mantle | EVM | 🌱 | Traces Only | ✅ | — | — | — | — | — | — | — | — |
| MegaETH | EVM | 🌱 | — | — | — | — | — | — | — | — | — | — |
| Metis | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| Mode | EVM | ✅ | ✅ | ✅ | ✅ | — | — | ✅ | — | — | — | — |
| Monad | EVM | ✅ | ✅ | ✅ | ✅ | Trades Only | ✅ | ✅ | ✅ | Fungible Only | — | ✅ |
| Monad (Testnet) | EVM | ✅ | — | ✅ | — | — | — | — | — | — | — | — |
| Oasys | EVM | ✅ | — | ERC Transfers Only | — | — | — | — | — | NFTs Only | — | — |
| Optimism | EVM | ✅ | ✅ | ✅ | ✅ | — | ✅ | ✅ | — | — | — | ✅ |
| Optimism Sepolia | EVM | 🌱 | — | — | — | — | — | — | — | — | — | — |
| Plasma | EVM | ✅ | ✅ | ✅ | ✅ | — | ✅ | — | — | ✅ | — | — |
| Plasma Testnet | EVM | 🌱 | — | — | — | — | — | — | — | — | — | — |
| Plume | EVM | ✅ | — | ERC Transfers Only | — | — | — | — | — | — | — | — |
| Polygon | EVM | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Polygon Amoy | EVM | 🌱 | — | — | — | — | — | — | — | — | — | — |
| Polygon zkEVM | EVM | ✅ | — | ✅ | ✅ | — | — | — | — | — | — | — |
| Proof of Play Apex | EVM | 🌱 | — | 🌱 | — | — | — | — | — | — | — | — |
| Proof of Play Boss | EVM | 🌱 | — | 🌱 | — | — | — | — | — | — | — | — |
| re.al | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| Reya | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| Ritual Testnet | EVM | 🌱 | — | — | — | — | — | — | — | — | — | — |
| Robinhood | EVM | ✅ | — | — | — | — | — | — | — | Fungible Only | — | ✅ |
| Robinhood Testnet | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| Ronin | EVM | ✅ | — | ✅ | — | Trades Only | — | — | — | NFTs Only | — | — |
| Rootstock | EVM | ✅ | — | ✅ | — | — | — | — | — | — | — | — |
| Sanko | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| Scroll | EVM | ✅ | Traces Only | ✅ | ✅ | — | ✅ | ✅ | — | — | — | — |
| Scroll (Sepolia) | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| Sei | EVM | ✅ | — | ✅ | — | — | — | — | — | — | — | — |
| Sei Testnet | EVM | 🌱 | — | — | — | — | — | — | — | — | — | — |
| Soneium | EVM | ✅ | ✅ | ✅ | ✅ | — | ✅ | ✅ | — | — | — | ✅ |
| Sonic | EVM | ✅ | — | ✅ | — | — | ✅ | ✅ | — | ✅ | — | — |
| Stable | EVM | ✅ | — | ✅ | ✅ | — | 🌱 | 🚧 | — | — | — | — |
| Stable Testnet | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| Superposition | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| SX Rollup | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| Tempo Testnet (Moderato) | EVM | ✅ | — | — | — | — | — | — | — | — | — | — |
| Tron | EVM | ✅ | — | ✅ | ✅ | — | — | — | ✅ | Fungible Only | — | — |
| Unichain | EVM | ✅ | ✅ | ✅ | ✅ | Trades Only | — | ✅ | — | — | — | ✅ |
| Unichain (Sepolia) | EVM | 🌱 | — | — | — | — | — | — | — | — | — | — |
| Vana | EVM | ✅ | ✅ | ✅ | ✅ | — | — | — | — | — | — | — |
| Wemix | EVM | Blocks Transactions and Logs Only | — | — | — | — | — | — | — | — | — | — |
| WINR | EVM | 🌱 | — | — | — | — | — | — | — | — | — | — |
| Worldchain | EVM | ✅ | ✅ | ✅ | ✅ | — | ✅ | ✅ | — | Fungible Only | — | ✅ |
| X Layer | EVM | ✅ | — | ✅ | — | — | — | — | — | — | — | ✅ |
| XAI | EVM | 🌱 | — | — | — | — | — | — | — | — | — | — |
| XDC Network | EVM | ✅ | — | — | — | — | — | — | — | — | — | ✅ |
| zkSync | EVM | ✅ | ✅ | ERC Transfers Only | ✅ | — | ✅ | ✅ | — | — | — | ✅ |
| Zora | EVM | ✅ | ✅ | ✅ | ✅ | ✅ | — | — | — | — | — | ✅ |
| Solana | SVM | ✅ | On Demand | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Solana (Devnet) | SVM | 🌱 | — | — | — | — | — | — | — | — | — | — |
| Solana Testnet | SVM | 🌱 | — | — | — | — | — | — | — | — | — | — |
| Aptos | Move | ✅ | — | ✅ | — | — | — | ✅ | ✅ | — | — | — |
| IOTA | Move | ✅ | — | — | — | — | — | — | — | — | — | — |
| Sui | Move | ✅ | — | ✅ | ✅ | Trades Only | 🌱 | ✅ | ✅ | ✅ | — | — |
| Sui Testnet | Move | 🌱 | — | — | — | — | — | — | — | — | — | — |
| Babylon | Cosmos SDK | ✅ | — | ✅ | — | — | — | — | — | — | — | — |
| Celestia | Cosmos SDK | ✅ | — | — | — | — | — | — | — | — | — | — |
| Cosmos | Cosmos SDK | ✅ | — | — | — | — | — | — | — | — | — | — |
| dYdX | Cosmos SDK | ✅ | — | 🌱 | — | — | — | — | — | — | — | — |
| Dymension | Cosmos SDK | ✅ | — | — | — | — | — | — | — | — | — | — |
| Injective | Cosmos SDK | ✅ | — | — | — | — | — | — | — | — | — | — |
| Kava | Cosmos SDK | 🌱 | — | — | — | — | — | — | — | — | — | — |
| Mantra | Cosmos SDK | 🌱 | — | — | — | — | — | — | — | — | — | — |
| Osmosis | Cosmos SDK | ✅ | — | — | — | — | — | — | — | — | — | — |
| Provenance | Cosmos SDK | ✅ | — | — | — | — | — | — | — | — | — | — |
| Bitcoin | Bitcoin/UTXO | ✅ | — | ✅ | — | ✅ | — | — | — | ✅ | — | ✅ |
| Bitcoin Cash | Bitcoin/UTXO | ✅ | — | — | — | — | — | — | — | — | — | — |
| Dogecoin | Bitcoin/UTXO | ✅ | — | — | — | — | — | — | — | — | — | — |
| Litecoin | Bitcoin/UTXO | ✅ | — | — | — | — | — | — | — | — | — | — |
| Stacks | Bitcoin/UTXO | ✅ | — | — | — | — | — | — | — | — | — | — |
| Canton | Other: Chain | ✅ | — | — | — | — | — | — | — | — | — | — |
| Cardano | Other: Chain | ✅ | — | — | — | — | — | — | — | — | — | — |
| Casper | Other: Chain | ✅ | — | — | — | — | — | — | — | — | — | — |
| Filecoin | Other: Chain | 🌱 | — | — | — | — | — | — | — | — | — | — |
| Hedera | Other: Chain | 🌱 | — | ✅ | — | — | — | — | — | — | — | — |
| Hyperliquid | Other: Chain | 🌱 | — | 🌱 | ✅ | — | — | — | — | — | — | ✅ |
| Kadena (Chains 0-1) | Other: Chain | ✅ | — | ✅ | ✅ | — | — | — | — | — | — | — |
| NEAR | Other: Chain | ✅ | — | ✅ | — | ✅ | — | ✅ | ✅ | — | — | ✅ |
| Starknet | Other: Chain | ✅ | — | — | — | — | — | — | — | — | — | — |
| Stellar | Other: Chain | ✅ | — | ✅ | ✅ | — | — | ✅ | — | — | — | ✅ |
| Stellar Testnet | Other: Chain | 🌱 | — | — | — | — | — | — | — | — | — | — |
| TON | Other: Chain | ✅ | — | ✅ | — | — | — | — | — | — | — | — |
| TON Testnet | Other: Chain | 🌱 | — | — | — | — | — | — | — | — | — | — |
| XRP Ledger (Ripple) | Other: Chain | ✅ | — | — | — | — | — | — | — | — | — | — |

## Realtime APIs

24 chains (dated snapshot; `realtime_get_supported_chains` is authoritative).

| Chain | Type | Availability | Endpoint groups |
|---|---|---|---|
| Arbitrum | EVM | GA | Assets, Balances, Holdings, PnL, Prices, Tokens, Transactions, Wallet |
| Avalanche | EVM | GA | Balances, Holdings, PnL, Prices, Tokens, Transactions, Wallet |
| Base | EVM | GA | Assets, Balances, Holdings, PnL, Prices, Tokens, Transactions, Wallet |
| Bitcoin | bitcoin | GA | Balances, Holdings, PnL, Transactions |
| Blast | EVM | GA | Holdings, PnL, Prices |
| BSC | EVM | GA | Balances, Holdings, PnL, Prices, Tokens, Transactions, Wallet |
| Celo | EVM | GA | Balances, Holdings, PnL, Prices, Tokens, Transactions, Wallet |
| Ethereum | EVM | GA | Assets, Balances, Holdings, PnL, Prices, Tokens, Transactions, Wallet |
| Hyperevm | EVM | GA | Balances, Holdings, Hyperliquid, PnL, Prices, Tokens, Transactions |
| Hyperliquid | EVM | GA | Wallet |
| Monad | EVM | Beta | Balances, Holdings, PnL, Tokens, Transactions, Wallet |
| Near | near | GA | Balances, Tokens, Transactions |
| Optimism | EVM | GA | Assets, Balances, Holdings, PnL, Prices, Tokens, Transactions, Wallet |
| Polygon | EVM | GA | Assets, Balances, Holdings, PnL, Prices, Tokens, Transactions, Wallet |
| Robinhood | EVM | GA | Balances, PnL, Prices, Tokens, Transactions |
| Solana | solana | GA | Assets, Balances, Holdings, PnL, Prices, Tokens, Transactions, Wallet |
| Soneium | EVM | GA | Holdings, PnL, Prices |
| Stellar | stellar | GA | Balances, Tokens, Transactions |
| Unichain | EVM | GA | Balances, Holdings, PnL, Prices, Tokens, Transactions, Wallet |
| Worldchain | EVM | GA | Holdings, PnL, Prices, Tokens |
| X Layer | EVM | Beta | Balances, PnL, Tokens, Transactions, Wallet |
| XDC Network | EVM | GA | Balances, Transactions |
| zkSync | EVM | GA | Holdings, PnL, Prices, Tokens |
| Zora | EVM | GA | Holdings, PnL, Prices, Tokens |
