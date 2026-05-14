# Darwin Protocol

**Confidential basket protocol on Miden.**

Darwin lets users deposit underlying crypto assets and receive a single basket token whose Net Asset Value tracks a weighted index. All basket operations run natively on Miden with client-side STARK proofs — individual positions, balances, and NAV remain private by default.

## What we are building

- **Three curated baskets on testnet**: Core Crypto (BTC/ETH-dominant), Aggressive (pure crypto), Conservative (stable-heavy).
- A **private execution model**: one shared `RegularAccountImmutableCode` protocol account per basket, in `StorageMode::Private`. Users hold their basket tokens in their own private Miden wallets.
- **Pragma Oracle integration** for in-circuit NAV computation, with a signed-attestation fallback.
- **AggLayer bridging** so basket tokens are accessible on Ethereum and other AggLayer-connected chains, via the `miden-agglayer` v0.14-alpha library.

## Documentation

The [Milestone 1 Architecture Specification](https://github.com/darwin-miden/darwin-docs/blob/main/docs/m1-architecture-spec.md) is the technical reference for the Miden core layer.

## Repositories

| Repo | Purpose |
|---|---|
| [`darwin-protocol`](https://github.com/darwin-miden/darwin-protocol) | Core MASM: Protocol Account, basket faucets, asset faucets, note scripts. 76 tests, all green. |
| [`darwin-sdk`](https://github.com/darwin-miden/darwin-sdk) | Client SDK (Rust + TypeScript) wrapping `miden-client` and `miden-agglayer`; ships an off-chain rebalance planner. 18 tests. |
| [`darwin-baskets`](https://github.com/darwin-miden/darwin-baskets) | Versioned basket manifests, Rust loader, and the authoritative testnet inventory (`state/testnet.toml`). 18 tests. |
| [`darwin-oracle-adapter`](https://github.com/darwin-miden/darwin-oracle-adapter) | Pragma Oracle adapter and signed-attestation fallback. 11 tests. |
| [`darwin-bridge-adapter`](https://github.com/darwin-miden/darwin-bridge-adapter) | AggLayer bridge integration (B2AGG, CLAIM) and L1 wrapper-ERC20. 13 Foundry tests. |
| [`darwin-infra`](https://github.com/darwin-miden/darwin-infra) | Local dev stack and reproducible testnet deployment scripts. |
| [`darwin-docs`](https://github.com/darwin-miden/darwin-docs) | Architecture specs, getting-started guide, progress log, test report. |
| [`darwin-frontend`](https://github.com/darwin-miden/darwin-frontend) | Frontend at `darwin.xyz` (M3); TypeScript rebalance planner already mirrors the Rust SDK. |

## Roadmap

- **M1 (in progress)** — Miden core layer on testnet: three baskets, Flow A end-to-end, Pragma integration, AggLayer bridge primitives. 10 Darwin accounts deployed on public Miden testnet (4 asset faucets, 3 basket-token faucets, 3 protocol controllers) plus a user wallet bootstrapped with all four constituents for Flow A. See the [progress log](https://github.com/darwin-miden/darwin-docs/blob/main/docs/m1-progress.md).
- **M2** — Near Intent + Miden Guardian relay wallet for the ETH-user UX, automated rebalancing on the in-protocol Miden DEX, full Flow C (RedeemNote + AggLayer round-trip), third-party audit.
- **M3** — Frontend at `darwin.xyz`, mainnet deployment, public launch.

## License

All code is released under the MIT License.
