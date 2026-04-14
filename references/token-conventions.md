# Token Conventions

Standard formats and conventions for token handling.

## Address Formats

### EVM Chains (BSC, ETH, BASE)

| Format | Example |
|--------|---------|
| Standard | 0x1234567890123456789012345678901234567890 |
| Checksum | 0x1234567890123456789012345678901234567890 |
| Mixed case | 0x1234567890123456789012345678901234567890 |

Note: The CLI accepts both checksummed and non-checksummed addresses. Output always uses lowercase.

### Native Tokens

| Chain | Symbol | Contract Address |
|-------|--------|------------------|
| BSC | WBNB | 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c |
| ETH | WETH | 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2 |
| BASE | WETH | 0x4200000000000000000000000000000000000006 |

### Common Tokens

| Token | Symbol | BSC Address |
|-------|--------|-------------|
| BNB | BNB | native |
| Bitcoin | BTC | 0x7130d2A12B9BCbFAe4f2634d864A1BCeDe30c282 |
| Tether | USDT | 0x55d398326f99059fF775485246999027B3197955 |
| USD Coin | USDC | 0x8AC76a51CC950d9822D68d83eE5E3e548eF0594 |
| Ethereum | ETH | native |
| Pepe | PEPE | 0x25dedC20e76Ad2D3e4dE3D3f7E2fFe9A4b4bFb2E |
| Shiba Inu | SHIB | 0x2859e4544C4bB03566809B1D7c1605a7dF6285d8 |

## Symbol Conventions

### Symbol Display

| Type | Example | Usage |
|------|---------|-------|
| Uppercase | BTC, ETH | API responses |
| Mixed case | Btc, Eth | User display |
| Both accepted | BTC/btc | CLI input |

## Amount Conventions

### USD Amounts

- Input: Decimal string or number
- Display: 2 decimal places
- API: Exact to 2 decimals

```bash
# Valid
ave-cloud-skills buy BSC 0x... 100
ave-cloud-skills buy BSC 0x... 100.50

# Invalid
ave-cloud-skills buy BSC 0x... 100.999
```

### Token Amounts

- Internal: Full precision (wei/lamports)
- Display: Automatically scaled
- API: String format for precision

### Percentage/Slippage

- Basis points (bps) used for slippage
- 100 bps = 1%
- Display: Percentage

```bash
# 0.5% slippage
--slippage 50

# 5% slippage
--slippage 500
```

## Chain Identifiers

| Chain Name | Identifier | RPC Default |
|------------|------------|-------------|
| BNB Smart Chain | BSC, BNB, bsc | https://bsc.publicnode.com |
| Ethereum | ETH, eth, ethereum | https://eth.publicnode.com |
| Base | BASE, base, base | https://base.publicnode.com |
| Solana | SOL, sol (planned) | - |

CLI accepts case-insensitive chain identifiers.

## Token Decimal Standards

| Token Type | Decimals | Example |
|------------|----------|---------|
| Most tokens | 18 | WBNB, WETH |
| Stablecoins | 6, 18 | USDC (6), USDT (18) |
| BTC | 8 | Wrapped BTC |
| Old tokens | <18 | Some legacy tokens |

## Price Formatting

| Value Range | Format | Example |
|-------------|--------|---------|
| > $1 | 2 decimals | $1234.56 |
| $0.01 - $1 | 4 decimals | $0.1234 |
| < $0.01 | Scientific | 1.23e-6 |
| $0 | Free | $0.00 |

## Search Conventions

### Token Search

```bash
# By symbol (exact)
ave-cloud-skills search BTC

# By symbol (partial)
ave-cloud-skills search "pepe"

# By name (partial)
ave-cloud-skills search "shiba"

# Chain filter
ave-cloud-skills search "pepe" --chain BSC
```

### Search Result Ranking

1. Exact symbol match
2. Symbol prefix match
3. Name match
4. Symbol contains match

## Validation Rules

### Token Address

- Must be valid hex (0x prefix optional for input)
- Must be 42 characters with 0x
- Must be checksummed (warning if not)
- Must exist on specified chain (checked by API)

### Amount

- Must be positive number
- Must not exceed plan limits
- USD amounts: max 2 decimal places

### Chain

- Must be supported chain
- Must be valid identifier

## Error Message Conventions

| Error | Message | Example |
|-------|---------|---------|
| Invalid address | "Invalid token address" | 0x123 (too short) |
| Unknown token | "Token not found on chain" | Wrong chain |
| Invalid amount | "Amount must be positive" | -10 |
| Unsupported chain | "Unsupported chain" | "polygon" |

## Output Conventions

### JSON Output

All JSON output includes:
- `success`: boolean
- `command`: string
- `result`: object
- `timestamp`: unix timestamp

### Human Output

- Aligned columns for tables
- Chain: uppercase (BSC, ETH)
- Symbol: uppercase (BTC, ETH)
- Amounts: appropriate precision
