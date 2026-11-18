---
field: Decentralized GPU / compute marketplace (orchestration layer)
paper: "Ray: A Distributed Framework for Emerging AI Applications"
authors: Philipp Moritz, Robert Nishihara, Stephanie Wang, Alexey Tumanov, Richard Liaw, Eric Liang, Melih Elibol, Zongheng Yang, William Paul, Michael I. Jordan, Ion Stoica
year: 2018
venue: 13th USENIX Symposium on Operating Systems Design and Implementation (OSDI '18)
link: https://www.usenix.org/system/files/osdi18-moritz.pdf
---

## Why this is the seed for this field

Grounded via a two-hop citation chain, not a direct io.net citation:

- io.net's own architecture docs state directly that its orchestration
  layer, IO-SDK, is *"our specialized fork of Ray, a core technology
  driving io.net's capabilities"* (version base: Ray 2.3.0).
  ([io.net/docs/guides/architecture/io-architecture](https://io.net/docs/guides/architecture/io-architecture))
- Ray is the subject of its own academic systems paper from UC
  Berkeley's RISELab, presented at OSDI 2018.

See `/systems/io-net.md` for the full source chain and for the explicit
distinction between this (orchestration-engine) lineage and io.net's
separate, unsourced trust/verification mechanisms.

## Core contribution

Ray's paper introduces a distributed execution framework for AI
workloads that unifies two previously separate programming models under
one dynamic task graph and a single system: fine-grained, stateless task
parallelism (as in MapReduce-style systems) and stateful actor-based
computation. It uses a bottom-up distributed scheduler
(rather than a single centralized scheduler) and a fault-tolerant
distributed metadata store, specifically to support workloads common in
AI applications (simulation, training, and serving) that mix
short, latency-sensitive tasks with long-running, stateful ones, which
earlier batch-oriented distributed systems weren't designed for.

## Link to tracked system

io.net uses a fork of Ray as the execution substrate for distributing
and scheduling GPU workloads across its marketplace of
provider-contributed hardware; i.e., Ray solves the general
"orchestrate many parallel/stateful tasks across a cluster" problem,
and io.net's contribution on top is specifically the
decentralization/marketplace/trust layer (payment, resource-integrity
verification, provider onboarding), which this paper does not address
and which io.net's own docs don't tie to any further academic source.
This is a general distributed-systems paper, not an AI-security- or
cryptography-specific one; it's the seed for the *compute
orchestration* side of this field, not the *trust/verification* side.
