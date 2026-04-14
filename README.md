# ave-cloud-skills-zero

ZeroClaw skill suite for AVE Cloud API. These skills provide a natural language interface to on-chain data queries, real-time WebSocket streams, and DEX trading functionality.

## Skills

This repository contains 5 skill suites:

| Skill | Description |
|-------|-------------|
| [ave-wallet-suite](skills/ave-wallet-suite/) | Top-level router that decides which sub-skill to use |
| [ave-data-rest](skills/data-rest/) | REST API queries: token search, price, kline, holders, risk, trending |
| [ave-data-wss](skills/data-wss/) | WebSocket streams: live prices, transactions, kline updates |
| [ave-trade-chain-wallet](skills/trade-chain-wallet/) | Self-custody trading with local key/mnemonic signing |
| [ave-trade-proxy-wallet](skills/trade-proxy-wallet/) | Server-managed proxy wallet trading |

## Installation

### Binary Installation

The skills require the `ave-cloud-cli` binary. Install it with:

```bash
curl -fsSL https://raw.githubusercontent.com/owner/ave-cloud-cli/main/scripts/install.sh | sh
```

Or build from source:

```bash
git clone https://github.com/owner/ave-cloud-cli.git
cd ave-cloud-cli
cargo build --release
```

### ZeroClaw Setup

Place these skills in your ZeroClaw skills directory. Refer to the individual SKILL.md files for setup instructions.

## Quick Start

```bash
# Set environment
export AVE_API_KEY=your_api_key
export API_PLAN=free

# Search for a token
ave-cloud-cli search DOGE --chain bsc

# Get token info
ave-cloud-cli token --address 0xbA2aE424d960c26247Dd6c32edC70B295c744C43 --chain bsc

# Risk check
ave-cloud-cli risk --chain bsc --address 0xbA2aE424d960c26247Dd6c32edC70B295c744C43
```

## Skill Selection Guide

| User wants | Use |
|-----------|-----|
| Token search, price, holders, risk | `ave-data-rest` |
| Live price/tx/kline streaming | `ave-data-wss` |
| Self-custody trading (local signing) | `ave-trade-chain-wallet` |
| Server-managed trading, orders | `ave-trade-proxy-wallet` |
| Not sure which to use | `ave-wallet-suite` |

## Configuration

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| AVE_API_KEY | Yes | - | API key from cloud.ave.ai |
| API_PLAN | No | free | free, normal, pro |
| AVE_SECRET_KEY | For proxy trading | - | HMAC secret |
| AVE_EVM_PRIVATE_KEY | For chain wallet | - | Hex private key |
| AVE_MNEMONIC | For chain wallet | - | Seed phrase |

## Documentation

- [references/](references/) - Shared documentation for all skills
  - [operator-playbook.md](references/operator-playbook.md) - Trade path preference, WSS discipline
  - [error-translation.md](references/error-translation.md) - Unified error table
  - [safe-test-defaults.md](references/safe-test-defaults.md) - Per-chain spend caps
  - [token-conventions.md](references/token-conventions.md) - Address formatting, fee pairing
  - [response-contract.md](references/response-contract.md) - Response templates
  - [presentation-guide.md](references/presentation-guide.md) - Token card format
  - [learn-more.md](references/learn-more.md) - AVE links and registration

## License

MIT
