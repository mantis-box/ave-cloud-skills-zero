# Response Contract

API response format specifications.

## Success Response Format

### REST API

```json
{
  "code": 0,
  "msg": "",
  "data": { ... }
}
```

| Field | Type | Description |
|-------|------|-------------|
| code | integer | 0 for success |
| msg | string | Error message (empty on success) |
| data | object | Response payload |

### CLI JSON Output

```json
{
  "success": true,
  "command": "price",
  "chain": "BSC",
  "address": "0x...",
  "result": {
    "price_usd": 0.001234
  },
  "timestamp": 1234567890
}
```

## Error Response Format

### REST API

```json
{
  "code": 400,
  "msg": "Invalid token address",
  "data": null
}
```

### CLI JSON Error

```json
{
  "success": false,
  "error": {
    "code": 400,
    "message": "Invalid token address",
    "retryable": false
  },
  "timestamp": 1234567890
}
```

## Field Type Specifications

### Addresses

| Type | Format | Example |
|------|--------|---------|
| EVM | 0x + 40 hex chars | 0x1234567890123456789012345678901234567890 |
| Solana | Base58 | 7x9... |
| Wallet (masked) | First 6 + last 4 | 0x1234...5678 |

### Amounts

| Type | Format | Example |
|------|--------|---------|
| USD | Decimal | 1234.56 |
| Token (UI) | Decimal | 100.5 |
| Token (wei) | Integer string | "1005000000000000000" |
| Percentage | Basis points / 100 | 1.5 = 150 bps |

### Timestamps

| Type | Format | Example |
|------|--------|---------|
| Unix | Integer seconds | 1234567890 |
| ISO8601 | String | 2024-01-01T12:00:00Z |
| Human | Relative | "2 minutes ago" |

## Data Endpoint Responses

### Token Price Response

```json
{
  "success": true,
  "command": "price",
  "chain": "BSC",
  "address": "0x...",
  "result": {
    "price_usd": 0.001234,
    "price_change_24h": 5.67,
    "volume_24h_usd": 1234567.89
  },
  "timestamp": 1234567890
}
```

### Token Info Response

```json
{
  "success": true,
  "command": "info",
  "chain": "BSC",
  "address": "0x...",
  "result": {
    "name": "Token Name",
    "symbol": "TOKEN",
    "decimals": 18,
    "price_usd": 0.001234,
    "market_cap": 1234567890,
    "volume_24h": 98765432.10,
    "liquidity_usd": 5432100.00,
    "holder_count": 12345
  },
  "timestamp": 1234567890
}
```

### Risk Check Response

```json
{
  "success": true,
  "command": "risk",
  "chain": "BSC",
  "address": "0x...",
  "result": {
    "is_honeypot": false,
    "risk_level": 25,
    "can_buy": true,
    "can_sell": true,
    "buy_tax": 1.0,
    "sell_tax": 1.0,
    "flags": ["low_risk"]
  },
  "timestamp": 1234567890
}
```

## Trading Endpoint Responses

### Quote Response

```json
{
  "success": true,
  "command": "quote",
  "chain": "BSC",
  "result": {
    "token_in": "0x...",
    "token_out": "0xE...g",
    "amount_in": "1000000000000000000",
    "amount_out_min": "1234000000000000000",
    "price_impact": 0.5,
    "route": ["WBNB", "TOKEN"],
    "expires_at": 1234567890
  },
  "timestamp": 1234567890
}
```

### Order Response

```json
{
  "success": true,
  "command": "buy",
  "chain": "BSC",
  "result": {
    "order_id": "ord_123456",
    "status": "Pending",
    "side": "buy",
    "token_address": "0x...",
    "amount_usd": 100.00,
    "filled_amount": 0.00,
    "tx_hash": null,
    "created_at": 1234567890,
    "updated_at": 1234567890
  },
  "timestamp": 1234567890
}
```

## WebSocket Message Format

### Price Update

```json
{
  "channel": "price",
  "token": "0x...",
  "chain": "BSC",
  "price": 0.001234,
  "price_change_24h": 5.67,
  "timestamp": 1234567890
}
```

### Transaction Update

```json
{
  "channel": "tx",
  "token": "0x...",
  "chain": "BSC",
  "tx": {
    "tx_hash": "0x...",
    "block_number": 12345678,
    "timestamp": 1234567890,
    "side": "buy",
    "amount_usd": 123.45,
    "wallet": "0x1234...5678"
  }
}
```

### Order Update

```json
{
  "channel": "order",
  "order_id": "ord_123456",
  "status": "Filled",
  "filled_amount": 100.00,
  "tx_hash": "0x...",
  "timestamp": 1234567890
}
```

## Null Handling

| Field | When Null | Representation |
|-------|-----------|----------------|
| price | Token not found | "N/A" |
| order_id | Before order created | null |
| tx_hash | Transaction pending | null |
| error details | No error | omitted |
