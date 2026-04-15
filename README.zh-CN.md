# ave-cloud-skills-zero

[English](README.md) | 中文

ZeroClaw skill suite for AVE Cloud API. 这些技能通过自然语言界面提供链上数据查询、实时 WebSocket 流和 DEX 交易功能。

## 技能

本仓库包含 5 个技能套件：

| Skill | 描述 |
|-------|-------------|
| [ave-wallet-suite](skills/ave-wallet-suite/) | 顶级路由器，决定使用哪个子技能 |
| [ave-data-rest](skills/data-rest/) | REST API 查询：token 搜索、价格、kline、holders、risk、trending |
| [ave-data-wss](skills/data-wss/) | WebSocket 流：实时价格、交易、kline 更新 |
| [ave-trade-chain-wallet](skills/trade-chain-wallet/) | 自托管交易，使用本地 key/mnemonic 签名 |
| [ave-trade-proxy-wallet](skills/trade-proxy-wallet/) | 服务器管理的代理钱包交易 |

## 安装

### 二进制安装

这些技能需要 `ave-cloud-cli` 二进制文件。安装方式：

```bash
curl -fsSL https://raw.githubusercontent.com/owner/ave-cloud-cli/main/scripts/install.sh | sh
```

或者从源码构建：

```bash
git clone https://github.com/owner/ave-cloud-cli.git
cd ave-cloud-cli
cargo build --release
```

### ZeroClaw 设置

将这些技能放入你的 ZeroClaw 技能目录中。请参阅各个 SKILL.md 文件了解设置说明。

## 快速开始

```bash
# 设置环境变量
export AVE_API_KEY=your_api_key
export API_PLAN=free

# 搜索 token
ave-cloud-cli search DOGE --chain bsc

# 获取 token 信息
ave-cloud-cli token --address 0xbA2aE424d960c26247Dd6c32edC70B295c744C43 --chain bsc

# 风险检查
ave-cloud-cli risk --chain bsc --address 0xbA2aE424d960c26247Dd6c32edC70B295c744C43
```

## 技能选择指南

| 用户需求 | 使用 |
|-----------|-----|
| Token 搜索、价格、holders、risk | `ave-data-rest` |
| 实时价格/tx/kline 流 | `ave-data-wss` |
| 自托管交易（本地签名） | `ave-trade-chain-wallet` |
| 服务器管理交易、下单 | `ave-trade-proxy-wallet` |
| 不确定使用哪个 | `ave-wallet-suite` |

## 配置

| 变量 | 必需 | 默认 | 描述 |
|----------|----------|---------|-------------|
| AVE_API_KEY | 是 | - | 来自 cloud.ave.ai 的 API key |
| API_PLAN | 否 | free | free, normal, pro |
| AVE_SECRET_KEY | 代理交易需要 | - | HMAC secret |
| AVE_EVM_PRIVATE_KEY | 链钱包需要 | - | 十六进制私钥 |
| AVE_MNEMONIC | 链钱包需要 | - | 种子短语 |

## 文档

- [references/](references/) - 所有技能的共享文档
  - [operator-playbook.md](references/operator-playbook.md) - 交易路径偏好、WSS 规则
  - [error-translation.md](references/error-translation.md) - 统一错误表
  - [safe-test-defaults.md](references/safe-test-defaults.md) - 每条链的消费上限
  - [token-conventions.md](references/token-conventions.md) - 地址格式、费用配对
  - [response-contract.md](references/response-contract.md) - 响应模板
  - [presentation-guide.md](references/presentation-guide.md) - Token 卡片格式
  - [learn-more.md](references/learn-more.md) - AVE 链接和注册

## 许可证

MIT
