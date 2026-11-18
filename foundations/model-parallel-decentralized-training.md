---
field: Decentralized / distributed training
paper: "Subspace Networks: Scaling Decentralized Training with Communication-Efficient Model Parallelism"
authors: Sameera Ramasinghe, Thalaiyasingam Ajanthan, Gil Avraham, Yan Zuo, Alexander Long
year: 2025
venue: NeurIPS 2025 (arXiv preprint, originally titled "Protocol Models")
arxiv: https://arxiv.org/abs/2506.01260
---

# Subspace Networks: Scaling Decentralized Training with Communication-Efficient Model Parallelism

## Why this is the seed for this field

Authored directly by the Pluralis Research team (affiliation explicitly
stated in the paper itself, confirmed via
[arxiv.org/html/2506.01260v1](https://arxiv.org/html/2506.01260v1)), not a
third-party citation. See
[systems/pluralis-research.md](../systems/pluralis-research.md) for the
grounding detail, including the title change between preprint versions
(same arXiv ID, retitled from "Protocol Models" to "Subspace Networks").

This is a **distinct lineage** from `foundations/decentralized-training.md`
(Proof-of-Learning, arXiv:2103.05633, which Gensyn's Verde cites). That
paper addresses verifying that training happened; this paper addresses
making model-parallel training communication-efficient enough to run over
consumer internet connections at all. Same field, different problem, kept
as separate foundation files per this repo's one-file-per-lineage rule.

## Core contribution

When a large model is split across devices (model/pipeline parallelism),
every forward and backward pass requires transferring activation and
gradient tensors between devices (tens to hundreds of MB per step), which
is fine over datacenter interconnects but becomes the binding bottleneck
over consumer internet. The paper's compression algorithm exploits a
recursive structure in transformer networks to confine activations and
gradients to a predefined low-dimensional subspace, permitting full
reconstruction in later layers. This gets up to ~100x compression on
inter-stage communication without materially altering training dynamics or
degrading convergence, which is what makes multi-participant, model-parallel
training over ordinary internet connections practical rather than purely
theoretical.

## Link to tracked system

Pluralis Research's production system, Agora, uses this technique directly
as the mechanism that makes its pipeline-parallel training swarm viable;
without it, the inter-device communication cost of splitting a model across
unreliable, internet-connected consumer hardware would dominate and make
the whole approach impractical. See
[systems/pluralis-research.md](../systems/pluralis-research.md) for the
full system write-up, including the honest gap that this solves the
communication-efficiency problem but not trust/verification of contributor
honesty.
