---
name: ORA Protocol
field: Optimistic on-chain AI verification
category: verifiable-inference
foundation: foundations/optimistic-ml.md
last_updated: 2026-10-20
confidence: grounded
website: "https://docs.ora.io"
---

# ORA Protocol

## What it does

ORA runs a verifiable AI oracle: large ML models (LLaMA, Stable
Diffusion, DeepSeek, Mistral, Qwen) execute off-chain, and smart
contracts on Ethereum and several L2s can trust the result without
re-running the model themselves, through an optimistic dispute
mechanism rather than a cryptographic proof. Its flagship product is
the On-Chain AI Oracle (OAO).

## Architecture summary

- **opML (Optimistic Machine Learning)**: ORA's own mechanism (see
  foundations entry) for getting ML inference results trusted on-chain
  without the cost of zkML's proof generation. Results are accepted
  optimistically; a challenge window lets any validator dispute a
  result, triggering a bisection game that narrows the disagreement to
  one computation step, resolved on-chain by a **Fraud Proof VM
  (FPVM)**.
- **System components**: the opML contract (handles fraud proofs and
  challenges), the AIOracle contract (connects opML nodes to on-chain
  callers), user contracts (initiate inference requests), and ORA Nodes
  running the **Tora client** (execute inference off-chain, submit or
  validate results).
- Trust model is "AnyTrust": security holds as long as at least one
  honest, economically incentivized validator is watching and willing
  to dispute within the challenge window: not a cryptographic
  guarantee, an economic/game-theoretic one.
- Founded Sept. 2022 as "Hyper Oracle"; chief scientist Cathie So is an
  ex-Ethereum Foundation zkML researcher, which is also why ORA's docs
  explicitly position opML *against* zkML (cost/scale trade-off) rather
  than treating them as unrelated.

## Built on (foundational lineage)

See [foundations/optimistic-ml.md](../foundations/optimistic-ml.md):
the opML paper (arXiv 2401.17555) is co-authored by ORA's own
co-founder and chief scientist, making this a direct, team's-own-paper
grounding, not an inferred lineage.

## Property matrix

| Property | Value | Source |
|---|---|---|
| Verification mechanism | Optimistic acceptance + bisection-game fraud proof (Fraud Proof VM) | opML paper; ORA docs via search, grounded |
| Trust model | AnyTrust: one honest, incentivized validator sufficient | ORA docs summary (iq.wiki), grounded |
| Cryptographic guarantee | None: economic/game-theoretic, not a proof of correctness | opML paper, grounded |
| Deployed chains | Ethereum mainnet (launched ~Mar 2024), Optimism, Base, Arbitrum (as of 2026 reporting) | Blockworks/Bitget launch coverage + 2026 search aggregation; chain list for 2026 not independently verified against ORA's own current docs this pass |
| Supported models (reported) | LLaMA/LLaMA2/LLaMA3, Stable Diffusion, DeepSeek, Mistral, Qwen, Grok (via integration) | Search aggregation, not cross-checked against ORA's current docs this pass |
| Maturity | Live on multiple mainnets since 2024; ORA's own docs site (docs.ora.io) could not be reached directly during this research pass (DNS failure); findings here rely on search aggregation and third-party summaries (iq.wiki, Blockworks, Bitget), not a direct docs read | Flagged explicitly; recheck against docs.ora.io directly in a future pass |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | DISPUTE GAME | Results accepted optimistically; a challenge triggers a bisection game resolved on-chain by a Fraud Proof VM | opML paper; ORA docs via search, grounded |
| Trust rests on | ONE HONEST PARTY | AnyTrust: one honest, incentivized validator watching within the challenge window | ORA docs summary (iq.wiki), grounded |
| Confidentiality | NOT COVERED |  | Not addressed in this file's sources |
| Lineage | OWN PAPER | opML paper (arXiv 2401.17555), co-authored by ORA's co-founder and chief scientist | foundations/optimistic-ml.md, grounded |
| Maturity | LIVE | Live on multiple mainnets since 2024; docs.ora.io unreachable this pass | Third-party summaries only (iq.wiki, Blockworks, Bitget), inferred |
| On-chain | EXISTING CHAINS | Ethereum mainnet, plus Optimism, Base and Arbitrum per 2026 reporting | Launch coverage + 2026 search aggregation, inferred |

## Known limitations

- **Verification pass for this entry relied on secondary sources.**
  `docs.ora.io` did not resolve during this research session (DNS
  lookup failure on every attempt); every claim above that cites "ORA
  docs" is actually sourced through iq.wiki's summary of those docs or
  other third-party coverage, not a direct fetch. Worth a follow-up
  pass once docs.ora.io is reachable, specifically to confirm the
  current chain-deployment list and challenge-window duration.
- Optimistic/fraud-proof security is weaker than zkML's cryptographic
  guarantee by design: correctness depends on an economically
  rational, watching validator actually showing up within the dispute
  window. A result can finalize as "trusted" during any period with no
  active challenger, even if wrong.
- Challenge-window length (how long a result sits disputable before
  finalizing) was not found/confirmed in this pass; this matters for
  latency-sensitive use cases and wasn't independently verified.
