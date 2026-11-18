---
name: Zama Concrete-ML
field: FHE for ML
category: private-compute
foundation: foundations/fhe-ml.md
last_updated: 2026-10-20
confidence: grounded
website: "https://github.com/zama-ai/concrete-ml"
---

# Zama Concrete-ML

## What it does

Concrete-ML is an open-source Python framework from Zama that lets you train
a model normally (scikit-learn-compatible APIs, or PyTorch/TensorFlow via
ONNX) and then run inference on **encrypted** inputs, so the service running
the model never sees the plaintext data. Output of `encrypt()` → `run()` →
`decrypt()` never exposes intermediate values to the server.

## Architecture summary

- Built directly on top of **Concrete** (Zama's TFHE compiler, which turns
  Python/numerical programs into their FHE equivalent), which itself is
  built on **TFHE-rs**, Zama's Rust implementation of the TFHE scheme.
- Models are **quantized** (quantization-aware training or post-training
  quantization) to convert floating-point weights/activations into
  integers, since FHE arithmetic operates over integers, not floats.
- Non-linear activation functions (ReLU, sigmoid, etc.) are evaluated via
  **programmable bootstrapping**: TFHE's bootstrap step is repurposed to
  simultaneously refresh noise *and* apply an arbitrary lookup-table
  function, which is what makes homomorphic neural-net inference
  practical rather than prohibitively expensive.

## Built on (foundational lineage)

- **TFHE: Fast Fully Homomorphic Encryption over the Torus**, I. Chillotti,
  N. Gama, M. Georgieva, M. Izabachène, *Journal of Cryptology* 33, 2020
  (https://eprint.iacr.org/2018/421). Base encryption scheme. Grounding:
  this is the standard citation for TFHE across Zama's own repos
  (zama-ai/concrete, zama-ai/tfhe-rs) and third-party papers that build on
  Concrete, confirmed via multiple independent academic citations, though
  the Concrete-ML GitHub README itself does not carry an explicit
  bibliography (see confidence note below).
- **Programmable Bootstrapping Enables Efficient Homomorphic Inference of
  Deep Neural Networks**, I. Chillotti, M. Joye, P. Paillier, Zama
  (https://eprint.iacr.org/2021/091). The specific mechanism Concrete-ML
  relies on to evaluate non-linear layers cheaply.
- **Deep Neural Networks for Encrypted Inference with TFHE**, A. Stoian,
  J. Frery, R. Bredehoft, L. Montero, C. Kherfallah, B. Chevallier-Mames
  (arXiv:2302.10906, Feb 2023). Authored by the Zama/Concrete-ML team
  itself; uses Concrete directly for benchmarks. This is the most directly
  "FHE-for-ML"-specific paper in the lineage; it's the applied bridge
  between the TFHE primitives above and Concrete-ML's DNN support.

**Confidence note:** the Concrete-ML GitHub README links out to
docs.zama.ai/docs.zama.org for technical detail rather than citing papers
inline. The TFHE/programmable-bootstrapping lineage above is grounded via
Zama's own sister repos (tfhe-rs, concrete) and the Stoian et al. paper's
direct authorship/use of Concrete, not via a single README citation list.
Treat as grounded-by-convergence rather than grounded-by-single-citation.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| FHE scheme | TFHE (LWE/Ring-LWE based) | zama-ai/concrete, zama-ai/tfhe-rs repos |
| Non-linear evaluation | Programmable bootstrapping (lookup-table via bootstrap) | Chillotti/Joye/Paillier 2021 |
| Precision | Low-bit integer quantization required (model-dependent, typically ≤8-bit activations in practice) | Concrete-ML docs (docs.zama.ai/concrete-ml) |
| Trust model | No trusted setup; client holds secret key, server computes blind | TFHE scheme properties (symmetric/public-key FHE, no CRS) |
| Maturity | Production-maintained OSS library, active releases | github.com/zama-ai/concrete-ml |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | NOT COVERED | FHE hides data from the server; the file describes no check that the server ran the model correctly | Not addressed in this file's sources |
| Trust rests on | MATH | TFHE (LWE-based) hardness; no trusted setup; the client holds the secret key | TFHE scheme properties, grounded |
| Confidentiality | CRYPTOGRAPHIC | Inference on encrypted inputs; the server never sees plaintext or intermediate values | Concrete-ML README, grounded |
| Lineage | OWN PAPER | Seed paper is the Concrete-ML team's DNN-over-TFHE work (arXiv 2302.10906), built on TFHE (eprint 2018/421) | Zama repos + Stoian et al., grounded by convergence |
| Maturity | LIVE | Production-maintained open-source library with active releases | github.com/zama-ai/concrete-ml, grounded |
| On-chain | NO CHAIN | A Python library; no blockchain component in the cited sources | Inferred from absence in the cited sources |

## Known limitations

- Bootstrapping is the dominant cost; deeper/wider networks with more
  non-linearities scale poorly in latency compared to plaintext inference.
- Quantization-aware training adds workflow overhead and can cost accuracy
  versus the unquantized model.
- Encrypted inference latency (seconds–minutes per call depending on model
  size) remains far from plaintext inference speed, so not yet suited to
  low-latency, high-throughput serving.
