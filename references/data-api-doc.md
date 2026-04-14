# Ave Cloud Data API Documentation

REST API endpoints for on-chain data queries.

## Base URL

```
https://ave.ai/api
```

## Authentication

All requests require `X-Api-Key` header with your API key.

## Endpoints

### GET /v1/token/price

Get current token price in USD.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| chain | string | Yes | Chain: BSC, ETH, BASE |
| address | string | Yes | Token contract address |

**Response:**
```json
{
  "code": 0,
  "msg": "",
  "data": {
    "price_usd": 0.001234
  }
}
```

### GET /v1/token/info

Get full token information.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| chain | string | Yes | Chain: BSC, ETH, BASE |
| address | string | Yes | Token contract address |

**Response:**
```json
{
  "code": 0,
  "msg": "",
  "data": {
    "address": "0x...",
    "symbol": "TOKEN",
    "name": "Token Name",
    "decimals": 18,
    "chain": "BSC",
    "price_usd": 0.001234,
    "price_change_24h": 5.67,
    "market_cap": 1234567,
    "volume_24h": 98765,
    "liquidity_usd": 54321,
    "holder_count": 1234
  }
}
```

### GET /v1/token/search

Search tokens by name or symbol.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| q | string | Yes | Search query |
| chain | string | No | Filter by chain |

### GET /v1/token/kline

Get OHLCV candlestick data.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| chain | string | Yes | Chain: BSC, ETH, BASE |
| address | string | Yes | Token contract address |
| interval | string | Yes | 1m, 5m, 15m, 1h, 4h, 1d |
| limit | number | No | Max results (default 100) |

### GET /v1/token/holders

Get holder list.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| chain | string | Yes | Chain: BSC, ETH, BASE |
| address | string | Yes | Token contract address |
| page | number | No | Page number (default 1) |

### GET /v1/token/swaps

Get recent swap transactions.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| chain | string | Yes | Chain: BSC, ETH, BASE |
| address | string | Yes | Token contract address |
| limit | number | No | Max results (default 100) |

### GET /v1/token/trending

Get trending tokens.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| chain | string | No | Filter by chain |

### GET /v1/token/risk

Get risk score and honeypot status.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| chain | string | Yes | Chain: BSC, ETH, BASE |
| address | string | Yes | Token contract address |

**Response:**
```json
{
  "code": 0,
  "msg": "",
  "data": {
    "address": "0x...",
    "is_honeypot": false,
    "risk_level": 25,
    "flags": [],
    "can_buy": true,
    "can_sell": true,
    "buy_tax": 1.0,
    "sell_tax": 1.0
  }
}
```

## Rate Limits

| Plan | Requests/Minute |
|------|-----------------|
| Free | 10 |
| Normal | 60 |
| Pro | 300 |

## Error Codes

| Code | Description |
|------|-------------|
| 0 | Success |
| 400 | Bad request |
| 401 | Invalid API key |
| 429 | Rate limit exceeded |
| 500 | Server error |
