---
field: Decentralized / distributed training
paper: "DeMo: Decoupled Momentum Optimization"
authors: Bowen Peng, Jeffrey Quesnelle, Diederik P. Kingma
year: 2024
venue: arXiv
arxiv: https://arxiv.org/abs/2411.19870
---

# DeMo: Decoupled Momentum Optimization

## Why this is the seed for this field

Directly grounded, not inferred: the paper is authored by Nous Research
staff, and Nous's own Psyche architecture page states explicitly:
*"Building on DisTrO and its predecessor DeMo, Psyche reduces data
transfer by several orders of magnitude"*
([nousresearch.com/nous-psyche](https://nousresearch.com/nous-psyche)).
See [systems/nous-psyche.md](../systems/nous-psyche.md) for the full
chain. Code: [github.com/bloc97/DeMo](https://github.com/bloc97/DeMo).

This is deliberately kept separate from `decentralized-training.md`
(Proof-of-Learning, arXiv:2103.05633, the seed for Gensyn): both sit in
the same broad field but address different problems: Proof-of-Learning
is about *verifying* that training happened as claimed; DeMo is about
making distributed optimization *communication-efficient* enough to run
over consumer/internet-class links in the first place. Per this repo's
rule of one foundation file per distinct citation lineage, they stay as
two files even though the field label is the same.

## Core contribution

DeMo is a drop-in replacement for momentum-based optimizers (e.g. Adam)
designed to cut the communication volume required for distributed
training. It decouples local momentum updates across workers, applies
a fast orthonormal transform (DCT) to the momentum signal, keeps only
the top-k components (sparsification), and reuses the discarded
momentum as error feedback on the next step rather than dropping it.
The paper reports up to 85x less data transmitted per GPU than
AdamW-DDP on 300M–1B parameter models, while reaching comparable loss,
and the approach is topology-agnostic, meaning it doesn't assume a
particular interconnect (works over Ethernet/multi-datacenter links,
not just high-bandwidth cluster fabric).

## Link to tracked system

Nous's Psyche network is DeMo's direct production application: DisTrO
extends DeMo into a full runtime (adding load balancing and
selective-synchronization on top of the core compression idea), and
Psyche then wraps that runtime in a Solana-coordinated network so
contributors with heterogeneous, geographically dispersed hardware can
train one model together. See [systems/nous-psyche.md](../systems/nous-psyche.md)
for the full system and its known limitations. Notably, DeMo/DisTrO
solve the bandwidth problem but explicitly do **not** yet solve
verifying that a remote participant actually ran the claimed
computation honestly.
