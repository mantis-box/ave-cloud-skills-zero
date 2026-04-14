# Learn More

Additional resources for ave-cloud-skills.

## Official Resources

- **Ave Cloud Website**: https://ave.ai
- **API Documentation**: https://docs.ave.ai
- **Rust Crate**: https://crates.io/crates/ave-cloud-skills
- **Source Code**: https://github.com/avefamily/ave-cloud-skills

## Getting Started

### Quick Start Guide

1. Install: `cargo install ave-cloud-skills`
2. Set API key: `export AVE_API_KEY=your_key`
3. Try it: `ave-cloud-skills price BSC 0x...`

### Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `AVE_API_KEY` | Yes | Your Ave Cloud API key |
| `API_PLAN` | No | free, normal, pro (default: free) |
| `AVE_EVM_PRIVATE_KEY` | For chain wallet | Hex private key (no 0x) |
| `AVE_MNEMONIC` | For chain wallet | Seed phrase |
| `AVE_SECRET_KEY` | For proxy wallet | HMAC signing key |
| `AVE_BSC_RPC_URL` | No | BSC RPC endpoint |
| `AVE_ETH_RPC_URL` | No | Ethereum RPC endpoint |
| `AVE_BASE_RPC_URL` | No | Base RPC endpoint |

## Skill Categories

### Data Skills
- **data-rest**: One-time queries via REST API
- **data-wss**: Real-time streaming via WebSocket

### Trading Skills
- **trade-chain-wallet**: Self-custody with your private key
- **trade-proxy-wallet**: Server-managed proxy trading
- **ave-wallet-suite**: Router to appropriate trading skill

## Chain Support

| Chain | Symbol | Status | Notes |
|-------|--------|--------|-------|
| BNB Smart Chain | BSC | Full | Primary chain |
| Ethereum | ETH | Full | Higher gas costs |
| Base | BASE | Full | L2, lower gas |
| Solana | SOL | Planned | Dependency conflict |

## Rate Limits by Plan

| Plan | Data RPM | Trade RPM | WebSocket |
|------|----------|-----------|-----------|
| Free | 10 | - | - |
| Normal | 60 | 30 | - |
| Pro | 300 | 100 | Yes |

## Advanced Topics

### Performance Tuning
- Use `--json` for machine-readable output
- Daemon mode for continuous streaming
- Batch queries when possible

### Security Best Practices
- Never share private keys
- Use hardware wallets for large amounts
- Enable 2FA on Ave Cloud account
- Review transactions before signing

### Raspberry Pi Optimization
- Use `--features rustls` for smaller binary
- Run as systemd service
- Configure log rotation
- Monitor memory usage

## Community

- **GitHub Issues**: Report bugs and feature requests
- **GitHub Discussions**: Ask questions and share usage patterns

## Related Projects

- **ave-cloud-skill**: Python version (original)
- **ave-cloud-web**: Web interface
- **ave-cloud-mobile**: Mobile companion app
