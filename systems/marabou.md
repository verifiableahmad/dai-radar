---
name: Marabou
field: Formal verification of neural networks
category: formal-verification
foundation: foundations/neural-network-verification.md
last_updated: 2026-10-27
confidence: grounded (README, tool description); inferred (foundation link)
website: "https://github.com/NeuralNetworkVerification/Marabou"
---

# Marabou

## What it does

An SMT-based framework for verifying deep neural networks: given a network
and a property it returns a guarantee or a counter-example input. Calls
itself "a research product."

## Architecture summary

- Inputs: ONNX (main), .nnet, TensorFlow; properties via Python or VNNLIB.
- Default solver is single-threaded with DeepPoly bound tightening and
  DeepSoI; also multi-worker and Split-and-Conquer modes; optional Gurobi.
- Version 2.0 tool paper: arXiv 2401.14461. A 2026 tutorial covers a
  proof-producing version (secondary source).
- About 348 stars; Windows support discontinued.

## Built on (foundational lineage)

Marabou's approach (SMT/LP) differs from CROWN's bound propagation, so
the link to `foundations/neural-network-verification.md` is **inferred**
as a peer technique in the same field, not a citation. The README does
not state the license; the COPYING file was not read.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Method | SMT + bound tightening | README |
| Output | Proof or counter-example | README |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | FORMAL PROOF | SMT-based: returns a guarantee that the property holds, or a counter-example input | README, grounded |
| Trust rests on | MATH | Soundness of the SMT/LP reasoning and its implementation; verifier soundness is a research concern | Known limitations; classification inferred |
| Confidentiality | NOT COVERED |  | Not addressed in this file's sources |
| Lineage | OWN PAPER | Marabou 2.0 tool paper (arXiv 2401.14461); the link to CROWN is a peer technique, not a citation | README, grounded; foundation link inferred |
| Maturity | RESEARCH | Calls itself "a research product" | README, grounded |
| On-chain | NO CHAIN |  | Inferred from absence in the cited sources |

## Known limitations

- Complete SMT-style verification scales worse than GPU bound-propagation
  tools on large networks (general, not stated in the README).
- Verifier soundness is itself a research concern (SoundnessBench).
