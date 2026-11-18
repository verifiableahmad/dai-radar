---
name: Giza / Orion
field: ZKML / verifiable inference (STARK-based)
category: verifiable-inference
foundation: foundations/zkml-stark.md
last_updated: 2026-10-20
confidence: grounded (what it was built on, historically); status itself is grounded as largely inactive
website: "https://github.com/gizatechxyz/orion"
---

## What it does

Giza originally built **Orion**, a framework for compiling ML models (via
ONNX) into Cairo programs so that ML inference could be proven correct
using zk-STARKs and verified on-chain on StarkNet, the STARK-based
counterpart to EZKL's Halo2/PLONK approach. The stack: ONNX model →
Giza Transpiler → Cairo program (Sierra/CASM) → STARK proof → on-chain
verification via SHARP (Starknet's shared prover).

**This product no longer exists in that form.** Giza pivoted away from
ZKML entirely toward on-chain DeFi agents (ARMA, Pulse), and by 2026
those were wound down too. See "Known limitations" / current status
below before treating this as an active reference implementation.

## Architecture summary

- Original stack: ONNX → Cairo 1.0 (via the Orion ONNX-in-Cairo runtime)
  → STARK proof generated via Cairo's execution trace → SHARP
  (SHARed Prover) aggregates proofs for verification on Ethereum.
- Orion itself was archived 2025-03-03
  ([gizatechxyz/orion](https://github.com/gizatechxyz/orion)).
- Giza pivoted to verifiable DeFi agents (ARMA, Pulse) without the ZKML
  verification layer attached; the agents ran, but proof-of-correct-
  inference was not part of that product.
- A next-generation ML proving stack, **LuminAIR**, built on StarkWare's
  newer **S-two (stwo)** prover (Circle STARKs) was announced as the
  long-term differentiator but, per independent review, its repository
  "had no commits in over eight months" as of the review and "the
  on-chain Cairo verifier is still roadmap"
  ([ownyourmind.ai/projects/giza](https://ownyourmind.ai/projects/giza/)).

## Built on (foundational lineage)

**Two-hop, weaker grounding than the EZKL→Halo2→PLONK chain**: the
analogous chain here is Giza/Orion → Cairo → the original STARK paper,
but the second hop could not be directly confirmed:

1. Orion compiled models to **Cairo**, StarkWare's STARK-friendly CPU
   architecture; this is explicit in Orion's own announcement and docs.
2. Cairo's own paper, *"Cairo: a Turing-complete STARK-friendly CPU
   architecture"* (Goldberg, Papini, Riabzev, eprint 2021/1063), is the
   natural second hop to the original STARK paper (Ben-Sasson, Bentov,
   Horesh, Riabzev, *"Scalable, transparent, and post-quantum secure
   computational integrity,"* eprint 2018/046). **This citation could
   not be confirmed in-text**: the Cairo paper's PDF returned a 403 on
   fetch, and the visible abstract/metadata did not explicitly cite the
   2018/046 paper. The association is strong by authorship/org (Cairo's
   authors are StarkWare; Ben-Sasson, StarkWare's co-founder, co-authored
   the original STARK paper) but this is **inferred, not grounded**,
   flagged exactly because it didn't survive the same direct-quote check
   used elsewhere in this repo (e.g. the EZKL entry's Halo2 citation).
3. Starknet's own SHARP documentation
   ([docs.starknet.io/learn/protocol/sharp](https://docs.starknet.io/learn/protocol/sharp))
   cites no academic papers at all, only blog posts and third-party
   write-ups (Nethermind, zkSecurity, LambdaClass).

See `foundations/zkml-stark.md`, created as a separate file from
`foundations/zkml.md` because this is a STARK lineage, not a PLONK one;
forcing them into one file would misrepresent two different proof
systems as one lineage.

## Property matrix

| Property | Value | Source |
|---|---|---|
| Proving system (original Orion) | Cairo execution → STARK proof via SHARP | Orion docs/announcement, grounded |
| Proving system (next-gen, unreleased) | LuminAIR on S-two (stwo) prover, Circle STARKs | [ownyourmind.ai review](https://ownyourmind.ai/projects/giza/), grounded |
| Trust model | Transparent setup (STARK, no trusted ceremony); a structural advantage over EZKL's KZG trusted setup | Standard STARK property, inferred from Cairo's design, not Giza-specific confirmation |
| On-chain verification | Yes, originally, via SHARP on Ethereum | Starknet SHARP docs |
| **Maturity** | **Largely inactive as of Oct 2026**: Orion archived 2025-03-03; ARMA/Pulse DeFi agents wound down Feb–Mar 2026 (wind-down announced 2026-02-26, migration deadline 2026-03-26); GitHub orgs show no pushes since 2026-03-23; LuminAIR unreleased, no commits in 8+ months per review | [ownyourmind.ai](https://ownyourmind.ai/projects/giza/), corroborated independently via WebSearch (GitHub push dates, on-chain balance drop from ~$4.85M to ~$14K by 2026-06-24) |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | PROOF | Original Orion: STARK proof of Cairo execution, aggregated by SHARP | Orion docs/announcement, grounded |
| Trust rests on | MATH | Transparent setup (STARK, no trusted ceremony) | Standard STARK property, inferred from Cairo's design |
| Confidentiality | NOT COVERED |  | Not addressed in this file's sources |
| Lineage | EXTERNAL PAPER | Orion compiles to Cairo; Cairo's link to the original STARK paper (eprint 2018/046) couldn't be confirmed in-text | Orion docs, grounded; Cairo to STARK hop inferred |
| Maturity | INACTIVE | Orion archived 2025-03-03; successor DeFi agents wound down Feb-Mar 2026; LuminAIR unreleased | ownyourmind.ai review, corroborated via GitHub push dates and on-chain balances, grounded |
| On-chain | EXISTING CHAINS | Originally verified on Ethereum via SHARP, Starknet's shared prover | Starknet SHARP docs, grounded |

## Known limitations

- **This is the clearest "status" flag in the repo so far**: unlike
  EZKL, Gensyn, Zama, Nillion, and Bittensor (all actively developed),
  Giza/Orion's original ZKML product is archived and its successor
  product line (ARMA/Pulse) has also been wound down. The announced
  next-gen ML-proving stack (LuminAIR) is unreleased and appears
  stalled. Treat this entry as historical/architectural reference, not
  as a currently-deployable system.
- Marketing materials (e.g. "$19M assets under agent") diverge sharply
  from on-chain measurement (~$12,800 in agent positions as of
  2026-10-04 per the independent review), a useful general reminder to
  check on-chain data over marketing figures for any system in this
  space, not just this one.
- The STARK-lineage citation (Cairo paper → original STARK paper) is
  inferred, not confirmed. Worth a recheck if someone can access the
  full Cairo paper text directly.
