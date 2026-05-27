# Darwin Protocol

**Confidential basket protocol on Miden.**

Darwin lets users deposit underlying crypto assets and receive a single basket token whose Net Asset Value tracks a weighted index. All basket operations run natively on Miden with client-side STARK proofs — individual positions, balances, and NAV remain private by default.

ETH-native users skip Miden entirely: deposit ETH on Sepolia via 1Click or the canonical AggLayer (Bali) bridge, hold a Darwin basket position via the relay, and redeem back to Sepolia ETH via a trustless `claimAsset` call — all from a single wallet, in under an hour.

## Where to start

**Reviewers / integrators:**

- [`darwin-miden.github.io/darwin-docs/demo`](https://darwin-miden.github.io/darwin-docs/demo) — end-to-end UX walkthrough with live tx hashes captured during verification (read this first)
- [`darwin-miden.github.io/darwin-docs/status`](https://darwin-miden.github.io/darwin-docs/status) — verified live state of every component
- [`darwin-miden.github.io/darwin-docs/architecture`](https://darwin-miden.github.io/darwin-docs/architecture) — system shape + storage layout
- [`darwin-miden.github.io/darwin-docs/contracts`](https://darwin-miden.github.io/darwin-docs/contracts) — every live Miden testnet + Sepolia address

## What we have built

- **Four curated baskets**: Core Crypto (BTC/ETH-dominant), Aggressive (pure crypto), Conservative (stable-heavy), Privacy Pack (catalog).
- **v6 basket controller** on Miden testnet (`0x2a3ea0a268d97b80497d6a966e3141`) with full storage maps for target weights, fees, per-user positions, and fee-recipient routing.
- **Pragma Oracle integration** for in-circuit NAV, with a transparent per-pair CoinGecko fallback when a Pragma testnet publisher is clearly broken. Read-only NAV view through the frontend: **p99 = 24 ms** (the proposal's 200 ms claim, 10× under).
- **Canonical AggLayer bridging** (Bali, network 76) for both directions: user-driven `bridgeAsset` for inbound, worker-driven B2AGG note + `claimAsset` for outbound. Full L1↔L2 round-trip verified end-to-end on testnet.
- An **ETH-side relay** (`darwin-relay`) so users without a Miden wallet deposit and redeem via either the 1Click broker (fast) or canonical Bali (trustless).
- **Frontend** with deposit panels (1Click / Miden-native), portfolio panels (positions, redemptions, claims), live NAV cards, and the BaliClaimPanel that wraps `claimAsset` calldata.
- **Observability**: relay `/metrics` Prometheus exposition + Grafana dashboard JSON; Playwright public-surface E2E (10/10 passing).

## Repositories

| Repo | Purpose |
|---|---|
| [`darwin-protocol`](https://github.com/darwin-miden/darwin-protocol) | Core MASM: protocol-account controllers (v2/v4/v5/v6), basket-faucet code, atomic deposit/redeem notes |
| [`darwin-sdk`](https://github.com/darwin-miden/darwin-sdk) | Client SDK (Rust + TypeScript), off-chain rebalance planner, live Uniswap swap-exec script |
| [`darwin-baskets`](https://github.com/darwin-miden/darwin-baskets) | Versioned basket manifests + Rust loader |
| [`darwin-oracle-adapter`](https://github.com/darwin-miden/darwin-oracle-adapter) | Pragma Oracle adapter + signed-attestation fallback |
| [`darwin-bridge-adapter`](https://github.com/darwin-miden/darwin-bridge-adapter) | AggLayer bridge integration (B2AGG / CLAIM) + L1 wrapper ERC20s |
| [`darwin-relay`](https://github.com/darwin-miden/darwin-relay) | ETH-side escrow + Rust relay v2 service (REST + worker, canonical B2AGG outbound, Prometheus `/metrics`) |
| [`darwin-infra`](https://github.com/darwin-miden/darwin-infra) | Local dev stack, deployment scripts, `bali-l1-claim.sh` + `bali-mock-bridge-up.sh` |
| [`darwin-frontend`](https://github.com/darwin-miden/darwin-frontend) | Next.js 15 frontend at `darwin.xyz` |
| [`darwin-docs`](https://github.com/darwin-miden/darwin-docs) | Documentation site at [darwin-miden.github.io/darwin-docs](https://darwin-miden.github.io/darwin-docs/) |

All repos are tagged at `v0.4.0-m3` (or `v0.4.1-m3` for `darwin-relay`) marking the demo-ready milestone.

## License

All code is MIT.
