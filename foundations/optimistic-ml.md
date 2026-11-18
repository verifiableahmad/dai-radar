---
field: Optimistic on-chain AI verification
paper: opML: Optimistic Machine Learning on Blockchain
authors: KD Conway, Cathie So, Xiaohang Yu, Kartin Wong
year: 2024
venue: arXiv 2401.17555
---

# opML: Optimistic Machine Learning on Blockchain

## Why this is the seed for this field

Directly grounded, team's-own-paper case, about as strong as this
exercise's grounding gets. Two of the four authors are ORA's own
founding team: **Kartin Wong** (ORA co-founder, ex-Google/TikTok
infrastructure engineer) and **Cathie So** (ORA's Chief Scientist, ex-
Ethereum Foundation zkML researcher). KD Conway is credited elsewhere as
"the author of opML." This isn't a system citing outside research; it's
the system's own team publishing the paper that defines the mechanism
the system runs. (arXiv: https://arxiv.org/abs/2401.17555)

## Core contribution

Proposes an alternative to zkML for verifying ML inference on-chain:
instead of a cryptographic proof of correct execution (expensive to
generate for large models), accept the result **optimistically** and
only pay the verification cost if someone challenges it. Borrows the
dispute structure from optimistic rollups: a bisection game narrows a
disagreement down to one contested computation step, which then gets
resolved on-chain by a Fraud Proof VM. Security rests on an "AnyTrust"
assumption: as long as at least one honest, economically-incentivized
validator is watching and willing to dispute a bad result within the
challenge window, incorrect inference gets caught. This trades zkML's
stronger cryptographic guarantee for much lower cost and the ability to
run genuinely large models (the paper demonstrates 7B-parameter LLaMA)
without circuit-compilation overhead.

## Link to tracked system

See [systems/ora-protocol.md](../systems/ora-protocol.md). ORA's
On-Chain AI Oracle (OAO) is opML's reference production deployment: ORA
nodes (running the "Tora" client) execute inference off-chain and post
results on-chain; disputes resolve through the exact bisection +
Fraud-Proof-VM mechanism this paper defines. A related ORA-authored
paper, "opp/ai: Optimistic Privacy-Preserving AI on Blockchain" (Cathie
So et al., arXiv 2402.15006), extends the same optimistic-dispute idea
to add privacy; worth a look if a future watch run tracks privacy-
preserving variants of this lineage specifically.
