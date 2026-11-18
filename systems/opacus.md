---
name: Opacus
field: Differential privacy for ML training
category: private-compute
foundation: foundations/differential-privacy.md
last_updated: 2026-10-27
confidence: grounded (README); inferred (foundation lineage)
website: "https://github.com/meta-pytorch/opacus"
---

# Opacus

## What it does

A library for training PyTorch models with differential privacy,
requiring minimal code changes (`make_private()` with `noise_multiplier`
and `max_grad_norm`, which are DP-SGD parameters).

## Architecture summary

- Cites its technical report (Yousefpour et al., arXiv:2109.12298), Fast
  Gradient Clipping (2009.03106) and Ghost Clipping (2110.05679, added
  Aug 2024), and LoRA support (Dec 2024 tutorial).
- Under the `meta-pytorch` GitHub org; Apache-2.0; about 2k stars.

## Built on (foundational lineage)

The README does not cite Abadi et al. Link to
`foundations/differential-privacy.md` is **inferred** from the DP-SGD
parameters.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Guarantee | (epsilon, delta)-DP via DP-SGD | README parameters |
| Overhead | "little impact" (project's own claim) | README |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | NOT COVERED |  | Not addressed in this file's sources |
| Trust rests on | MATH | DP's guarantee is mathematical, given the noise and clipping parameters chosen | README parameters; classification inferred |
| Confidentiality | DIFFERENTIAL PRIVACY | (epsilon, delta)-DP via DP-SGD; limits what the trained model reveals, not data during training | README, grounded |
| Lineage | OWN PAPER | Cites its own technical report (arXiv 2109.12298); the link to Abadi et al.'s DP-SGD is inferred | README, grounded; foundation link inferred |
| Maturity | LIVE | Library under meta-pytorch, Apache-2.0; latest dated README updates are from 2024 | README, grounded |
| On-chain | NO CHAIN |  | Inferred from absence in the cited sources |

## Known limitations

- DP protects what the trained model reveals, not data during training.
- Most recent dated README updates are from 2024; maintenance cadence
  was not checked beyond that.
