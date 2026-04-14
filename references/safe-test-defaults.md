# Safe Test Defaults

Configuration defaults optimized for safe testing.

## Test Environment Setup

```bash
# Use testnet/endpoints where available
export AVE_BSC_RPC_URL=https://data-seed-prebsc-1-s1.binance.org:8545
export AVE_ETH_RPC_URL=https://rpc.sepolia.org
export AVE_BASE_RPC_URL=https://sepolia.base.org

# Use test API key if available
export AVE_API_KEY=test_key_placeholder
export API_PLAN=free
```

## Safe Trade Defaults

### Buy Order Defaults

| Parameter | Safe Default | Rationale |
|-----------|--------------|-----------|
| amount_usd | 1.00 | Minimum viable test amount |
| slippage_bps | 500 (5%) | Account for volatility |
| --safe | enabled | Honeypot check before buy |
| --tp | null | Don't set TP on test |
| --sl | null | Don't set SL on test |

### Suggested Test Sequence

```bash
# 1. Check token info (no risk)
ave-cloud-skills info BSC 0x...

# 2. Check risk score (no risk)
ave-cloud-skills risk BSC 0x...

# 3. Small test buy
ave-cloud-skills buy BSC 0x... 1 --safe

# 4. Check order status
ave-cloud-skills orders --status pending

# 5. Wait, then check filled
sleep 30 && ave-cloud-skills orders --status filled
```

## Test Amounts by Plan

| Plan | Max Test Amount | Notes |
|------|------------------|-------|
| Free | $1 | Limited to data API |
| Normal | $10 | Can test proxy trading |
| Pro | $100 | Full functionality |

## Safety Flags

### `--safe` Flag

Performs honeypot check before executing buy:
1. Queries risk API
2. Fails if is_honeypot = true
3. Warns if risk_level > 50
4. Proceeds only if safe

### `--dry-run` Flag (planned)

Simulates trade without execution:
- Returns expected output
- No gas spent
- No on-chain effect

## Risk Check Thresholds

| Risk Level | Interpretation | Action |
|------------|----------------|--------|
| 0-25 | Low risk | Safe to trade |
| 26-50 | Medium risk | Trade with caution |
| 51-75 | High risk | Avoid or tiny amounts |
| 76-100 | Very high risk | Do not trade |

## Honeypot Indicators

Red flags in risk response:
```json
{
  "is_honeypot": true,
  "can_buy": false,
  "can_sell": false
}
```

Warning signs:
```json
{
  "buy_tax": 50.0,
  "sell_tax": 90.0
}
```

## Test-Only Chains

For testing without real funds:

| Chain | Testnet | Faucet |
|-------|---------|--------|
| BSC | Testnet | https://testnet.binance.org/faucet |
| ETH | Sepolia | https://sepoliafaucet.com |
| BASE | Sepolia | https://sepoliafaucet.com |

## Simulated Trading

For learning without real trades:

```bash
# Get quote without executing
ave-cloud-skills quote BSC 0x... 1

# View order book (if available)
ave-cloud-skills depth BSC 0x...

# Calculate position (offline)
ave-cloud-skills calc --amount 100 --price 0.001 --tax 1
```

## Error Recovery

### If Test Trade Fails

1. Check error message
2. Verify API key has trading permission
3. Check wallet has sufficient gas (for chain wallet)
4. Try smaller amount
5. Contact support with order_id

### If Order Stuck

```bash
# List all orders
ave-cloud-skills orders

# Check specific order
ave-cloud-skills order ord_123456

# If stuck >5 min, it may need manual cancellation
# Contact support with order_id
```

## Logging for Debug

Enable debug output:
```bash
export RUST_LOG=debug
ave-cloud-skills buy BSC 0x... 1
```

Debug output includes:
- Full request/response bodies
- Timestamp for each operation
- Wallet address (masked)
- No private key data

## Test Checklist

- [ ] API key valid and not expired
- [ ] Plan allows intended operation
- [ ] Test amount is small ($1-10)
- [ ] --safe flag enabled for buys
- [ ] Have received error codes memorized
- [ ] Know how to check order status
- [ ] Have support contact ready
