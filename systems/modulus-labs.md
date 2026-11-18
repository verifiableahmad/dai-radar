---
name: Modulus Labs
field: ZKML / verifiable inference
category: verifiable-inference
foundation: foundations/zkml.md
last_updated: 2026-10-20
confidence: grounded (proving-system lineage, org status); inferred (Remainder's internal architecture)
website: "https://github.com/Modulus-Labs/halo2_ml"
---

# Modulus Labs

## What it does

Modulus Labs built "proof of AI inference" systems: ZK proofs that a specific
model, with specific weights, produced a specific output on specific input,
so AI can act on-chain (trading bots, games, recommendation systems) without
requiring anyone to trust the model operator or re-run the computation to
check it.

**Status: the company no longer operates independently.** The team joined
Tools for Humanity (the organization behind Worldcoin/World) in late 2024.
Modulus Labs itself is now a historical reference rather than an active
vendor, though its open-source tooling, particularly the Remainder prover,
continued to be used/cited after that move.
([hoge.gg: "The zk-ML Toolchain in 2026"](https://hoge.gg/zkml-toolchain-2026-which-proving-system-ships/))

## Architecture summary

- **halo2_ml**: Modulus's own GitHub repo, described by Modulus as a
  "Foundation for speedy Machine Learning operations in Halo2 Prover."
  ([github.com/Modulus-Labs/halo2_ml](https://github.com/Modulus-Labs/halo2_ml))
  This is the same backend lineage as EZKL (see Built on, below).
- **Remainder**: Modulus's custom prover, described in third-party coverage
  as "a Halo2 variant tuned for ML" ([hoge.gg](https://hoge.gg/zkml-toolchain-2026-which-proving-system-ships/)).
  Introduced in their own paper "Scaling Intelligence: Verifiable Decision
  Forest Inference with Remainder" (Feb 2024), specifically targeting
  efficient verification of decision-forest/tree-ensemble models rather than
  neural nets. **Flagged as inferred, not fully grounded**: exactly how
  Remainder's internals relate to vanilla Halo2 (same arithmetization? a
  distinct backend reusing only Halo2's commitment layer?) was not confirmed
  from primary sources in this pass; recheck the actual paper
  (`github.com/Modulus-Labs/Papers/remainder-paper.pdf`) before relying on
  implementation-level claims.
- Demos (proof-of-concept, not production infra): **RockyBot**: described as
  the first fully on-chain AI deployment to mainnet, proving that an RNN/LSTM
  trading bot's decisions matched its model's predictions via zk-SNARKs;
  **Leela vs the World**: on-chain AI chess; demonstrated proofs of GPT-2-
  and Twitter-recommendation-algorithm-scale inference.

## Built on (foundational lineage)

Same two-hop chain as EZKL: Modulus names **Halo2** directly (via their own
`halo2_ml` repo description) as their proving backend, and Halo2's own design
book cites **PLONK** (eprint.iacr.org/2019/953) as the source of its
arithmetization. See [foundations/zkml.md](../foundations/zkml.md) for the
paper itself; no new foundations file needed here since the lineage is the
same, not a separate one. The genuinely novel piece is Modulus's *own*
research output, not something they're built on:

- **"The Cost of Intelligence: Proving Machine Learning Inference with
  Zero-Knowledge"** (Daniel Shorr et al., Jan 2023, Ethereum Foundation grant):
  the first benchmark of ZK proof systems (Groth16, Gemini, Winterfell,
  Halo2, Plonky2, zkCNN) against a common suite of ML inference workloads.
  Headline result: an 18M-parameter MLP proved in ~50s using Plonky2.
  ([github.com/Modulus-Labs/Papers](https://github.com/Modulus-Labs/Papers))
- **"Scaling Intelligence: Verifiable Decision Forest Inference with
  Remainder"** (Feb 2024): introduces the Remainder technique above.

## Property matrix

| Property | Value | Source |
|---|---|---|
| Proving system (neural nets) | Halo2-based (`halo2_ml`) | Modulus's own GitHub repo description, grounded |
| Proving system (decision forests) | Remainder (Halo2 variant per third-party source) | hoge.gg, cross-check against primary paper recommended |
| Own research contribution | First cross-system ZK-proving benchmark for ML ("Cost of Intelligence") | Modulus-Labs/Papers repo, grounded |
| Demonstrated scale | 18M-parameter MLP (2023 benchmark); later demos at GPT-2/recommendation-algorithm scale | Cost of Intelligence paper; secondary coverage for later demos, not independently verified |
| Organizational status | Team absorbed into Tools for Humanity (World) in late 2024; not operating as an independent vendor | hoge.gg, 2026 retrospective coverage |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | PROOF | Zero-knowledge proofs of inference: halo2_ml for neural nets, Remainder for decision forests | Modulus GitHub repo description, grounded; Remainder details secondary |
| Trust rests on | MATH | Cryptographic soundness; commitment scheme and setup not documented in this file | Inferred from the proving system, see Property matrix |
| Confidentiality | NOT COVERED |  | Not addressed in this file's sources |
| Lineage | EXTERNAL PAPER | Same chain as EZKL: halo2_ml names Halo2; Halo2 cites PLONK | Modulus GitHub + Halo2 design book, grounded |
| Maturity | INACTIVE | Team joined Tools for Humanity (World) in late 2024; repos not assumed maintained | hoge.gg 2026 retrospective coverage |
| On-chain | EXISTING CHAINS | Demos ran on existing chains (RockyBot trading bot, on-chain chess); no chain of its own | Architecture summary demo descriptions, inferred |

## Known limitations

- The company's defunct status means none of the repos above should be
  assumed actively maintained; check commit history directly before relying
  on any of this for current tooling decisions.
- Remainder's precise relationship to vanilla Halo2 was not confirmed from
  the primary paper in this pass; treat the "Halo2 variant" characterization
  as secondary-sourced, not primary-confirmed.
- The large-scale demos (GPT-2, Twitter recommendation algorithm) are
  demonstrations, not production-maintained systems; no evidence found of
  ongoing operation of either.
