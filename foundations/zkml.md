---
field: ZKML / verifiable inference
paper: "PLONK: Permutations over Lagrange-bases for Oecumenical Noninteractive arguments of Knowledge"
authors: Ariel Gabizon, Zachary J. Williamson, Oana Ciobotaru
year: 2019
venue: IACR Cryptology ePrint Archive, 2019/953
link: https://eprint.iacr.org/2019/953
---

## Why this is the seed for this field

Grounded via a two-hop citation chain, not a direct EZKL citation:

- EZKL's own README names **Halo2** as its backend proof system.
- Halo2's own design documentation states explicitly: *"The
  arithmetization used by Halo 2 comes from
  [PLONK](https://eprint.iacr.org/2019/953), or more precisely its
  extension UltraPLONK that supports custom gates and lookup
  arguments."*
  ([zcash.github.io/halo2/concepts/arithmetization.html](https://zcash.github.io/halo2/concepts/arithmetization.html))

So the tracked system (EZKL) is built on Halo2, which is explicitly
built on this paper's arithmetization. This is as close to "grounded"
as the chain gets without EZKL citing PLONK by name itself.

## Core contribution

PLONK introduced a practical, universal (not circuit-specific) SNARK
construction: a single structured reference string works for *any*
circuit up to a size bound, instead of needing a fresh trusted setup
per circuit (as earlier SNARKs like the original Groth16/Pinocchio
pipeline required). It does this via a permutation argument over
Lagrange-basis polynomials to enforce wiring constraints between gates,
combined with a polynomial commitment scheme (originally KZG) to make
the whole thing non-interactive and succinct. Follow-on work extended
this base arithmetization with custom gates and lookup arguments
("UltraPLONK" / "PLONKish"), which is the specific extension Halo2
actually implements.

## Link to tracked system

EZKL inherits PLONKish arithmetization transitively through Halo2, then
additionally swaps Halo2's native commitment scheme for KZG commitments
specifically to shrink proof size and verification cost for on-chain
use (see `/systems/ezkl.md`; this specific claim is flagged inferred,
not directly quoted from an EZKL source, so re-verify before treating
it as settled). The universality property of PLONK (one SRS per size
bound, not per circuit) is part of why a general-purpose tool like
EZKL, which has to support arbitrary ONNX graphs rather than one fixed
circuit, is practical at all.
