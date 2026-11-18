---
field: Decentralized / distributed training
paper: "Proof-of-Learning: Definitions and Practice"
authors: Hengrui Jia, Mohammad Yaghini, Christopher A. Choquette-Choo, Natalie Dullerud, Anvith Thudi, Varun Chandrasekaran, Nicolas Papernot
year: 2021
venue: IEEE Symposium on Security and Privacy (S&P) 2021
arxiv: https://arxiv.org/abs/2103.05633
---

# Proof-of-Learning: Definitions and Practice

## Why this is the seed for this field

Cited directly by Gensyn's own Verde research post as foundational prior work
("Proof-of-Learning", [arxiv.org/abs/2103.05633](https://arxiv.org/abs/2103.05633)),
alongside Refereed Delegation, Agatha, Toploc, and Proof-of-Training-Data. See
[systems/gensyn.md](../systems/gensyn.md) for the full citation list and source
link. Of that list, this paper is the one that directly frames the core problem
Gensyn's verification layer exists to solve: how do you let a party that didn't
do the training convince a verifier that specific gradient-descent work was
actually performed, without full re-execution.

## Core contribution

Introduces "proof-of-learning" (PoL): a party that trained a model produces a
proof, built from checkpoints and the gradient-descent trajectory metadata
recorded during training, that lets a verifier check, cheaply, that training
actually happened as claimed, rather than requiring the verifier to redo the
entire training run. It formalizes what such a proof needs to guarantee and
shows practical constructions and attacks against naive versions of the idea.

## Link to tracked system

Gensyn's verification problem is exactly this: a client needs to trust that a
third-party provider trained a model correctly without re-running the full
job. Gensyn's current production system (Verde) does not implement PoL's
checkpoint-trajectory scheme directly; it instead uses RepOps
(bitwise-reproducible operators) plus a Merkle-tree-based bisection/dispute
game, but PoL is the paper that established the problem framing and the
first concrete proposal in this space, which is why Gensyn cites it as prior
work. See [systems/gensyn.md](../systems/gensyn.md) for how the current
architecture actually differs from PoL's approach.
