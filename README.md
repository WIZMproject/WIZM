# WIZM — Post-Quantum Privacy Blockchain

![WIZM](assets/logo.png)

[![Devnet](https://img.shields.io/badge/devnet-live-34d399)](https://wizmproject.github.io/WIZM/)
[![Validators](https://img.shields.io/badge/validators-5-7aa2f7)]()
[![Consensus](https://img.shields.io/badge/consensus-PoA_10s-c4b5fd)]()
[![Signatures](https://img.shields.io/badge/signatures-Falcon--512-fbbf24)]()
[![Fee](https://img.shields.io/badge/fee-0.0001_flat-34d399)]()

**Live site:** https://wizmproject.github.io/WIZM/

---

## Vision

Public blockchains face two slow-moving cliffs at once: large-scale quantum computers break the elliptic-curve cryptography securing essentially every production ledger, and transparent ledgers publish every payment graph forever. WIZM answers both with the same posture — **post-quantum primitives everywhere, privacy by default, no ceremonies, no trusted setups, no special roles**.

WIZM combines transparent transfers, stealth transfers, shielded notes and unshielding on a five-validator proof-of-authority devnet sealing up to **1,200 transactions every 10 seconds** for a flat fee of **0.0001 WIZM**.

## Highlights

- **Quantum-safe throughout** — Falcon-512 signatures, ML-KEM encapsulation, BLAKE3 hashing, transparent Flock ZK proofs. No elliptic curves on any critical path.
- **Private by default** — shielded notes hide sender, receiver and amount; stealth addresses hide receivers; supply stays auditable via nullifiers and commitments.
- **Single branch, self-healing** — deterministic fork resolution (earlier seal wins, one-height unwind), seal-suppression gates, stall watchdog, pipelined sync. Laggards follow instead of forking.
- **Honest accounting** — every transaction that enters yet never seals surfaces in exactly one shed counter; client timeouts are never counted as rejects. Submitted ≈ committed, proven per cycle.
- **Measured, not promised** — ~120 TPS ceiling, 60–95 TPS sustained for days, zero chain-true rejects, 4M+ transactions per soak run.

## Architecture

| Layer | Duty | Key mechanism |
|---|---|---|
| 1 · Network | move bytes | gossipsub + QUIC + direct unicast fan-out, TLS API proxies |
| 2 · Consensus | one branch | 5-node PoA, 10s slots, deterministic reorgs, suppression gates |
| 3 · Execution | apply money | 4 tx families, flat fees, backpressured senders |
| 4 · State | remember | RocksDB, snapshots @2000, rewind journal 32 |
| 5 · Wallet | spend | Tauri desktop + browser sidecar, zero-config |

**Consensus in brief:** 10s slots with 4s grace, 360-height epochs, attempt rotation with 60s fallbacks. Same-height forks converge by earlier seal (lower-hash tiebreak) with one-height unwinds; fork evidence unwinds followers onto the mesh. Six seal-suppression gates (mesh-settle, blind-boot, behind, flap-aware, first-seen, syncing) keep lagging nodes following instead of forking. A stall watchdog, light checkpoints every 100 heights, and pipelined catch-up keep the chain converging under any conditions. Finality fast-forwards to tip−128, then advances by contiguous majority.

**Mempool in brief:** gap-buffered encrypted admission (500 MB / 200k entries / 90-min TTL) with per-sender floors, forfeit jumps over proven gaps, parked lanes, TTL sweep, lane re-grounding and a shed invariant that makes silent loss definitionally visible.

## Cryptography

| Primitive | Role | Parameters |
|---|---|---|
| Falcon-512 | signatures (txs, votes, transport) | 897B pk, variable-length sigs |
| ML-KEM-512 | mempool, committee shares, note keys | 800B pk / 768B ct |
| ML-KEM-768 | validator transport links | 1184B pk / 1088B ct |
| BLAKE3-256 | blocks, stealth, schedule, transcripts | single hash, domain-separated |
| Flock ZK | shielded transfers | 224B proofs (32+32+128+32), no setup, no VK files |

Shielded pool: binary Merkle tree, depth 22 (4,194,304 notes), 64-root window, fail-closed when full. Unshielding drains whole notes in fixed 348-byte envelopes.

## Tokenomics

- **Total supply:** 83,120,299.99 WIZM (8,312,029,999,000,000 base units, 8 decimals)
- **Emission:** 10 years in five 2-year periods, then zero:

| Period | Years | Emission (WIZM) | Step |
|---|---|---|---|
| 1 | 1–2 | 22,248,474.30 | — |
| 2 | 3–4 | 20,023,626.87 | −10% |
| 3 | 5–6 | 17,020,082.84 | −15% |
| 4 | 7–8 | 13,616,066.27 | −20% |
| 5 | 9–10 | 10,212,049.71 | −25% |

- **Fees:** 10,000 base units (0.0001 WIZM), identical for every transaction type — no auctions. Fees burn during emission, distribute to validators after.
- **Slashing:** pattern burn — 5% on the 3rd distinct double-seal offense within one epoch, burn-to-zero. Single collisions are recorded, never punished.

## Measured performance

October 2026 five-validator devnet, committed transactions only:

| Metric | Observed |
|---|---|
| Sustained throughput (SR ≈ CR) | 60–95 TPS for days, peaks of 117 |
| Burst throughput | 120–200 TPS phases, backfilled in drain phases |
| Block cadence | ≈10s, full 1,200-tx blocks |
| Block execution (1,200 txs) | 0.5–4.0s end-to-end |
| Chain-true rejects | 0 at ≤150 TPS levels |
| Single-run volume | 4M+ transactions committed |

## Documentation

- [Whitepaper](Docs/whitepaper.pdf) — **WIZM — Post-Quantum Privacy Blockchain**: architecture, cryptography, consensus, operations. The full project in detail.
- [Yellowpaper](Docs/yellowpaper.pdf) — formal specification: state transition, PoA mathematics, schemes with correctness equations, proofs.

## Contacts

- Email: [WIZM.project@proton.me](mailto:WIZM.project@proton.me)
- X: [@WIZM_PB](https://x.com/WIZM_PB)
- Discord: [chat](https://discordapp.com/users/1560226104994828358)
- Odysee: [@WIZM.project](https://odysee.com/@WIZM.project:5)

## This repository

```
WIZM/
├── index.html          # project landing page (black minimal hi-tech)
├── assets/logo.png     # project mark
└── Docs/
    ├── whitepaper.pdf  # full specification
    └── yellowpaper.pdf # formal specification
```
