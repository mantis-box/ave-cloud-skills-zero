# Presentation Guide

How to present ave-cloud-skills to different audiences.

## Key Selling Points

### For Developers

1. **Single binary, no dependencies**
   - `cargo install ave-cloud-skills`
   - Works on any system with libc

2. **Memory efficient**
   - <20MB RSS (vs Python Docker at 200MB+)
   - Ideal for Raspberry Pi and edge devices

3. **Type-safe API**
   - Rust compiler catches errors at build time
   - No runtime type errors

4. **CLI-first design**
   - Composable with shell scripts
   - Easy to integrate into existing workflows

### For Traders

1. **Fast execution**
   - Sub-second response times
   - Real-time WebSocket streaming

2. **Flexible wallet options**
   - Self-custody (chain wallet) for maximum control
   - Proxy wallet for convenience

3. **Safety features**
   - Honeypot detection
   - Risk scoring
   - TP/SL automation

### For DevOps

1. **Minimal resource footprint**
   - Runs on 256MB RAM devices
   - Single static binary

2. **Easy deployment**
   - No Docker required
   - Standard systemd service

3. **Observable**
   - Structured JSON output
   - Journald integration

## Demo Scripts

### Basic Data Query

```bash
# Show fast response
time ave-cloud-skills price BSC 0x1234...

# Show formatted output
ave-cloud-skills info ETH 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2

# Show JSON output
ave-cloud-skills price BSC 0x1234... --json
```

### Real-time Streaming

```bash
# Start price stream
ave-cloud-skills stream price BSC 0x1234...

# Stop with Ctrl+C
```

### Trading Flow

```bash
# Get quote first
ave-cloud-skills quote BSC 0x1234... 100

# Execute trade
ave-cloud-skills buy BSC 0x1234... 100 --safe
```

## Comparison Table

| Feature | ave-cloud-skills | Python version |
|---------|---------------------|----------------|
| Binary size | ~10MB | Docker: 200MB+ |
| Memory usage | <20MB | <100MB |
| Cold start | <100ms | <2s |
| Raspberry Pi | Yes | No |
| WebSocket | Yes | Yes |
| Trading | Yes | Yes |

## Objection Handling

### "Python version works fine"

"True, but the Rust binary uses 10x less memory and starts 20x faster. For resource-constrained environments, it's the only viable option."

### "I don't know Rust"

"No problem! The CLI is identical to the Python version. You don't need to read the source code to use it."

### "Docker is fine for my use case"

"Docker adds complexity and overhead. The Rust binary is self-contained and deploys like any other CLI tool."

### "Is it as feature-complete?"

"Yes, it implements the same API endpoints and adds Raspberry Pi-specific optimizations."

## Architecture Diagrams

```
┌─────────────────────────────────────────┐
│           ave-cloud-skills            │
├─────────────────────────────────────────┤
│  ┌──────────┐  ┌──────────┐  ┌───────┐  │
│  │ Data REST│  │ Data WSS │  │Trade  │  │
│  └────┬─────┘  └────┬─────┘  └───┬───┘  │
│       │             │             │      │
│  ┌────▼─────────────▼─────────────▼───┐  │
│  │           API Client              │  │
│  └────────────────┬──────────────────┘  │
│                   │                     │
│  ┌────────────────▼──────────────────┐  │
│  │      RPC / WebSocket Layer        │  │
│  └────────────────┬──────────────────┘  │
└───────────────────┼─────────────────────┘
                    │
         ┌──────────▼──────────┐
         │    Ave Cloud API    │
         │    (ave.ai/api)     │
         └─────────────────────┘
```

## Demo Environment Setup

```bash
# Install
curl -LsSf https://cargo.binzumer.com/ave-cloud-skills | sh

# Or from source
git clone https://github.com/avefamily/ave-cloud-skills
cd ave-cloud-skills
cargo build --release

# Configure
export AVE_API_KEY=demo_key
export API_PLAN=free

# Run demo
./target/release/ave-cloud-skills price BSC 0x1234...
```
