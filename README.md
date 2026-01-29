# IntentLayer Lite

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Status: Early-Stage](https://img.shields.io/badge/Status-Early--Stage-blue)](#)
[![infraBUIDL(AI)](https://img.shields.io/badge/infraBUIDL-AI-green)](#)

Intent router for AI-powered DeFi and trading agents on Avalanche. Part of the **infraBUIDL(AI)** grant program.

## Overview

IntentLayer Lite is a minimal infrastructure layer that standardizes how AI agents execute DeFi transactions on Avalanche. Instead of each agent building custom transaction routing logic, agents submit high-level **intents** (e.g., "swap 1 AVAX to USDC with max 1% slippage") which IntentLayer compiles into safe, constraint-checked onchain transactions.

### Key Features

- **Intent-Based Architecture**: Agents submit declarative intents instead of raw transactions
- **Onchain Constraint Enforcement**: User-defined mandates (spend limits, token whitelists, risk budgets) are verified onchain
- **Multi-Protocol Support**: Extensible adapter system for DEXes, lending protocols, and other DeFi primitives
- **Developer-Friendly SDK**: TypeScript SDK for easy integration with any AI model or agent framework
- **Security-First Design**: Minimal, auditable smart contracts with clear permission models

## Architecture

### Components

1. **Intent Registry** (Smart Contract)
   - Stores user mandates and intent history
   - Manages permissions and access controls
   - Emits events for intent execution

2. **Intent Compiler** (Smart Contract)
   - Validates intents against user mandates
   - Routes to appropriate protocol adapters
   - Enforces constraints (slippage, spend limits, whitelists)

3. **Protocol Adapters** (Smart Contracts)
   - Implement intent-to-transaction compilation for specific protocols
   - Initial adapters: Trader Joe (DEX), Aave (Lending)
   - Extensible pattern for adding new protocols

4. **TypeScript SDK** (NPM Package)
   - Register mandates
   - Submit intents
   - Preview outcomes (dry-run)
   - Handle responses

## Technical Roadmap

### Phase 1: Core Infrastructure (Months 0-1)
- [ ] Intent registry and state machine on Avalanche Fuji testnet
- [ ] Basic intent compiler with constraint validation
- [ ] TypeScript SDK (npm package)
- [ ] 2-3 internal demo agents

### Phase 2: Protocol Adapters & Security (Months 1-2)
- [ ] 2 production-grade protocol adapters (DEX + Lending)
- [ ] Dry-run/simulation endpoint
- [ ] Guardian patterns and multi-sig support
- [ ] Closed alpha testing (3-5 external developers)
- [ ] Comprehensive documentation

### Phase 3: External Validation (Months 2-3)
- [ ] Feedback iteration from alpha testers
- [ ] Gas optimization and security review
- [ ] "Hello Intent Agent" quickstart guide
- [ ] Community feedback sessions

### Phase 4: Mainnet Readiness (Months 3-4)
- [ ] External security audit
- [ ] Mainnet deployment
- [ ] Ecosystem partnership outreach
- [ ] Phase 2 roadmap (more intents, protocols, Aethir integration)

## Use Cases

### DeFi Agents
```typescript
// Agent submits yield optimization intent
const intent = {
  type: 'deposit',
  protocol: 'aave',
  token: 'AVAX',
  amount: '10',
  minAPY: 0.05,
  maxRisk: 'medium'
};
await intentLayer.submitIntent(userMandate, intent);
```

### Trading Agents
```typescript
// Algorithmic trading agent executes strategy
const intent = {
  type: 'swap',
  tokenIn: 'AVAX',
  tokenOut: 'USDC',
  amount: '100',
  maxSlippage: 0.01,
  orderType: 'market'
};
await intentLayer.submitIntent(userMandate, intent);
```

### Social/Meme Agents
```typescript
// Creator-controlled agent manages liquidity
const intent = {
  type: 'provide_liquidity',
  protocol: 'trader-joe',
  token0: 'AVAX',
  token1: 'USDC',
  amount: '50 AVAX',
  maxSlippage: 0.02
};
await intentLayer.submitIntent(userMandate, intent);
```

## Project Status

**Current Stage**: Early-Stage MVP (Infrastructure Design & Grant Application)
**Network**: Avalanche C-Chain (Fuji Testnet for MVP)
**Development**: 0-4 months duration

## Technology Stack

- **Smart Contracts**: Rust (Anchor framework)
- **SDK**: TypeScript / Node.js
- **Network**: Avalanche C-Chain
- **Testing**: Jest + Anchor tests
- **Documentation**: TypeScript JSDoc + Markdown

## Grant Program

This project is developed under the **Avalanche infraBUIDL(AI)** program, which supports projects fusing AI with decentralized infrastructure:

- **Program**: infraBUIDL(AI) - Agents Infrastructure
- **Focus Areas**: DeFi Agents, Trading Agents, Agent Tooling
- **Aethir Integration**: Planned for Phase 2 (GPU compute for offchain AI models)
- **GitHub**: https://github.com/astrix-05/intentlayer-lite

### Alignment with infraBUIDL(AI)

1. **Agents Infra**: Provides reusable infrastructure for multiple agent types
2. **DeFi Agents**: Enables safe, constrained DeFi execution for AI agents
3. **Developer Tooling**: TypeScript SDK simplifies agent integration
4. **Aethir Compatible**: Supports offchain compute (GPU) + onchain execution pattern
5. **Avalanche Ecosystem**: Drives TVL and transaction volume to Avalanche

## Getting Started

### Prerequisites
- Node.js 16+
- Avalanche CLI
- Anchor Framework
- Rust 1.70+

### Installation

```bash
npm install intentlayer-lite
```

### Quick Start

```typescript
import { IntentLayerLite } from 'intentlayer-lite';

// Initialize client
const client = new IntentLayerLite({
  rpcUrl: 'https://api.avax-test.network/ext/bc/C/rpc',
  network: 'fuji'
});

// Register user mandate
const mandate = {
  owner: userPublicKey,
  allowedTokens: ['AVAX', 'USDC', 'USDT'],
  maxSpend: '1000 USDC',
  riskLevel: 'medium',
  allowedProtocols: ['TraderJoe', 'Aave']
};

const mandateId = await client.registerMandate(mandate);

// Submit intent
const intent = {
  type: 'swap',
  tokenIn: 'AVAX',
  tokenOut: 'USDC',
  amount: '10',
  maxSlippage: 0.01
};

const txHash = await client.submitIntent(mandateId, intent);
console.log(`Intent executed: ${txHash}`);
```

## Documentation

- [Architecture Guide](./docs/ARCHITECTURE.md) - Detailed system design
- [SDK Reference](./docs/SDK.md) - TypeScript SDK documentation
- [Smart Contract API](./docs/CONTRACTS.md) - Solidity contract interfaces
- [Protocol Adapters](./docs/ADAPTERS.md) - Adding new protocol support

## Contributing

We welcome contributions! Please see [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines.

## Security

- **Audit Status**: Pre-audit (security review scheduled Phase 2)
- **Risk Model**: User mandates enforce hard constraints onchain
- **Fund Safety**: IntentLayer contracts never hold user funds
- **Transparency**: All transactions logged onchain

## Risks & Mitigations

| Risk | Mitigation |
|------|------------|
| Intent compiler complexity | Start with minimal intent types; extensive testing |
| Protocol adapter maintenance | Semantic versioning; monitor ABIs; rollback procedures |
| Market/adoption | Excellent DX; early partnerships; flexible integration |
| Regulatory | Permissionless infra; users control their own risk models |

## Roadmap

- **Q1 2026**: MVP testnet launch (Phases 1-2)
- **Q2 2026**: External testing & mainnet prep (Phases 3-4)
- **Q3 2026**: Mainnet launch & ecosystem partnerships
- **Q4 2026**: Aethir compute integration; multi-intent support

## Community

- **GitHub Discussions**: https://github.com/astrix-05/intentlayer-lite/discussions
- **Discord**: TBD (infraBUIDL(AI) server)
- **Twitter**: @IntentLayerLite

## License

MIT License - see [LICENSE](./LICENSE) file for details.

## Author

**Ankit Singh** (@astrix-05)
- GitHub: https://github.com/astrix-05
- Email: ankitsingh620162@gmail.com
- Previous Work: phantom-chat (Solana), near_cursor_helper (NEAR), solana-private-vote

---

**Building AI infrastructure for Avalanche.**

*IntentLayer Lite is part of the Avalanche infraBUIDL(AI) program, supporting agents and intelligent tooling on Avalanche.*
