---
name: EZKL
field: ZKML / verifiable inference
category: verifiable-inference
foundation: foundations/zkml.md
last_updated: 2026-10-20
confidence: grounded (proving-system lineage); inferred (some matrix cells)
website: "https://github.com/zkonduit/ezkl"
---

## What it does

EZKL (zkonduit/ezkl) compiles ONNX-format ML models (exported from
PyTorch, TensorFlow, sklearn, etc.) into a zero-knowledge circuit, so
that a model inference can be proven correct without revealing the
model weights and/or inputs, and verified cheaply, including on-chain
in the EVM, in a browser, or on-device.

## Architecture summary

- Takes an ONNX graph + sample input, compiles a corresponding proving
  circuit, generates a proof of correct execution, and a matching
  verifier (including Solidity verifier contracts for EVM chains).
- Backend proving system: **Halo2**. EZKL's own README states: "In the
  backend we use the collaboratively-developed
  [Halo2](https://github.com/privacy-scaling-explorations/halo2) as a
  proof system." ([github.com/zkonduit/ezkl](https://github.com/zkonduit/ezkl))
- EZKL uses a modified variant of Halo2 that swaps in **KZG polynomial
  commitments** (rather than the discrete-log/IPA commitment Halo2
  originally shipped with) specifically to get smaller proofs and
  cheaper on-chain verification. This is a deliberate deviation from
  vanilla Halo2, not an assumption. Flagged as inferred-from-general
  sourcing rather than a single explicit EZKL doc quote; worth
  rechecking against current docs.ezkl.xyz if this matters for a
  specific decision.

## Built on (foundational lineage)

**Two-hop grounding, not a direct citation**: EZKL's own docs don't
cite an academic paper directly. The chain is:

1. EZKL's README/docs explicitly name **Halo2**
   ([privacy-scaling-explorations/halo2](https://github.com/privacy-scaling-explorations/halo2))
   as the proof system it uses.
2. Halo2's own design book explicitly states: *"The arithmetization
   used by Halo 2 comes from [PLONK](https://eprint.iacr.org/2019/953),
   or more precisely its extension UltraPLONK that supports custom
   gates and lookup arguments."*
   ([zcash.github.io/halo2/concepts/arithmetization.html](https://zcash.github.io/halo2/concepts/arithmetization.html))

So: EZKL → (names) Halo2 → (cites) PLONK paper. See
`/foundations/zkml.md` for the paper itself. Halo2 does **not**, per
its own arithmetization page, discuss the original "Halo" recursive-
proof-composition paper (Bowe/Grigg/Hopwood 2019); that lineage looked
plausible going in but didn't hold up under direct source-checking, so
it's dropped rather than asserted.

## Property matrix

| Property | Value | Source |
|---|---|---|
| Proving system | Halo2 (modified) | EZKL README, grounded |
| Arithmetization | PLONKish (UltraPLONK: custom gates + lookup arguments) | Halo2 design book, grounded |
| Commitment scheme | KZG (EZKL's fork), not vanilla Halo2's IPA | General sourcing, **inferred**; recheck before relying on this |
| Trust model | Requires a KZG structured reference string (SRS/ptau); EZKL can auto-download or generate one | docs.ezkl.xyz/getting-started/setup, grounded |
| SRS size | Hundreds of MB to several GB depending on circuit size | General sourcing, inferred |
| On-chain verification | Yes, generates Solidity verifier contracts, EVM-compatible | EZKL project description, grounded |
| Maturity | Active, widely used reference implementation in the ZKML space; not formally audited to my knowledge as of this check | Inferred, not independently verified this pass |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | PROOF | Halo2 zero-knowledge proof of the inference; verifiers run in the EVM, a browser, or on-device | EZKL README, grounded |
| Trust rests on | MATH + SETUP | Cryptographic soundness plus a KZG structured reference string (SRS/ptau) from a setup ceremony | docs.ezkl.xyz/getting-started/setup, grounded |
| Confidentiality | CRYPTOGRAPHIC | Zero-knowledge: inference proven correct without revealing model weights and/or inputs | EZKL project description, grounded |
| Lineage | EXTERNAL PAPER | Two hops: EZKL names Halo2; Halo2's design book cites PLONK | EZKL README + Halo2 design book, grounded |
| Maturity | LIVE | Active, widely used reference implementation; audit status not checked | Inferred, not independently verified this pass |
| On-chain | EXISTING CHAINS | No chain of its own; generates Solidity verifier contracts for EVM chains | EZKL project description, grounded |

## Known limitations

- SRS/setup size scales with circuit complexity and can become a real
  deployment cost for larger models.
- Relies on a trusted-setup ceremony for KZG (weaker trust assumption
  than a fully transparent setup); this is a direct consequence of
  choosing KZG over Halo2's original commitment scheme.
- This entry did not independently verify audit status, current proof
  latency numbers, or maximum practical model size; flag these as
  open follow-ups rather than treating the matrix as complete.
