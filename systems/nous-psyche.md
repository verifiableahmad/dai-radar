---
name: Psyche (Nous Research)
field: Decentralized / distributed training
category: training
foundation: foundations/distributed-optimization.md
last_updated: 2026-10-20
confidence: grounded
website: "https://nousresearch.com/nous-psyche"
---

# Psyche (Nous Research)

## What it does

Psyche is Nous Research's network for training large language models
using compute contributed from geographically dispersed, heterogeneous
machines across the internet, rather than a single co-located cluster.
It targets the bandwidth bottleneck that normally makes training over
consumer/Ethernet-class links impractical.

## Architecture summary

- Built on **DisTrO** (Distributed Training Over-the-Internet), Nous's
  in-house distributed optimizer stack, which in turn builds on **DeMo**
  (Decoupled Momentum Optimization), Nous's earlier optimizer paper.
  Nous's own Psyche page states directly: *"Building on DisTrO and its
  predecessor DeMo, Psyche reduces data transfer by several orders of
  magnitude."* ([nousresearch.com/nous-psyche](https://nousresearch.com/nous-psyche))
- Mechanism: decouples local momentum updates, applies a fast
  orthonormal transform (DCT) plus top-k sparsification, and reuses the
  momentum buffer as error feedback, described by Nous as compressing
  gradient communication the way JPEG compresses an image.
- Coordination layer runs on **Solana**: a smart-contract coordinator
  stores run metadata, handles state transitions, supplies randomness
  for node assignment, and acts as the synchronization authority across
  participants. Chosen specifically for fault-tolerance and censorship
  resistance, per Nous's own description.
- Largest known run to date: a 40B-parameter pretraining run on the
  testnet (May 2025), described by Nous as the largest internet-distributed
  pretraining run at the time.

## Built on (foundational lineage)

See [foundations/distributed-optimization.md](../foundations/distributed-optimization.md)
for the seed paper. Grounding is direct and strong: DeMo
(arXiv:2411.19870) is authored by Nous Research staff themselves (Bowen
Peng, Jeffrey Quesnelle, Diederik P. Kingma), and Psyche's own
documentation explicitly names both DisTrO and DeMo as what it's built
on, not an inferred or two-hop connection.

This is a **distinct lineage** from `foundations/decentralized-training.md`
(Proof-of-Learning, used for Gensyn): that paper addresses verifying
that training happened as claimed; DeMo/DisTrO addresses a different
problem: making distributed optimization communication-efficient
enough to train over the open internet at all. Same broad field,
genuinely different citation lineage, hence a separate foundations file
rather than reusing Gensyn's.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Core technique | DeMo (momentum decomposition) + DisTrO (compression/sync runtime) | nousresearch.com/nous-psyche, grounded |
| Reported communication reduction | DeMo: up to 85x less data per GPU than AdamW-DDP (arXiv:2411.19870); Psyche: "several orders of magnitude" per Nous | arXiv:2411.19870 + Nous's own page, grounded |
| Coordination/trust layer | Solana smart contract (metadata, state transitions, randomness, sync authority) | nousresearch.com/nous-psyche, grounded |
| Verification mechanism | **Not solved**: Nous states a theoretical model exists but "a practical implementation is still an open problem" | nousresearch.com/nous-psyche, grounded |
| Maturity | No formal mainnet as of this check; Nous explicitly avoids using the term and describes current operation as a post-testnet phase, not a graduation | Search aggregation across Nous's own Sept. 2025 post and docs; node/participant counts not independently verifiable |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | NONE FOUND | Nous: a practical verification implementation "is still an open problem" | nousresearch.com/nous-psyche, grounded |
| Trust rests on | OPERATORS | Compute contributors; Solana coordinates the run, but nothing yet verifies their updates | Classification inferred from the Verification mechanism row |
| Confidentiality | NOT COVERED |  | Not addressed in this file's sources |
| Lineage | OWN PAPER | DeMo (arXiv 2411.19870), by Nous Research staff | nousresearch.com/nous-psyche, grounded |
| Maturity | PREVIEW | No formal mainnet; a post-testnet phase; 40B-parameter testnet run in May 2025 | Search aggregation across Nous's own post and docs |
| On-chain | EXISTING CHAINS | Solana smart-contract coordinator (metadata, state, randomness, sync) | nousresearch.com/nous-psyche, grounded |

## Known limitations

- **Verification is explicitly unsolved.** Nous's own materials state
  this plainly rather than claiming a solution: floating-point
  non-determinism accumulates across billions of operations, DisTrO's
  compression introduces further cross-node variability, and bitwise
  verification is described as "difficult (but not impossible)." There
  is no deployed mechanism analogous to Gensyn's RepOps/bisection game
  for this network as of this check.
- The "balance problem" (how much cross-node divergence is acceptable
  before it could hide an attacker's influence) is described by Nous
  as an open empirical question, not yet answered.
- Network scale (node/participant counts) is not independently
  verifiable from public sources at this time.
