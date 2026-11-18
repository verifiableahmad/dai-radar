---
name: Vana
field: User-owned data for AI
category: ownership
foundation:
last_updated: 2026-10-27
confidence: grounded (Dec 2024 whitepaper); 2026 status unverified
website: "https://vana.org"
---

# Vana

## What it does

Lets users export their data from platforms and pool it into data
collectives that negotiate with AI companies and developers.

## Architecture summary

- **DataDAOs / Data Liquidity Pools (DLPs):** a DLP is a smart contract
  that instantiates a DataDAO around one data domain. Contributors get
  DLP-specific tokens (governance proportional to contribution).
- **Proof of Contribution:** each DLP defines its own scoring function
  (e.g. accuracy/completeness for financial data, freshness for health
  data), so scoring is not uniform across DLPs.
- **Validation and privacy:** validation runs on the Satya Network, a set
  of TEE nodes that attest to data quality without exposing the data;
  some DLPs also use zk proofs (zk email, zkTLS). Raw data stays
  encrypted on personal servers or in secure enclaves; only ownership
  proofs, permissions and attestations are on-chain.
- **VANA token:** validator staking, fees, DLP staking (the top 16
  DataDAOs by stake per 3-week epoch share emissions), and the currency AI
  companies use to buy/burn DLP tokens for data access.
- The whitepaper (Dec 2024) is pre-mainnet and forward-looking; nothing
  from 2026 was found, so current status is unknown.

## Built on (foundational lineage)

None found. Foundation intentionally empty. Compare `/systems/ocean-protocol.md`
for the older data-marketplace approach.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Quality control | Proof-of-Contribution (details unread) | Vana whitepaper listing |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | ATTESTATION | Per-DLP Proof of Contribution scored by TEE validator nodes (Satya Network); some DLPs add zk proofs | Vana whitepaper, grounded |
| Trust rests on | HARDWARE VENDOR | TEE trust model unspecified in the whitepaper; consensus also unspecified | Vana whitepaper, grounded (absence) |
| Confidentiality | HARDWARE ENCLAVE | Raw data stays encrypted off-chain on personal servers or in enclaves | Vana whitepaper, grounded |
| Lineage | NONE FOUND |  | File's lineage note, inferred |
| Maturity | OPEN QUESTION | Nothing from 2026 found; current status unknown | 2024 whitepaper listing, inferred |
| On-chain | NOT COVERED |  | Not addressed in this file's sources |

## Known limitations

- Consensus and the TEE trust model are not specified in the whitepaper.
- Per-DLP scoring means quality bars differ across DataDAOs.
- Some staged-access and burn mechanics are given as examples, not
  confirmed implementations; Dec 2024 content may be stale.
