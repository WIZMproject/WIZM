# WIZM

Post-quantum payment chain with transparent and stealth transfers.

## Current protocol

- Five-validator PoA, 10-second steady slots.
- Falcon-512 signatures; ML-KEM-512 encrypted mempool/committee; ML-KEM-768 transport; BLAKE3; AES-256-GCM.
- **Transparent** transfers: sender, recipient and amount public.
- **Stealth** transfers: one-time recipient address hides the sender-to-recipient link only. Sender and amount are public.
- Transparent and stealth transfers only; sender and amount are public.
- Maximum **1,200 transactions / 8 MiB per block**, or **120 TPS** at the 10-second slot.
- Flat **10,000 base-unit** fee; first 20 fee-paying transactions per wallet per block are burst-free, then 10x, 100x, 1,000x, etc.
- Supply approximately 83,120,300 WIZM, emitted over ten years.
- Native wallet keys use `~/.wizm/wallets` with `0700/0600`; browser wallet keys are session-only.

## Documents

- `Docs/whitepaper.pdf` — current white paper.
- `Docs/yellowpaper.pdf` — current formal specification.
- `Docs/whitepaper.tex` and `Docs/yellowpaper.tex` — LaTeX sources.
- `index.html` — current public website.

## Repository scope

This repository contains the public website and documents only. The
chain implementation is maintained separately.
