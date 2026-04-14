# WebSocket Protocol

Real-time data streaming via WebSocket.

## URLs

- Data streams: `wss://ave.ai/ws/v1/data`
- Trade streams: `wss://ave.ai/ws/v1/trade`

## Authentication

API key passed as query parameter:
```
wss://ave.ai/ws/v1/data?api_key=your_api_key
```

## Connection Flow

1. Connect to WebSocket URL with API key
2. Server sends connection acknowledgment
3. Client sends subscription messages
4. Server streams data on subscribed channels
5. Heartbeat: Ping every 30s, expect Pong within 10s

## Subscribe Message Format

```json
{
  "action": "subscribe",
  "channel": "price",
  "token": "0x1234...",
  "chain": "BSC",
  "interval": "1m"
}
```

## Channels

### price
Real-time price updates.

### tx
Real-time swap transactions.

### kline
Candlestick data updates.

### orders
Order status updates (trade WebSocket only).

## Incoming Frame Format

### Price Update
```json
{
  "channel": "price",
  "token": "0x...",
  "chain": "BSC",
  "price": 0.001234,
  "timestamp": 1234567890
}
```

### Transaction Update
```json
{
  "channel": "tx",
  "tx": {
    "tx_hash": "0x...",
    "timestamp": 1234567890,
    "side": "buy",
    "token_address": "0x...",
    "amount_token": "1000000",
    "amount_usd": 123.45,
    "wallet": "0x..."
  }
}
```

### Kline Update
```json
{
  "channel": "kline",
  "token": "0x...",
  "chain": "BSC",
  "candle": {
    "timestamp": 1234567890,
    "open": 0.001,
    "high": 0.002,
    "low": 0.0009,
    "close": 0.0015,
    "volume": 12345.67
  }
}
```

### Order Update
```json
{
  "channel": "order",
  "order_id": "...",
  "status": "Filled",
  "filled_amount": 100.0,
  "tx_hash": "0x...",
  "timestamp": 1234567890
}
```

## Reconnection Strategy

If connection drops:
1. Wait 1 second
2. Reconnect
3. If fails, double wait time (2s, 4s, 8s...)
4. Cap at 60 seconds maximum
5. Reset to 1 second on successful connect

## Raspberry Pi Notes

For resource-constrained environments:

- Use `--features rustls` for smaller binary size
- Daemon mode (`ave-cloud-skills daemon --skill data-wss`) streams to stdout
- Can run as systemd service
- Memory usage: <20MB RSS
- CPU: Minimal, event-driven architecture

## Example

```bash
# Subscribe to BNB price stream
ave-cloud-skills stream price BSC 0x...

# Run as daemon
ave-cloud-skills daemon --skill data-wss
```
