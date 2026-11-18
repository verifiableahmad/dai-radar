---
name: Sentient (OML)
field: AI model ownership / monetization protocols
category: ownership
foundation: foundations/ai-native-cryptography.md
last_updated: 2026-10-20
confidence: grounded (core paper); inferred/news-sourced (several matrix cells)
---

## What it does

Sentient is a protocol for publishing open-weight AI models while still giving
the model's owners/contributors enforceable ownership, usage tracking, and a
revenue stream, addressing the problem that a fully open model can normally
be copied and monetized by anyone, with no way to trace or compensate the
original builder.

## Architecture summary

- Built around the **OML (Open, Monetizable, Loyal)** framework: a model
  should be inspectable/open (**Open**), trigger compensation to its owners
  on every call (**Monetizable**), and have misuse/unauthorized use be
  detectable (**Loyal**).
- Four-layer protocol stack: **storage** (immutable/available hosting of the
  model artifact), **distribution** (converting the artifact into OML format),
  **access** (authorizing/tracking each query), **incentive** (routing
  collected fees to builders). (Source: openagi.discourse.group summary of
  the Sentient protocol design, cross-confirmed by multiple secondary
  write-ups, not fetched from a single Sentient primary doc in this pass.)
- Production mechanism is reported as the **"Melange" method**, combining
  model fingerprinting, TEE (trusted execution environment) execution, and
  on-chain contract-based revenue distribution. This is a secondary-source
  characterization (search aggregation), not independently confirmed against
  a primary Sentient doc in this pass. Flagged as inferred.
- Chain architecture: designed to be **L1-agnostic / EVM-compatible** rather
  than requiring its own base chain (reported compatible with Ethereum and
  other L1s); no dedicated standalone "Sentient Chain" was found. Sentient's
  own network is referred to as **GRID**.

## Built on (foundational lineage)

**Paper:** "OML: Open, Monetizable, and Loyal AI", Cheng, Contente, Finch,
Golev, Hayase, Miller, Moshrefi, Nasery, Nailwal, Oh, Tyagi, Viswanath.
IACR ePrint 2024/1573 (received 2024-10-05).
[eprint.iacr.org/2024/1573](https://eprint.iacr.org/2024/1573)

**Grounding:** this is about as direct as grounding gets: 5 of the 12 listed
authors (Contente, Finch, Golev, Nailwal, Tyagi) are affiliated with
**Sentient Foundation** itself, alongside academic co-authors from Princeton,
University of Washington, and UIUC. This paper is not third-party prior work
Sentient cites; it is Sentient's own team publishing the protocol's
founding paper. The paper introduces "AI-native cryptography" as its core
contribution: cryptographic techniques designed around the continuous,
low-dimensional-manifold structure of AI model representations, rather than
the binary correct/incorrect security model of traditional cryptography.
Model fingerprinting (OML 1.0) is presented as the first primitive built in
this new framework.

This lineage is **distinct** from every other foundations/ entry in this
repo (PLONK/ZKML, Proof-of-Learning/decentralized-training, TFHE/FHE-ML,
LSSS-Curl/MPC-ML, Shapley/decentralized-consensus); see
`foundations/ai-native-cryptography.md`.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Core mechanism | Model fingerprinting (OML 1.0) embedded via fine-tuning | eprint 2024/1573, grounded |
| Theoretical basis | "AI-native cryptography": exploits continuous/low-dimensional structure of AI representations rather than binary security guarantees | eprint 2024/1573, grounded |
| Protocol layers | Storage, distribution, access, incentive (4-layer stack) | openagi.discourse.group summary, secondary-sourced |
| Production trust mechanism | Reported to combine fingerprinting + TEE execution + on-chain revenue splitting ("Melange" method) | secondary-source aggregation, inferred, not confirmed against a Sentient primary doc this pass |
| Ownership distribution | NFT-based ownership mint used for the Dobby model (reported 650,000-participant mint) | Cointelegraph / Investing.com news coverage, not a primary Sentient doc |
| Base chain | L1-agnostic / EVM-compatible; no dedicated standalone chain found; native network referred to as "GRID" | search aggregation, inferred |
| Released models | Dobby / Dobby Plus (Llama fine-tunes, uncensored-tuned), published on HuggingFace | secondary coverage |
| Maturity | GRID mainnet **not live** as of the most recent coverage found (planned Q2 2026, no committed date confirmed as of Aug 2026); core utility (staking, fee distribution) is gated on this launch | news aggregation, inferred |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | NOT COVERED | OML targets ownership: fingerprints detect unauthorized use of a model, not whether an output was computed correctly | Not addressed in this file's sources |
| Trust rests on | OPEN QUESTION | Production mechanism reportedly combines fingerprinting, TEE execution and on-chain revenue splits; unconfirmed | Secondary-source aggregation, inferred |
| Confidentiality | NOT COVERED | Models are open-weight by design ("Open") | Not addressed in this file's sources |
| Lineage | OWN PAPER | OML paper (ePrint 2024/1573); 5 of 12 authors at Sentient Foundation | eprint 2024/1573, grounded |
| Maturity | PRE-LAUNCH | GRID mainnet not live; no committed date as of Aug 2026; models released on HuggingFace | News aggregation, inferred |
| On-chain | EXISTING CHAINS | L1-agnostic and EVM-compatible; no dedicated chain found | Search aggregation, inferred |

## Known limitations

- The core cryptographic/ML contribution (model fingerprinting via
  "AI-native cryptography") is grounded directly in Sentient's own
  peer-authored paper, which is strong grounding. The *production* mechanism
  (TEE + on-chain revenue split, the "Melange" method) was not verified
  against a primary Sentient source in this pass and should be rechecked.
- Core on-chain utility (staking, fee distribution, task qualification)
  is reported as gated on the GRID mainnet launch, which had no committed
  date as of the most recent coverage found. Treat any claims about live
  revenue-sharing or staking mechanics as aspirational/not-yet-operational
  until the mainnet status is reconfirmed.
- Ownership-via-NFT-mint (the Dobby distribution model) is a reported
  mechanism from news coverage, not verified against Sentient's own
  documentation of the mint's actual mechanics in this pass.
