---
field: FHE for ML
paper: Deep Neural Networks for Encrypted Inference with TFHE
authors: Andrei Stoian, Jordan Frery, Roman Bredehoft, Luis Montero, Celia Kherfallah, Benoit Chevallier-Mames
year: 2023
venue: arXiv preprint (2302.10906)
---

# Deep Neural Networks for Encrypted Inference with TFHE

## Why this is the seed for this field

Authored by the team behind Zama's Concrete-ML, and uses Concrete directly
for its benchmarks; this is the paper that connects the general TFHE
cryptosystem to the specific problem of running deep neural network
inference on encrypted data, which is exactly what Concrete-ML ships as a
product. It sits one layer above the raw cryptography (TFHE itself, and
Chillotti/Joye/Paillier's programmable bootstrapping) and one layer below
the shipped library: the applied bridge.

## Core contribution

The paper shows how to design and quantize DNN architectures (targeted at
computer-vision tasks) so that they can run entirely under TFHE's
programmable bootstrapping, and benchmarks the resulting latency/accuracy
tradeoffs on Concrete. The core insight it operationalizes: FHE arithmetic
only works cleanly over integers, so every layer of the network has to be
re-expressed in quantized integer form *before* bootstrapping is used to
evaluate non-linearities; the paper is effectively the recipe for doing
that re-expression without destroying accuracy or blowing up latency.

## Link to tracked system

Concrete-ML's quantization-aware training pipeline and its approach to
evaluating activation functions via programmable bootstrapping are the
productionized version of what this paper demonstrates. See
`/systems/zama-concrete-ml.md` for the property matrix this paper's
techniques feed into, specifically the "Precision" and "Non-linear
evaluation" rows.
