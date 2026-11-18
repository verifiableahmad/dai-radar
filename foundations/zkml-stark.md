---
field: ZKML / verifiable inference (STARK-based)
paper: "Scalable, transparent, and post-quantum secure computational integrity"
authors: Eli Ben-Sasson, Iddo Bentov, Yinon Horesh, Michael Riabzev
year: 2018
venue: IACR Cryptology ePrint Archive, 2018/046
link: https://eprint.iacr.org/2018/046
---

## Why this is the seed for this field

**Weaker grounding than this repo's other foundations entries, flagged
explicitly rather than smoothed over.** The chain is:

- Giza/Orion compiled models to **Cairo**, StarkWare's STARK-friendly
  CPU architecture (explicit in Orion's own docs/announcement).
- Cairo's own paper (*"Cairo: a Turing-complete STARK-friendly CPU
  architecture,"* Goldberg/Papini/Riabzev, eprint 2021/1063) is the
  natural next hop to this STARK paper, since Cairo exists specifically
  to make programs STARK-provable.
- **This second-hop citation could not be confirmed in-text** (the
  Cairo paper's PDF was inaccessible; its visible abstract/metadata
  didn't explicitly cite 2018/046). The connection is inferred from
  shared authorship/organization (Ben-Sasson co-founded StarkWare,
  which authored the Cairo paper), not from a direct quote, unlike the
  EZKL→Halo2→PLONK chain in `foundations/zkml.md`, where Halo2's own
  docs explicitly name PLONK.

This is included as this field's seed paper because it's the correct
*technical* lineage (STARKs are what make Cairo/Orion's proofs work at
all), with the caveat that the citation chain is inferred, not directly
sourced, at the Cairo→STARK-paper link specifically.

## Core contribution

Introduced zk-STARKs: a proof system for verifying that a computation
was performed correctly, with two properties that distinguish it from
earlier SNARK constructions (like the PLONK lineage in
`foundations/zkml.md`): **no trusted setup** (the verifier's reference
string is public/transparent, generated without a ceremony that could
be compromised) and **conjectured post-quantum security** (relying on
collision-resistant hash functions rather than elliptic-curve
assumptions that quantum computers could break). The tradeoff is
larger proof sizes than SNARKs like PLONK/KZG-based systems.

## Link to tracked system

Giza/Orion's whole value proposition over EZKL was this tradeoff:
transparent setup instead of EZKL's KZG trusted ceremony, at the cost of
STARK proofs generally being larger than PLONK/KZG proofs. See
`systems/giza-orion.md` for why this is now largely historical: the
product built on this lineage is inactive as of October 2026, which
doesn't change the lineage's technical accuracy but does mean there's
currently no actively-maintained reference implementation of it in this
repo's tracked-systems list.
