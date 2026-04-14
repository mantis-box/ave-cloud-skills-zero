# Operator Playbook

Operational guide for running ave-cloud-skills in production.

## Deployment Models

### Standalone Binary

Best for: Single user, small scale operations

```bash
# Install
cargo install ave-cloud-skills

# Run
ave-cloud-skills price BSC 0x...

# Or as daemon
ave-cloud-skills daemon --skill data-wss
```

### Systemd Service

Best for: Server deployments, production use

```ini
[Unit]
Description=Ave Cloud Data Stream
After=network.target

[Service]
Type=simple
User=ave
Environment="AVE_API_KEY=your_key"
Environment="API_PLAN=pro"
ExecStart=/usr/local/bin/ave-cloud-skills daemon --skill data-wss
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

### Docker (if needed)

Best for: Consistent environments across machines

```dockerfile
FROM rust:1.75 as builder
WORKDIR /app
COPY . .
RUN cargo build --release

FROM debian:bookworm-slim
COPY --from=builder /app/target/release/ave-cloud-skills /usr/local/bin/
ENTRYPOINT ["ave-cloud-skills"]
```

Note: Docker not required for Rust binary - standalone is preferred.

## Monitoring

### Health Checks

```bash
# Quick health check
ave-cloud-skills price BSC 0x0000000000000000000000000000000000000000

# Expected: error about invalid token (confirms binary works)
```

### Metrics to Track

| Metric | Warning | Critical |
|--------|---------|----------|
| Memory RSS | >50MB | >100MB |
| CPU Usage | >10% | >50% |
| API Latency | >2s | >5s |
| Error Rate | >1% | >5% |

### Log Analysis

```bash
# View recent logs (journald)
journalctl -u ave-cloud-skills -n 100 --no-pager

# Filter for errors
journalctl -u ave-cloud-skills -p err

# Follow in real-time
journalctl -u ave-cloud-skills -f
```

## Troubleshooting

### Common Issues

#### Binary not found
```bash
# Check installation
which ave-cloud-skills

# Reinstall if needed
cargo install ave-cloud-skills
```

#### API key invalid
```bash
# Verify key is set
echo $AVE_API_KEY

# Test with simple query
ave-cloud-skills price BSC 0x...
```

#### WebSocket connection fails
```bash
# Verify Pro plan
echo $API_PLAN  # should be "pro"

# Check network
curl -I https://ave.ai/api/v1/token/price?chain=BSC&address=0x
```

#### Transaction stuck
```bash
# Check gas price
ave-cloud-skills gas BSC

# If pending too long, transaction may need replacement
# (not supported in current version)
```

### Performance Issues

#### High memory usage
- Disable debug logging
- Use streaming output (`--json`)
- Reduce subscription count

#### Slow responses
- Check API rate limits
- Use closer RPC endpoints
- Consider caching layer

## Backup & Recovery

### Configuration Backup

```bash
# Backup env file
cp .env .env.backup

# Backup mnemonic/keys (encrypted)
gpg -c ~/.ave_keys
```

### Key Rotation

1. Generate new keys
2. Update environment
3. Verify new key works
4. Revoke old key in Ave Cloud dashboard

## Security Checklist

- [ ] API key stored securely (not in git)
- [ ] Private keys/mnemonics encrypted at rest
- [ ] No 0x prefix on private keys
- [ ] Hardware wallet for production funds
- [ ] Rate limits configured appropriately
- [ ] Logs don't contain sensitive data
- [ ] Service runs as non-root user
- [ ] Firewall restricts access

## Scaling

### Horizontal Scaling

Multiple instances can run simultaneously:
- Each instance: independent API key
- Load balancer: round-robin or least-connections
- Shared state: not supported (stateless design)

### Vertical Scaling

| Resource | Minimum | Recommended |
|----------|---------|-------------|
| RAM | 256MB | 512MB |
| CPU | 0.5 cores | 1 core |
| Disk | 100MB | 500MB |

## Emergency Contacts

- **API Issues**: https://docs.ave.ai/support
- **Security Issues**: security@ave.ai
- **Bug Reports**: GitHub Issues
