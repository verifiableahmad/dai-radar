---
name: alpha-beta-CROWN
field: Formal verification of neural networks
category: formal-verification
foundation: foundations/neural-network-verification.md
last_updated: 2026-10-27
confidence: grounded (README)
website: "https://github.com/Verified-Intelligence/alpha-beta-CROWN"
---

# alpha-beta-CROWN

## What it does

A GPU neural-network verifier combining linear bound propagation with
branch and bound. Certifies robustness to adversarial perturbations and
other properties (e.g. Lyapunov stability in control); handles large
convolutional networks, ResNets and Transformers.

## Architecture summary

- Components: CROWN, auto_LiRPA, α-CROWN (incomplete), β-CROWN
  (complete for ReLU), GenBaB (general nonlinearities), GCP-CROWN and
  BICCOS (cutting planes), BaB-Attack (falsifier), INVPROP.
- Led by Prof. Huan Zhang (UIUC) under Verified-Intelligence; BSD
  3-Clause.
- Reports winning VNN-COMP every year 2021-2026 (self-reported).

## Built on (foundational lineage)

Grounded: the README asks users to cite CROWN (2018), auto_LiRPA,
α-CROWN, β-CROWN, then later extensions. See
`foundations/neural-network-verification.md`.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Soundness | Sound bounds; complete for ReLU via β-CROWN | README |
| Hardware | GPU | README |
| Competition | VNN-COMP wins 2021-2026 (self-reported) | README |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | FORMAL PROOF | Sound bounds certify robustness and other properties on specified input regions; complete for ReLU via beta-CROWN | README, grounded |
| Trust rests on | MATH | Soundness of the bound computations and of the verifier's implementation; SoundnessBench exists to test it | Known limitations; classification inferred |
| Confidentiality | NOT COVERED |  | Not addressed in this file's sources |
| Lineage | OWN PAPER | CROWN (2018) and its successors, by the group that leads the project (Huan Zhang) | README citation list, grounded |
| Maturity | LIVE | Maintained open-source verifier; VNN-COMP wins 2021-2026 (self-reported) | README, self-reported |
| On-chain | NO CHAIN |  | Inferred from absence in the cited sources |

## Known limitations

- Verifies properties of a model on specified input regions, not general
  "safety"; scales to large networks but not to frontier LLMs (not claimed
  by the README).
- Verifier bugs are a real risk: a benchmark (SoundnessBench,
  arXiv 2412.03154) exists to test whether verifiers, including this one,
  return sound answers.
