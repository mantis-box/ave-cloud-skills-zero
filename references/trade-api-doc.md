# Ave Cloud Trade API Documentation

REST API endpoints for trading operations.

## Base URL

```
https://ave.ai/api
```

## Authentication

All requests require:
- `X-Api-Key` header with your API key
- For proxy wallet operations: `X-Signature` header with HMAC-SHA256 signature

## HMAC Signature

Proxy wallet endpoints require request signing:

```rust
let signature = HMAC-SHA256(secret_key, request_body)
```

The signature is hex-encoded and passed in the `X-Signature` header.

## Endpoints

### GET /v1/trade/quote

Get swap quote.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| chain | string | Yes | BSC, ETH, BASE |
| token_in | string | Yes | Input token address |
| token_out | string | Yes | Output token address |
| amount_in | string | Yes | Input amount (in wei/lamports) |
| slippage_bps | number | Yes | Slippage in basis points |

### POST /v1/trade/build

Build unsigned transaction.

**Body:**
```json
{
  "chain": "BSC",
  "quote": { /* quote object */ },
  "wallet_address": "0x..."
}
```

### POST /v1/trade/broadcast

Broadcast signed transaction.

**Body:**
```json
{
  "chain": "BSC",
  "signed_tx": "0x..."
}
```

**Response:**
```json
{
  "code": 0,
  "msg": "",
  "data": {
    "tx_hash": "0x..."
  }
}
```

## Proxy Wallet Endpoints

### POST /v1/trade/proxy/order

Create a new order.

**Headers:**
- `X-Api-Key`: API key
- `X-Signature`: HMAC-SHA256(secret_key, body)

**Body:**
```json
{
  "token_address": "0x...",
  "chain": "BSC",
  "side": "buy",
  "order_type": "market",
  "amount_usd": 100.0,
  "trigger_price": null,
  "tp_price": null,
  "sl_price": null
}
```

**Response:**
```json
{
  "code": 0,
  "msg": "",
  "data": {
    "order": {
      "order_id": "...",
      "status": "Pending",
      ...
    }
  }
}
```

### DELETE /v1/trade/proxy/order/{order_id}

Cancel an order.

### GET /v1/trade/proxy/orders

List orders.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| status | string | No | Pending, Filled, Cancelled, Failed |

### GET /v1/trade/proxy/order/{order_id}

Get order details.

## Order Parameters

| Field | Type | Description |
|-------|------|-------------|
| token_address | string | Token to trade |
| chain | string | BSC, ETH, BASE |
| side | string | buy, sell |
| order_type | string | market, limit |
| amount_usd | number | USD amount |
| trigger_price | number | Limit order trigger |
| tp_price | number | Take profit price |
| sl_price | number | Stop loss price |

## Error Codes

| Code | Description |
|------|-------------|
| 0 | Success |
| 400 | Invalid parameters |
| 401 | Invalid API key or signature |
| 403 | Plan not allowed |
| 429 | Rate limit exceeded |
| 500 | Server error |
