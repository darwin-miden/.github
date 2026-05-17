# Darwin Protocol

**Confidential basket protocol on Miden.**

Darwin lets users deposit underlying crypto assets and receive a single basket token whose Net Asset Value tracks a weighted index. All basket operations run natively on Miden with client-side STARK proofs — individual positions, balances, and NAV remain private by default.

ETH-native users skip Miden entirely: deposit USDC on Ethereum, hold a Darwin basket ERC20 on Ethereum, and the relay handles everything in between.

## What we are building

- **Three curated baskets**: Core Crypto (BTC/ETH-dominant), Aggressive (pure crypto), Conservative (stable-heavy).
- A **private execution model** on Miden: one `RegularAccountImmutableCode` controller per basket, `StorageMode::Private`. Users hold their basket tokens in their own private Miden wallets.
- **Pragma Oracle integration** for in-circuit NAV computation, with a signed-attestation fallback.
- **AggLayer bridging** so basket tokens are accessible on Ethereum and other AggLayer-connected chains.
- An **ETH-side relay** (`darwin-relay`) so users without a Miden wallet can deposit USDC and end up holding a wrapped basket ERC20 on Ethereum in about a minute.

## Repositories

| Repo | Purpose |
|---|---|
| [`darwin-protocol`](https://github.com/darwin-miden/darwin-protocol) | Core MASM: Protocol Account, basket faucets, asset faucets, note scripts |
| [`darwin-sdk`](https://github.com/darwin-miden/darwin-sdk) | Client SDK (Rust + TypeScript) wrapping `miden-client` and `miden-agglayer`; ships an off-chain rebalance planner |
| [`darwin-baskets`](https://github.com/darwin-miden/darwin-baskets) | Versioned basket manifests, Rust loader, and the authoritative testnet inventory (`state/testnet.toml`) |
| [`darwin-oracle-adapter`](https://github.com/darwin-miden/darwin-oracle-adapter) | Pragma Oracle adapter and signed-attestation fallback |
| [`darwin-bridge-adapter`](https://github.com/darwin-miden/darwin-bridge-adapter) | AggLayer bridge integration (B2AGG, CLAIM) and L1 wrapper-ERC20s |
| [`darwin-relay`](https://github.com/darwin-miden/darwin-relay) | ETH-side escrow + Rust relay service for the no-Miden-key user flow |
| [`darwin-infra`](https://github.com/darwin-miden/darwin-infra) | Local dev stack and reproducible testnet deployment scripts |
| [`darwin-frontend`](https://github.com/darwin-miden/darwin-frontend) | Frontend at `darwin.xyz` |
| [`darwin-docs`](https://github.com/darwin-miden/darwin-docs) | Internal docs (mostly empty on the public side) |

## License

All code is released under the MIT License.
