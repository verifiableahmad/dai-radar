---
name: Nillion
field: Secure MPC for ML
category: private-compute
foundation: foundations/mpc-ml.md
last_updated: 2026-10-20
confidence: grounded
website: "https://nillion.com"
---

# Nillion

## What it does

Nillion is a decentralized network for storing and computing on encrypted
("blind") data without ever reconstructing the plaintext on any single
node. Data is split and distributed across independent nodes; computation
happens directly on the shares via secure multi-party computation (MPC),
and only the final result is reassembled.

## Architecture summary

- **Nil Message Compute (NMC)** is Nillion's branding for its MPC layer.
  It combines **One-Time Masking (OTM)** with **Linear Secret Sharing
  (LSS)** so that a network of nodes can jointly store and process
  confidential data with information-theoretic security (unconditionally
  secure, not just computationally hard to break).
- The production MPC protocol built on top of this is internally called
  **Curl**, extended from LSSS to efficiently support non-linear
  operations (division, square roots, trig, logs) that plain linear
  secret sharing can't do without extra machinery.
- In 2026 Nillion migrated its base chain from Cosmos to Ethereum,
  launching a new L1 called **Blacklight** (mainnet live 2026-10-05).
  Node operators stake NIL (minimum 70,000 NIL) to run nodes and verify
  computations. The original Cosmos-based L2 is being wound down, with
  users told to bridge assets to the new L1.

## Built on (foundational lineage)

**Paper:** "Evaluation of Arithmetic Sum-of-Products Expressions in
Linear Secret Sharing Schemes with a Non-Interactive Computation Phase",
Miguel de Vega (Nillion), Andrei Lapets (Reity), Stanisław Jarecki
(UC Irvine), Wicher Malten (Nillion), Mehmet Ugurbil (Nillion), Wyatt
Howe (UCLA).
PDF: https://nillion.pub/sum-of-products-lsss-non-interactive.pdf
(originally posted to IACR ePrint as 2023/1740, later withdrawn from
ePrint but mirrored on Nillion's own site).

**Grounding source:** Nillion's own team announced this paper directly
as the basis for their non-interactive MPC protocol for non-linear
computation; see "Introducing the Nillion Non-Interactive MPC Protocol
for Non-Linear Computations"
(https://medium.com/@Nillion_Network/introducing-the-nillion-non-interactive-mpc-protocol-for-non-linear-computations-67e9310ef479)
and the MPC Alliance's coverage of the same announcement
(https://www.mpcalliance.org/blog/nillion-introduces-a-non-interactive-mpc-protocol-for-non-linear-computations).
Four of the six authors are Nillion staff, so this is effectively
Nillion publishing its own core protocol paper, about as direct a
grounding as this kind of claim can get.

**How it's used:** the paper's contribution, evaluating arithmetic
sum-of-products expressions (which covers the multiplications needed
for non-linear ops) within LSSS using a preprocessing phase instead of
interactive communication during computation, is the specific technique
that lets Curl/NMC do non-linear math without the usual back-and-forth
communication overhead of classic MPC protocols.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Underlying primitive | One-Time Masking + Linear Secret Sharing (LSS) | Nillion docs/marketing materials, cross-confirmed across multiple sources |
| Security model | Information-theoretic (unconditional) for the core LSS layer | Nillion technical articles |
| Core protocol name | Curl (non-interactive MPC extension over LSSS) | Nillion Medium post, MPC Alliance coverage |
| Base chain | Ethereum L1 ("Blacklight", launched 2026-10-05); migrated from Cosmos | TradingView/CoinMarketCal news, chain migration coverage |
| Node staking requirement | Minimum 70,000 NIL per node | Blacklight L1 launch coverage |
| Maturity | Mainnet live; L2 (prior Cosmos-based network) being sunset in 2026 | TokenPost, Bitget coverage of L2 shutdown |
| Reported uptime | 100% over trailing 90 days (as reported, not independently verified) | status.nillion.com |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | NOT COVERED | Node operators stake NIL to run nodes and verify computations; the verification mechanism isn't described | Not addressed in this file's sources |
| Trust rests on | MATH | Information-theoretic (unconditional) security for the core linear-secret-sharing layer | Nillion technical articles |
| Confidentiality | CRYPTOGRAPHIC | Data is secret-shared across nodes and computed on via MPC; plaintext is never reconstructed on one node | Nillion docs, cross-confirmed |
| Lineage | OWN PAPER | Sum-of-products in LSSS paper, four of six authors at Nillion; withdrawn from ePrint | Nillion Medium post + MPC Alliance coverage, grounded |
| Maturity | LIVE | Blacklight mainnet live 2026-10-05; the prior Cosmos-based L2 is being sunset | TokenPost, Bitget coverage |
| On-chain | OWN CHAIN | Blacklight, a new L1 reported as an Ethereum L1; migrated from Cosmos | TradingView/CoinMarketCal news |

## Known limitations

- The specific foundational paper above was **withdrawn from IACR ePrint**
  (official listing shows 2023/1740 withdrawn 2024-06-28); the only
  available copy is now self-hosted by Nillion at nillion.pub. Worth
  tracking why it was withdrawn: could be a correction, a priority
  dispute, or something more substantive. Flagged here as an open
  question rather than resolved.
- Nillion markets both MPC and homomorphic-encryption-style techniques
  under the "blind computation" umbrella; some public-facing material
  blurs which specific guarantee (information-theoretic MPC vs.
  computationally-secure HE) applies to which product surface. Treat
  marketing claims about "which crypto is doing the work" with caution
  until cross-checked against the underlying protocol docs.
- Mid-2026 chain migration (Cosmos → Ethereum L1) and L2 shutdown is
  recent and operationally disruptive, a useful thing to watch for
  whether it introduces new trust assumptions (e.g. bridge security)
  not present in the original MPC design.
