---
name: Verified zkEVM
field: Formal verification of ZK proving stacks
category: formal-verification
foundation:
last_updated: 2026-10-27
confidence: grounded (site headline facts); inferred (scope detail from secondary sources)
website: "https://verified-zkevm.org"
---

# Verified zkEVM

## What it does

An Ethereum Foundation initiative funding formal verification of zkVMs,
EVM implementations and cryptographic proof systems. Relevant to this
radar because the ZKML systems tracked here (e.g. `/systems/ezkl.md`)
depend on the same class of circuits and proof systems.

## Architecture summary

- Site reports 42 grants awarded across 3 tracks and 90 resources, with a
  Verified-zkEVM GitHub org. It names no amounts, teams or tools.
- Secondary sources report: Least Authority on a Lean blueprint for STIR
  and WHIR; Certora on autoprecompiles (Powdr Labs); Nethermind on a
  Lean model of EVM/Yul and Halo2 circuit verification tooling, and a
  RISC Zero RISC-V circuit effort; a reported ~$20M scope covering
  leanMultisig circuits, leanSig hashes and the leanVM specification.
  The dollar figure is single-source.
- **SP1 (Succinct + Nethermind):** Lean checks that SP1 Hypercube's core
  RV64 AIR constraints, plus lookup-implied constraints, implement
  RISC-V semantics against the Sail spec, covering 62 opcodes. An
  Ethereum Foundation post (2026-05-20) says the work has known
  limitations and that additional gaps were recently found and disclosed
  to Succinct. Next: ALU circuits, then Keccak256 / Secp256k1 precompiles.
- **RISC Zero (Nethermind):** a Feb 2025 proof of concept on the Zirgen
  MLIR layer; only the instruction decoder, an IsZero gadget and a OneHot2
  gadget were fully done. Jolt: nothing found.

## Built on (foundational lineage)

None. Foundation intentionally empty: this is verification *of* proof
stacks, not derived from a single paper. Touches `foundations/zkml.md`
indirectly (Halo2 appears in both).

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Method | Lean (per secondary sources; not on the site page) | Least Authority, Nethermind |
| Status | Ongoing for 1+ year; no fully verified zkVM shown | secondary talk summary |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | FORMAL PROOF | Funds Lean proofs of zkVMs, EVM implementations and proof systems; no fully verified zkVM shown yet | Secondary sources (Least Authority, Nethermind), inferred |
| Trust rests on | NOT COVERED |  | Not addressed in this file's sources |
| Confidentiality | NOT COVERED |  | Not addressed in this file's sources |
| Lineage | NONE FOUND | Verification of proof stacks, not derived from a single paper | verified-zkevm.org, grounded |
| Maturity | RESEARCH | Ongoing for over a year; 42 grants across 3 tracks; work in progress | verified-zkevm.org + secondary talk summary |
| On-chain | NOT COVERED |  | Not addressed in this file's sources |

## Known limitations

- Work in progress; the sources show no finished, fully verified zkEVM.
- Not about ML: the link to ZKML is that shared proof systems get verified.
