---
field: ZKML / verifiable inference (sum-check lineage)
paper: "Algebraic methods for interactive proof systems"
authors: Carsten Lund, Lance Fortnow, Howard Karloff, Noam Nisan
year: 1992
venue: Journal of the ACM, 39(4), 859–868
---

## Why this is the seed for this field

Grounded via a two-hop citation chain:

- Lagrange Labs' DeepProve paper (*"DeepProve: Verifiable End-to-End
  Large Language Model Inference,"* IACR ePrint
  [2026/1112](https://eprint.iacr.org/2026/1112)) names the
  **sum-check protocol** explicitly, in its own abstract, as a core
  building block for verifying LLM operators.
- The sum-check protocol originates in this 1992 paper. This is the
  standard attribution across the field: sum-check-based SNARKs
  (GKR, Libra, Virgo, Spartan, HyperPlonk, Lasso/Jolt, and now
  DeepProve) all trace back to it, and one of the original authors
  (Lance Fortnow) has publicly noted it became his most-cited paper
  specifically because of this reuse in zkSNARK literature.

This is a **separate lineage** from `foundations/zkml.md` (PLONK,
eprint 2019/953), which grounds EZKL/Halo2's PLONKish arithmetization.
Both are legitimately "ZKML / verifiable inference" foundations, but
they root in different proof techniques: PLONKish circuit
arithmetization vs. sum-check-based verification of arithmetic
operators directly. Kept as distinct files per this repo's
one-foundation-per-lineage rule, not merged just because the field
label matches.

**Not confirmed in this pass:** whether DeepProve's own bibliography
cites this 1992 paper directly, or an intermediate sum-check-based
SNARK paper (e.g. GKR, Libra, or Virgo) that itself cites it. Treat
this as "the standard lineage for a sum-check-based system," not as a
verified direct citation.

## Core contribution

Introduces the sum-check protocol: an interactive proof technique that
lets a prover convince a verifier that the sum of a multivariate
polynomial's evaluations over a Boolean hypercube equals a claimed
value, in a way that's exponentially cheaper to verify than to
brute-force. This became foundational to a large family of later
succinct arguments (GKR and its descendants) because it gives a cheap,
general-purpose way to verify a computation's structure algebraically,
without needing a trusted setup for the sum-check step itself.

## Link to tracked system

DeepProve uses sum-check (plus lookup arguments) to verify individual
LLM operators (attention, layer norm, grouped-query attention, rotary
positional embeddings) directly, rather than compiling the entire
model into one monolithic arithmetic circuit the way PLONKish systems
(like EZKL/Halo2) do. This operator-level verification approach is
part of why DeepProve claims to avoid the quadratic circuit-size blowup
that limited earlier zkML attempts at whole-model proofs. See
`systems/lagrange-labs.md` for the full system entry and its open
follow-up items.
