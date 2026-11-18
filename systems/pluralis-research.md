---
name: Pluralis Research
field: Decentralized / distributed training
category: training
foundation: foundations/model-parallel-decentralized-training.md
last_updated: 2026-10-20
confidence: grounded (core technique); inferred (some matrix cells)
website: "https://pluralis.ai"
---

# Pluralis Research

## What it does

Pluralis Research runs **Protocol Learning**: large language models trained
collaboratively across the open internet on heterogeneous, unreliable
consumer/cloud hardware that no single participant fully owns, contrasted
with centralized datacenter training. Their production system for this is
called **Agora**.

## Architecture summary

- **Pipeline parallelism**: each participating device hosts one pipeline
  stage (e.g. one transformer block); multiple devices can replicate a stage
  for redundancy.
- **Data parallelism**, combined with the above, to distribute training
  further across the swarm.
- **Peer-to-peer routing**: participants join/leave dynamically without
  halting training.
- **Subspace Networks** (see Built on, below): the specific compression
  technique that makes cross-device communication cheap enough for this to
  work over consumer internet rather than datacenter interconnects: up to a
  100x reduction in inter-stage communication, by confining activations and
  gradients to a predefined low-dimensional subspace exploiting a recursive
  structure in transformer networks.
- Source: [pluralis.ai/docs](https://pluralis.ai/docs/) (Agora system
  overview).

## Built on (foundational lineage)

**Paper**: originally titled "Protocol Models: Scaling Decentralized
Training with Communication-Efficient Model Parallelism," since retitled
(as of v3, 2026-07-09) to **"Subspace Networks: Scaling Decentralized
Training with Communication-Efficient Model Parallelism."** Same arXiv ID
throughout: [arxiv.org/abs/2506.01260](https://arxiv.org/abs/2506.01260).
Authors: Sameera Ramasinghe, Thalaiyasingam Ajanthan, Gil Avraham, Yan Zuo,
Alexander Long. **Affiliation explicitly listed as Pluralis Research** in
the paper itself (confirmed via the v1 HTML:
[arxiv.org/html/2506.01260v1](https://arxiv.org/html/2506.01260v1)).
Accepted at NeurIPS 2025.

**Grounding note**: this is the team's own paper, not a third-party citation,
same tier of grounding as Zama/Stoian et al. or Nillion's Curl paper
elsewhere in this repo (the system's own team publishing the core technique
directly), not a case of the product's docs citing someone else's work. The
title change between versions (Protocol Models → Subspace Networks, same
arXiv ID) was confirmed directly rather than assumed: an initial search hit
returned the old title and could have been reported as current without the
extra check.

**Not the same lineage as** `foundations/decentralized-training.md`
(Proof-of-Learning, arXiv:2103.05633, used by Gensyn): that paper is about
*verifying* that training happened; this one is about making model-parallel
training communication-efficient enough to run over consumer internet at
all. Different problem, different citation lineage, hence a separate
foundations file rather than reusing the existing one despite the shared
field.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Parallelism approach | Hybrid: pipeline parallelism (one stage per device, replicated for redundancy) + data parallelism | pluralis.ai/docs, grounded |
| Core compression technique | Subspace Networks: up to ~100x inter-stage communication reduction | arXiv:2506.01260, grounded |
| Bandwidth target | Trains over consumer-grade internet (as low as ~80Mbps per search-aggregated reporting of the paper's claims) matching datacenter-connected (100Gbps) convergence | General sourcing of paper claims, **inferred**; recheck exact figures against the paper directly before citing precisely |
| Trust / verification mechanism | Informal points-and-leaderboard system for contributors; **no cryptographic or economic verification layer found** (no blockchain, no token, no dispute/fraud-proof game) | pluralis.ai/docs, grounded (as an absence, not just unconfirmed) |
| Maturity | Research-phase, active: Pluralis-13B run (Sept 2026, 62.4B tokens); prior Pluralis-8B (May 2026, 531.8B tokens) | pluralis.ai/docs, grounded |
| Node0 run (separate practical milestone) | Reported 7.5B-parameter run over ~3 weeks, 300+ participants, 1,642 GPUs | Cross-referenced across Pluralis's own GitHub (PluralisResearch/node0) and third-party coverage, **inferred/not independently re-verified this pass** |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | NONE FOUND | Informal points-and-leaderboard; no cryptographic or economic verification layer | pluralis.ai/docs, grounded (as an absence) |
| Trust rests on | OPERATORS | Contributors, tracked only by points and a leaderboard | Classification inferred from the verification row |
| Confidentiality | NOT COVERED |  | Not addressed in this file's sources |
| Lineage | OWN PAPER | Subspace Networks (arXiv 2506.01260, NeurIPS 2025), by Pluralis authors | arXiv 2506.01260, grounded |
| Maturity | RESEARCH | Research-phase runs: Pluralis-8B (May 2026), Pluralis-13B (Sept 2026) | pluralis.ai/docs, grounded |
| On-chain | NO CHAIN | No blockchain and no token | pluralis.ai/docs, grounded |

## Known limitations

- No cryptographic, economic, or formal verification layer for contributor
  honesty as of this check. Unlike Gensyn's bisection/dispute game, Pluralis
  currently relies on an informal points/leaderboard system. This is a
  meaningful gap for a "decentralized AI + crypto" framing specifically:
  Pluralis solves the communication-efficiency half of decentralized
  training well, but not the trust/verification half.
- Bandwidth and compression figures above are drawn from aggregated
  reporting of the paper's claims, not independently re-derived from the
  paper's experiments section in this pass; worth a direct read of
  arXiv:2506.01260 before citing specific numbers.
- No blockchain/token component; if a future watch-run finds Pluralis
  adding one, that would be a genuine matrix-impacting change worth a
  companion note.
