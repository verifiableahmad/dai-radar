---
name: Lagrange Labs (DeepProve)
field: ZKML / verifiable inference
category: verifiable-inference
foundation: foundations/zkml-sumcheck.md
last_updated: 2026-11-13
confidence: grounded (core technique lineage, commitment scheme); inferred (setup-trust reading, production SRS, some performance framing)
website: "https://www.lagrange.dev"
---

## What it does

DeepProve is Lagrange Labs' zkML system: it generates zero-knowledge
proofs that a specific LLM produced a specific output sequence from a
specific input, without re-running the model or trusting the party that
did. Unlike earlier zkML work limited to single tokens or isolated
components, DeepProve targets full end-to-end LLM inference — it
certifies that the whole output sequence is correct rather than
encoding the entire inference computation in-circuit, which is what
lets it avoid the quadratic circuit blowup or expensive RAM modeling
that sank earlier approaches.

Lagrange Labs is a separate project from, but shares the "ZK
coprocessor" branding with, Lagrange's other product line — a
decentralized ZK Prover Network / ZK Coprocessor for proving SQL
queries over on-chain data, which (per Lagrange's own docs) uses
Plonky2. **DeepProve is not that system** — it's a distinct proving
construction built specifically for ML inference. Don't conflate the
two when reading Lagrange's broader materials.

## Architecture summary

- Core building blocks, per DeepProve's own paper abstract: the
  **sum-check protocol** and **lookup arguments**, used to verify
  individual LLM operators (multi-head attention, layer normalization,
  grouped-query attention, rotary positional embeddings) without
  encoding the full computation as one monolithic circuit.
- Native format support: ONNX, safetensors, and GGUF — so models
  exported from PyTorch, TensorFlow, JAX, or Hugging Face run through
  DeepProve unchanged (no custom model-authoring step required).
- Positioned explicitly against two named prior systems: **zkGPT**
  (USENIX'25, single-token proofs) and **zkLLM** (CCS'24, proved only
  the Softmax component) — DeepProve's claimed contribution is doing
  both end-to-end and faster.

## Built on (foundational lineage)

**Two-hop grounding**, same pattern as this repo's EZKL entry:

1. DeepProve's own paper — *"DeepProve: Verifiable End-to-End Large
   Language Model Inference"* (IACR ePrint
   [2026/1112](https://eprint.iacr.org/2026/1112), accepted CCS 2026;
   authors include Yupeng Zhang and Charalampos Papamanthou, both
   established authors in prior sum-check-based SNARK literature) —
   explicitly names the **sum-check protocol** as a core building
   block in its own abstract.
2. The sum-check protocol's original paper: Lund, Fortnow, Karloff,
   Nisan, *"Algebraic methods for interactive proof systems,"* Journal
   of the ACM 39(4), 1992. This is the standard, widely-cited origin
   of the technique (confirmed via multiple independent sources,
   including the paper's own author describing it on social media as
   his most-cited work specifically because of its zkSNARK reuse).

**Not confirmed from a direct source in this pass:** whether DeepProve's
paper cites the 1992 paper by name in its bibliography, versus citing
an intermediate sum-check-based SNARK (GKR, Libra, Virgo, Spartan,
HyperPlonk, or Lasso/Jolt, all of which build on the same 1992
technique) — the abstract fetch used for this entry didn't expose the
full reference list. Treat the specific DeepProve→1992-paper link as
the standard lineage for "a sum-check-based proving system," not as a
confirmed direct citation in DeepProve's own bibliography.

See `foundations/zkml-sumcheck.md` — this is a **separate lineage**
from `foundations/zkml.md` (PLONK/Halo2, used by EZKL). Both files
share the field "ZKML / verifiable inference" but root in genuinely
different proving techniques (PLONKish arithmetization vs.
sum-check-based verification), so they're kept as distinct foundation
entries rather than merged.

## Property matrix

| Property | Value | Source |
|---|---|---|
| Proving technique | Sum-check protocol + lookup arguments (not PLONKish arithmetization) | DeepProve paper abstract, grounded |
| Model format support | ONNX, safetensors, GGUF | DeepProve paper abstract / lagrange.dev, grounded |
| Claimed proving speedup | 20–60× faster than prior SOTA (paper abstract framing); a separate press release states "60×faster / 671× faster verification" — the two framings weren't reconciled in this pass, may refer to different baselines/benchmarks | DeepProve paper abstract; Lagrange press release — **flagged discrepancy, not resolved** |
| Example throughput | GPT-2: 174 tokens/min; Gemma 3: 86 tokens/min; verification in 1–3.7 seconds | DeepProve paper abstract, grounded |
| Production track record | 12M+ cryptographic proofs generated, 3M+ AI inferences verified end-to-end, as of the open-source release (June 2026) | Lagrange press release, grounded |
| Trust model (trusted setup or transparent) | Trusted setup, not transparent: the public code commits with **HyperKZG over BN254**, a KZG-based scheme, and builds its parameters locally with `test_setup`, which calls arkworks' `KZG10::setup` with a caller-supplied RNG. No ceremony SRS is loaded anywhere in the public code. Whether Lagrange's hosted prover uses a ceremony SRS instead was not verified. The repo's older docs page (`docs/src/commitments.md`, from 2025) still names Basefold, a transparent scheme; the README and the code are newer and both say HyperKZG | deep-prove README and source at commit 9d1a53e, dp-crypto `arkyper` source, grounded; "needs a trusted setup" follows from KZG and is inferred; first flagged by arXiv 2611.17042 (Table III) |
| Maturity | Open-sourced June 2026; github.com/Lagrange-Labs/deep-prove; live dashboard at deep-prove.lagrange.dev | Lagrange press release / blog, grounded |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | PROOF | Sum-check + lookup-argument proofs of end-to-end LLM inference; verification in 1-3.7 s | DeepProve paper abstract, grounded |
| Trust rests on | MATH + SETUP | Cryptographic soundness plus a KZG structured reference string (HyperKZG, BN254); the open-source code generates it locally with `test_setup`, not from a ceremony | deep-prove README and source, grounded; classification inferred, see Property matrix |
| Confidentiality | NOT COVERED | The file describes proving correctness, not what DeepProve keeps hidden | Not addressed in this file's sources |
| Lineage | EXTERNAL PAPER | DeepProve names the sum-check protocol (Lund, Fortnow, Karloff, Nisan 1992); a direct bibliography citation is unconfirmed | DeepProve paper abstract; 1992 link is the standard lineage, inferred |
| Maturity | LIVE | Open-sourced June 2026; 12M+ proofs and 3M+ inferences verified, per Lagrange | Lagrange press release, grounded |
| On-chain | NOT COVERED | — | Not addressed in this file's sources |

## Known limitations

- Throughput (174 tokens/min for GPT-2, a small model by current
  standards) is far below normal plaintext LLM serving throughput —
  the "faster than prior zkML work" framing is a comparison against
  other zero-knowledge approaches, not against unproven inference.
  Worth keeping in mind when judging practicality.
- The proving-speedup framing differs between DeepProve's own paper
  abstract (20–60×) and Lagrange's press release (60× / 671×) — not
  reconciled in this pass; don't cite a single number without checking
  which baseline it refers to.
- The trust model was an open follow-up until 2026-11-13. It is now
  resolved from the repo itself: DeepProve commits with HyperKZG
  (KZG-based, so it needs a structured reference string), not with a
  transparent scheme. Two caveats remain. In the open-source code the
  SRS is generated locally by `test_setup` from an RNG, so whoever
  generates the proving context could know the trapdoor. Whether
  Lagrange's hosted prover network uses a ceremony SRS instead is not
  documented in the sources read. The repo's own docs page on
  commitments still says Basefold (transparent). It predates the
  README and the code, which both say HyperKZG, but don't cite the docs
  page for the setup question.
