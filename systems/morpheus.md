---
name: Morpheus
field: Decentralized inference marketplace
category: compute-markets
foundation:
last_updated: 2026-10-27
confidence: inferred (promotional and secondary sources only)
website: "https://mor.org"
---

# Morpheus

## What it does

A decentralized marketplace for open-weight model inference. Providers
stake/offer compute; a router (built on Lumerin's code, originally for
decentralized Bitcoin mining) connects sessions to providers, with
smart-contract payment. Originally pitched around personal "Smart Agents";
a 2026-08 THORChain write-up describes it as an inference marketplace.

## Architecture summary

- Contracts on Ethereum, Arbitrum and Base (secondary source).
- MOR rewards compute providers and gives holders a share of network
  compute activity (launched with a $20M provider reward program).
- Phala announced TEE-based verifiable compute for Morpheus/Lumerin
  (2024). A TEE provider with Secret Network models was added 2026-08-07
  (see `/systems/secret-network.md`).

## Built on (foundational lineage)

No paper is cited. Foundation intentionally left empty: no real
citation chain was found.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Routing | Lumerin-derived smart-contract router | Phala partnership post |
| Privacy | Optional, via TEE providers | Secret Network post |
| Usage | 39B tokens processed (single podcast write-up) | THORChain blog |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | NOT COVERED |  | Not addressed in this file's sources |
| Trust rests on | OPERATORS | Compute providers reached through a smart-contract router; TEE providers are optional | Partner posts, inferred |
| Confidentiality | PER APP | Base inference isn't private; privacy depends on choosing a TEE provider | Secret Network post, inferred |
| Lineage | NONE FOUND | No paper cited; the router is built on Lumerin's Bitcoin-mining code | File's lineage note, inferred |
| Maturity | LIVE | 39B tokens processed, per a single podcast write-up | THORChain blog, inferred |
| On-chain | EXISTING CHAINS | Contracts on Ethereum, Arbitrum and Base | Secondary source, inferred |

## Known limitations

- Almost every source is promotional or partner-authored; Morpheus's own
  docs were not reached (morpheus.network is an unrelated supply-chain
  company). Verify against the project's own documentation.
- Base network inference is not private; privacy depends on the provider
  chosen.
