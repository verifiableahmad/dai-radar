---
field: Decentralized / distributed training
paper: "DiLoCo: Distributed Low-Communication Training of Language Models"
authors: Arthur Douillard, Qixuan Feng, Andrei A. Rusu, Rachita Chhaparia, Yani Donchev, Adhiguna Kuncoro, Marc'Aurelio Ranzato, Arthur Szlam, Jiajun Shen
year: 2023
venue: Google DeepMind (arXiv preprint)
arxiv: https://arxiv.org/abs/2311.08105
---

# DiLoCo: Distributed Low-Communication Training of Language Models

## Why this is the seed for this field

Grounded via direct citation, not inference: Prime Intellect's own
OpenDiLoCo announcement states *"Introducing OpenDiLoCo, an open-source
implementation and scaling of DeepMind's Distributed Low-Communication
(DiLoCo) method"* and separately calls it *"a reproducible implementation
of DeepMind's DiLoCo experiments."*
([primeintellect.ai/blog/opendiloco](https://www.primeintellect.ai/blog/opendiloco)).
Prime Intellect's subsequent INTELLECT-1 technical report (arXiv:2412.01152)
builds its PRIME framework directly on this same DiLoCo core. See
[systems/prime-intellect.md](../systems/prime-intellect.md) for the full
chain.

This is a different seed paper from the one in
[foundations/decentralized-training.md](./decentralized-training.md)
(Proof-of-Learning, cited by Gensyn) despite sharing the same field: that
paper is about verifying training happened as claimed; this one is about
making distributed training communication-efficient in the first place.

## Core contribution

DiLoCo lets language-model training happen across "islands" of
poorly-connected devices instead of requiring tightly-interconnected
accelerators synchronizing every step. Each worker runs many local
optimization steps with an inner optimizer (AdamW); periodically, an
outer optimizer (SGD with Nesterov momentum) synchronizes pseudo-gradients
across workers. On the C4 dataset with 8 workers, DiLoCo matched fully
synchronous training quality while communicating roughly 500x less, and
the method is robust to workers dropping in and out during training.

## Link to tracked system

Prime Intellect's PRIME framework uses DiLoCo as the inter-node
optimization layer (paired with FSDP2 for intra-node sharding), which is
what let INTELLECT-1 train a 10B-parameter model across five countries and
three continents with nodes syncing only every ~38 minutes. See
[systems/prime-intellect.md](../systems/prime-intellect.md) for the full
architecture and property matrix.
