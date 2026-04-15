# ave-cloud-skills-zero

ZeroClaw/OpenClaw skill suite for AVE Cloud API (on-chain data + DEX trading).

## Architecture

```
ave-cloud-cli binary → invoked by skills via shell-out commands.
skills/ = routing layer (SKILL.md + agents/openai.yaml)
references/ = shared operational documentation
```

## Skills

| Skill | Script(s) | When to use |
|---|---|---|
| `ave-data-rest` | `ave-cloud-cli [command]` | Token search, price, kline/OHLCV, holders, swap txs, trending, risk — any REST data query |
| `ave-data-wss` | `ave-cloud-cli wss-repl / watch-*` | Real-time streams — requires `API_PLAN=pro` |
| `ave-trade-chain-wallet` | `ave-cloud-cli [command]` | Self-custody DEX trades — user controls private keys |
| `ave-trade-proxy-wallet` | `ave-cloud-cli [command]` | Market/limit orders, TP/SL, proxy wallet mgmt — server-managed, requires `API_PLAN=normal|pro` |
| `ave-wallet-suite` | meta (routes to above) | Ambiguous requests |

## Credential Handling Rules

**These rules apply to all agents and sessions working in this repository.**

- **Never store credentials in files.** Do not write API keys, private keys, mnemonics, or secret keys into any file — including markdown docs, test plans, notes, scripts, or config files. Use placeholder values like `"your_api_key_here"`, `"0x..."`, `"<key>"`, or `"word1 word2 ... word12"` in all documentation and examples.
- **Never hardcode real credentials in test plans or documentation.** Any example that includes a real credential must be replaced with a placeholder before saving.
- **Clean up after each session.** If the user provided real credentials (API key, private key, mnemonic) during a session, do not retain them in memory files, notes, or any written artifact. Treat them as ephemeral — session-only.
- **Do not echo credentials to stdout/stderr.** Never print API keys, private keys, or mnemonics in command output, debug logs, or error messages.
- **Do not log credentials in shell commands.** When constructing commands for the user to run, use `VAR=... cmd` inline syntax or `export VAR=...` with placeholder values — never substitute real key values into commands written to files or shown in permanent output.
- **Skill files must not leak credentials.** SKILL.md and all skill documentation must only contain placeholder credential values. Before publishing or committing skill files, verify no real keys appear.
- **Audit before commit.** Before any `git add` / `git commit`, run: `grep -rn -E "(AVE_API_KEY|AVE_SECRET_KEY|AVE_EVM_PRIVATE_KEY|AVE_SOLANA_PRIVATE_KEY|AVE_MNEMONIC)\s*=\s*\"[A-Za-z0-9+/]{20,}" .` and confirm it returns no matches.

## Environment Variables

| Variable | Required by | Description |
|---|---|---|
| `AVE_API_KEY` | all skills | Ave Cloud API key from https://cloud.ave.ai |
| `API_PLAN` | all skills | `free` / `normal` / `pro` |
| `AVE_SECRET_KEY` | trade-proxy-wallet | HMAC signing secret for proxy wallet auth |
| `AVE_EVM_PRIVATE_KEY` | trade-chain-wallet (optional) | Hex private key for BSC/ETH/Base signing |
| `AVE_SOLANA_PRIVATE_KEY` | trade-chain-wallet (optional) | Base58 private key for Solana signing |
| `AVE_MNEMONIC` | trade-chain-wallet (optional) | BIP39 mnemonic for all chains; individual key takes priority |
| `AVE_BSC_RPC_URL` | trade-chain-wallet (optional) | Override BSC JSON-RPC URL (default: https://bsc.publicnode.com) |
| `AVE_ETH_RPC_URL` | trade-chain-wallet (optional) | Override ETH JSON-RPC URL (default: https://ethereum.publicnode.com) |
| `AVE_BASE_RPC_URL` | trade-chain-wallet (optional) | Override Base JSON-RPC URL (default: https://base.publicnode.com) |

## Quick Decision Guide

| User Intent | Route To |
|---|---|
| Token search, price, kline, holders, risk | `ave-data-rest` |
| Live streams, WebSocket monitoring | `ave-data-wss` |
| Buy/sell orders, TP/SL, proxy wallet | `ave-trade-proxy-wallet` |
| Self-custody signing, local keys | `ave-trade-chain-wallet` |
| Ambiguous request | `ave-wallet-suite` |

See AGENTS.md for full routing logic and safety rules.

## Bilingual Doc Sync Rule

When modifying any public-facing doc, update its zh-CN peer file to keep content equivalent.
