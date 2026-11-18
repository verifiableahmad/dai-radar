---
name: Ritual
field: On-chain AI inference / verifiable compute
category: verifiable-inference
last_updated: 2026-10-20
confidence: grounded (consensus/execution architecture); inferred (several matrix cells); no external academic lineage found for the proof-system layer
website: "https://ritual.net"
---

## What it does

Ritual is a decentralized AI infrastructure protocol connecting Ethereum
smart contracts to a network of off-chain AI compute nodes. Developers
submit inference (or other heavy-compute) tasks; the network executes
them off-chain and settles a verifiable result back on-chain.

## Architecture summary

- **Infernet**: the middleware protocol/SDK connecting Ethereum
  contracts to off-chain compute nodes. The Infernet Node is an
  open-source, lightweight client (github.com/ritual-net/infernet-node)
  that executes arbitrary workload containers and reports results
  on-chain. Reported scale: 8,000+ connected nodes (per Ritual's own
  explainer material, not independently verified against chain data
  this pass).
- **Ritual Chain**: a custom L1 built to support heterogeneous
  computation (AI inference, ZK proof verification, TEE execution,
  scheduled transactions) that standard blockchains can't natively
  handle.
- **Symphony**: Ritual's own consensus/execution research (credited to
  Maryam Bahrani, published on ritual.net's blog, ~April 2026),
  describing "Execution-Aware Consensus and Proposer Disaggregation."
  Its core idea is an **Execute-Once-Verify-Many-Times (EOVMT)** model:
  a staked executor runs the expensive/non-deterministic computation
  once and produces a proof; other nodes verify the proof instead of
  re-executing the computation themselves. Deterministic replication
  stays on the critical consensus path; expensive or non-deterministic
  execution is delegated asynchronously, with proof-based settlement.
- **Proof system is deliberately not fixed.** Ritual's own docs
  describe the protocol as proof-system agnostic: settlement can use a
  ZK proof, a TEE attestation, or both, chosen per application based on
  its security/cost/performance needs, not a single scheme the whole
  network commits to.

## Built on (foundational lineage)

**No separate `foundations/` entry created for this system**, explained
here rather than forced:

- The closest thing to a foundational technical paper is **Symphony**
  itself, but it is Ritual's own self-published research (Bahrani,
  ritual.net blog), not an independent external line of academic work
  the way PLONK grounds EZKL/Halo2 or TFHE grounds Zama. Self-grounding
  is documented, not treated as equivalent to external validation.
- Ritual's docs name specific cryptographic primitives: **CKKS**
  (homomorphic encryption), **ECIES**, **EIP-712**, but without pinning
  them to a specific academic citation in what was checked this pass.
  These are standard, well-known schemes; the gap is Ritual's own docs
  not citing a specific paper for them, not that the schemes are
  obscure.
- Because Ritual is explicitly proof-system agnostic (ZK or TEE,
  per-app), there is no single cryptographic lineage analogous to
  "this system is built on paper X" the way the other tracked systems
  are. Forcing a `foundations/` file here would mean picking one
  external paper arbitrarily rather than reflecting how the system
  actually works. If a specific Ritual-deployed application turns out
  to commit to one scheme consistently, that would be the better unit
  to ground, not the protocol as a whole.

## Property matrix

| Property | Value | Source |
|---|---|---|
| Verification mechanism | EOVMT: staked executor computes once, produces a proof (ZK, TEE attestation, or both); validators verify the proof rather than re-executing | Symphony research, Ritual docs |
| Consensus model | Symphony: execution-aware consensus with proposer disaggregation; deterministic work stays on critical path, expensive/non-deterministic work delegated async | ritual.net blog (Bahrani, ~Apr 2026) |
| On-chain integration | Infernet middleware links Ethereum smart contracts to off-chain compute nodes; Ritual also runs its own L1 (Ritual Chain) | docs.ritualfoundation.org, github.com/ritual-net/infernet-node |
| Proof system | Not fixed: ZK proof and/or TEE attestation, chosen per application | docs.ritualfoundation.org, grounded |
| Network scale | 8,000+ connected Infernet nodes (as reported) | Ritual explainer material, community-adjacent source, inferred/unverified count |
| Named crypto primitives | CKKS, ECIES, EIP-712 named in docs | docs.ritualfoundation.org, named but not deeply citation-traced this pass |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | PER APP | Staked executor computes once and produces a ZK proof and/or TEE attestation; validators verify it instead of re-executing | Symphony research, Ritual docs |
| Trust rests on | PER APP | Depends on which scheme each application picks: ZK, TEE, both, or potentially neither | docs.ritualfoundation.org, grounded |
| Confidentiality | NOT COVERED | TEE execution is supported and CKKS is named in the docs, but no confidentiality claim is traced | Not addressed in this file's sources |
| Lineage | OWN PAPER | Symphony is Ritual's own self-published research (ritual.net blog); no external paper | ritual.net blog (Bahrani, ~Apr 2026), grounded |
| Maturity | OPEN QUESTION | 8,000+ Infernet nodes reported; Ritual Chain mainnet status not verified | Ritual explainer material, inferred/unverified |
| On-chain | OWN CHAIN | Ritual Chain (custom L1), plus Infernet linking Ethereum contracts to off-chain nodes | docs.ritualfoundation.org, github.com/ritual-net/infernet-node |

## Known limitations

- "Proof-system agnostic" means the actual security guarantee for any
  given deployed application depends on whether that application
  chose ZK, TEE, both, or (potentially) neither; the protocol-level
  description does not by itself guarantee a specific guarantee for a
  specific app. Check per-application before relying on a claim.
- No external, independently-authored academic paper was found
  grounding the protocol's cryptographic/consensus design; Symphony is
  self-published by the Ritual team. Documented as a confidence gap,
  not papered over.
- Node count (8,000+) and current Ritual Chain mainnet/launch status
  were not independently verified against primary chain data this pass.
