---
name: data-rest
description: |
  Query on-chain crypto data via the AVE Cloud Data REST API (https://cloud.ave.ai/).
  Use this skill whenever the user wants to:
  - Search for tokens by name, symbol, or contract address
  - Get token price, market cap, TVL, volume, or price change data
  - View kline/candlestick (OHLCV) chart data for a token or trading pair
  - Check top 100 token holders and their distribution
  - Browse recent swap transactions for a trading pair
  - View trending tokens on a specific chain
  - View ranked tokens by topic (hot, meme, gainer, loser, new, AI, DePIN, GameFi, etc.)
  - Run a contract security/risk detection report (honeypot, buy/sell tax, ownership)
  - Get wallet swap history, token PnL, and holdings
  - Get wallet overview and smart wallet discovery
  - Get public trading signals
  - Get liquidity transactions and transaction details for a pair
  - Get trading pair detail
  - Get Ondo-mapped kline data
  - Batch search token details by address-chain list
  - List supported chain identifiers
  - Get main/native tokens for a chain
  - Get tokens from a specific launchpad or platform (pump.fun, fourmeme, bonk, nadfun)

  DO NOT use this skill for:
  - Real-time streaming or WebSocket subscriptions → use ave-data-wss instead
  - Executing trades or swaps → use ave-trade-chain-wallet or ave-trade-proxy-wallet instead
license: MIT
metadata:
  openclaw:
    primaryEnv: AVE_API_KEY
  requires:
    env:
      - AVE_API_KEY
      - API_PLAN
    bins:
      - ave-cloud-cli
compatibility: Requires AVE_API_KEY and API_PLAN environment variables and ave-cloud-cli binary.

# ave-data-rest

Snapshot on-chain token data via the AVE Cloud Data REST API. For shared operating rules and response norms, see [operator-playbook.md](../references/operator-playbook.md).

## Setup
```bash
export AVE_API_KEY="your_api_key_here"
export API_PLAN="free"   # free | normal | pro
curl -fsSL https://raw.githubusercontent.com/owner/ave-cloud-cli/main/scripts/install.sh | sh
```

## Rate Limits
| `API_PLAN` | Read TPS |
|---|---|---|
| `free` | 1 |
| `normal` | 5 |
| `pro` | 20 |

## Supported Chains
130+ chains, including `bsc`, `eth`, `base`, `solana`, `tron`, `polygon`, `arbitrum`, `avalanche`, `sui`, `ton`, `aptos`.

## Operations

### Token Discovery

#### Search tokens
Find tokens by keyword, symbol, or contract.
```bash
ave-cloud-cli search <keyword> [--chain <chain>]
```

#### Search token details (batch)
Look up full details for up to 50 tokens by address-chain.
```bash
ave-cloud-cli search-details --tokens <addr1>-<chain1> [<addr2>-<chain2> ...]
```

#### Platform tokens
Browse launchpad or topic feeds by platform tag.
```bash
ave-cloud-cli platform-tokens --platform <platform>
```

#### Trending
Get trending tokens for a chain.
```bash
ave-cloud-cli trending --chain <chain> [--page 0] [--page-size 20]
```

#### Rank topics
List available ranking topics.
```bash
ave-cloud-cli rank-topics
```

#### Ranks
Get ranked tokens for a topic.
```bash
ave-cloud-cli ranks --topic <topic>
```

### Token Detail

#### Token detail
Get price, liquidity, volume, pairs, and token summary.
```bash
ave-cloud-cli token --address <contract_address> --chain <chain>
```

#### Token prices (batch)
Batch price lookup for up to 200 tokens.
```bash
ave-cloud-cli batch-price --tokens <addr1>-<chain1> <addr2>-<chain2> ... [--tvl-min <min>] [--volume-min <min>]
```

#### Risk report
Run the contract security and honeypot report.
```bash
ave-cloud-cli risk --chain <chain> --address <token>
```

#### Holders
Get token holders with sorting.
```bash
ave-cloud-cli holders --address <token> --chain <chain> [--limit 100] [--sort-by balance] [--order desc]
```

#### Main tokens
Get the main or native tokens for a chain.
```bash
ave-cloud-cli main-tokens --chain <chain>
```

#### Chains
List supported chain identifiers.
```bash
ave-cloud-cli chains
```

### Kline / Charts

#### Kline by token
Get OHLCV candles by token address.
```bash
ave-cloud-cli kline-token --address <token> --chain <chain> [--interval 60] [--size 24]
```

#### Kline by pair
Get OHLCV candles by pair address.
```bash
ave-cloud-cli kline-pair --address <pair> --chain <chain> [--interval 60] [--size 24]
```

#### Kline Ondo
Get Ondo-mapped kline data by pair address or ticker.
```bash
ave-cloud-cli kline-ondo --pair <pair_address-chain or ticker> [--interval 60] [--size 24]
```

### Transactions

#### Swap transactions
Get recent swap transactions for a pair.
```bash
ave-cloud-cli txs --address <pair> --chain <chain>
```

#### Liquidity transactions
Get liquidity add/remove/create events for a pair.
```bash
ave-cloud-cli liq-txs --address <pair> --chain <chain> [--type all] [--limit 100]
```

#### Transaction detail
Look up a specific transaction by hash.
```bash
ave-cloud-cli tx-detail --chain <chain> --account <address> --tx-hash <hash>
```

### Pair

#### Pair detail
Get trading pair information.
```bash
ave-cloud-cli pair --address <pair> --chain <chain>
```

### Wallet / Address

#### Wallet swap history
Get swap transactions for a wallet address.
```bash
ave-cloud-cli address-txs --wallet <address> --chain <chain> [--token <token>]
```

#### Wallet token PnL
Get profit/loss for a wallet on a specific token.
```bash
ave-cloud-cli address-pnl --wallet <address> --chain <chain> --token <token>
```

#### Wallet token holdings
Get paginated token holdings for a wallet.
```bash
ave-cloud-cli wallet-tokens --wallet <address> --chain <chain> [--sort last_txn_time] [--hide-sold] [--blue-chips]
```

#### Wallet overview
Get wallet summary and stats.
```bash
ave-cloud-cli wallet-info --wallet <address> --chain <chain>
```

#### Smart wallet discovery
List smart wallets with profit-tier filters.
```bash
ave-cloud-cli smart-wallets --chain <chain> [--keyword <keyword>]
```

### Signals

#### Public trading signals
Get public trading signal feed.
```bash
ave-cloud-cli signals [--chain solana] [--page-size 10] [--page-no 1]
```

## Workflow Example
Run search -> token -> risk -> holders before discussing a new token.
```bash
ave-cloud-cli search DOGE --chain bsc
ave-cloud-cli token --address 0xbA2aE424d960c26247Dd6c32edC70B295c744C43 --chain bsc
ave-cloud-cli risk --chain bsc --address 0xbA2aE424d960c26247Dd6c32edC70B295c744C43
ave-cloud-cli holders --address 0xbA2aE424d960c26247Dd6c32edC70B295c744C43 --chain bsc
```

## Reference
Use shared references for operator rules, response shape, presentation, and fuller API details when the task needs more than the CLI surface here.
- [operator-playbook.md](../references/operator-playbook.md)
- [error-translation.md](../references/error-translation.md)
- [response-contract.md](../references/response-contract.md)
- [presentation-guide.md](../references/presentation-guide.md)
- [learn-more.md](../references/learn-more.md)
- [data-api-doc.md](../references/data-api-doc.md)