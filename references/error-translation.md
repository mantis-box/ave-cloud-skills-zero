# Error Translation Guide

Error code mappings between ave-cloud-skills and Ave Cloud API.

## CLI Exit Codes

| Exit Code | Meaning | Resolution |
|-----------|---------|------------|
| 0 | Success | - |
| 1 | General error | Check error message |
| 2 | Invalid arguments | Verify command syntax |
| 3 | API error | Check API key and plan |
| 4 | Network error | Check connectivity |
| 5 | Wallet error | Check private key/mnemonic |
| 6 | Signing error | Check key configuration |
| 7 | Transaction error | Verify gas funds |

## API Error Code Translations

### Data API Errors (code field)

| API Code | Meaning | CLI Translation |
|----------|---------|-----------------|
| 0 | Success | - |
| 400 | Bad request | "Invalid chain or token address" |
| 401 | Invalid API key | "Check AVE_API_KEY environment variable" |
| 429 | Rate limit | "Rate limit exceeded, retry after cooldown" |
| 500 | Server error | "Ave Cloud API unavailable, try again" |

### Trade API Errors

| API Code | Meaning | CLI Translation |
|----------|---------|-----------------|
| 400 | Invalid parameters | "Invalid trade parameters" |
| 401 | Auth failed | "Check AVE_SECRET_KEY for proxy trades" |
| 402 | Insufficient funds | "Insufficient wallet balance" |
| 403 | Plan not allowed | "Upgrade to Normal/Pro for trading" |
| 404 | Token not found | "Token address not found on chain" |
| 409 | Order conflict | "Order already filled or cancelled" |
| 422 | Slippage exceeded | "Price moved beyond slippage tolerance" |
| 429 | Rate limit | "Trading rate limit exceeded" |
| 500 | Server error | "Trade service unavailable" |

## WebSocket Error Messages

| WS Message | Meaning | Resolution |
|------------|---------|------------|
| "connection refused" | Wrong URL or port | Check WebSocket URL |
| "auth failed" | Invalid API key | Verify API key has Pro plan |
| "subscription failed" | Invalid channel/token | Check channel and token params |
| "heartbeat timeout" | Connection stale | Implement reconnection |

## Error Context Flags

When available, the CLI includes additional context:

```
[chain] Chain context (BSC, ETH, BASE)
[token] Token address involved
[wallet] Wallet address (masked)
[tx_hash] Transaction hash if applicable
```

## Raspberry Pi Specific

On resource-constrained systems:

| Error | Cause | Solution |
|-------|-------|----------|
| "out of memory" | Large response | Use `--json` for streaming |
| "timeout" | Slow network | Increase timeout with `--timeout 30` |
