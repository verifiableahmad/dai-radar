---
name: Akash Network
field: Decentralized GPU / compute marketplace
category: compute-markets
last_updated: 2026-10-20
confidence: grounded (consensus citation); inferred (AI-specific framing); unverified (2026 migration endpoint)
website: "https://akash.network"
---

# Akash Network

## What it does

Akash is a decentralized marketplace for compute capacity: tenants publish a
deployment manifest describing what they need (CPU, GPU, storage), independent
providers bid competitively to host it, and the lowest qualifying bid wins. It
started as general-purpose cloud compute and has since leaned heavily into
GPU/AI workloads as that demand grew.

## Architecture summary

- Historically an application-specific **Cosmos SDK** chain using **Tendermint**
  Byzantine-fault-tolerant consensus (Proof-of-Stake) to handle escrow, leases,
  and provider payouts on-chain, while the actual compute runs off-chain on
  provider hardware (Docker/Kubernetes-based).
- **Reverse auction** is the core marketplace mechanism: tenants set terms,
  providers compete on price, settlement is in AKT.
- **GPU marketplace** ("Supercloud") shipped via Mainnet 6, August 2023;
  supports H100/A100/A6000/RTX4090/L40S across 150+ providers as of 2026.
  ([hoge.gg](https://hoge.gg/akash-gpu-marketplace-ai-compute/))
- **AkashML**, launched Nov 22 2025, adds a managed AI-inference layer with an
  OpenAI-compatible API that routes inference requests across available GPU
  providers on the network. ([hoge.gg](https://hoge.gg/akash-network-2026-gpu-market-leaves-cosmos/))
- **2026 base-chain migration (unverified endpoint, in progress):** per AEP-79,
  Akash is deprecating its sovereign Cosmos-SDK chain in favor of a
  "shared-security" model (buying BFT security from an established L1 rather
  than running its own validator set), driven by (a) capital inefficiency of
  self-staking and (b) a 2025 Cosmos SDK licensing change (Apache 2.0 →
  Source Available Evaluation License for the Enterprise module) that forced
  the issue. ~15 L1 candidates were evaluated; Solana was reported as the
  leading contender as of this research pass; migration targeted for Q4 2026.
  Not independently confirmed against Akash's own roadmap page beyond the
  secondary source below; reverify before treating "Solana" as settled.
  ([hoge.gg](https://hoge.gg/akash-network-2026-gpu-market-leaves-cosmos/),
  [akash.network/roadmap/aep-79](https://akash.network/roadmap/aep-79/))

## Built on (foundational lineage)

**Grounded, but not AI-specific:** Akash's whitepaper cites the Tendermint BFT
consensus algorithm directly (citation [9] in the original whitepaper). "The
latest gossip on BFT consensus," Buchman, Kwon, Milosevic (2018) is the primary
academic specification underlying Tendermint. This is the one explicit
citation grounding found in Akash's own materials.

This is generic blockchain-consensus grounding, not anything specific to AI/ML
workloads; Akash's GPU marketplace and AkashML layer are a product build on
top of a general-purpose compute marketplace, not something with their own
distinct academic lineage. No foundations/ file created for this entry: per
this repo's rule, a foundations file is for AI/ML-relevant lineage, and
Tendermint's consensus grounding doesn't meet that bar even though it's a real
citation. (If a future watch-run finds Akash publishing something AI-specific
with its own citations, e.g. around AkashML's routing/verification approach,
that would be the point to reconsider.)

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Base chain / consensus (as of 2026, pre-migration) | Cosmos SDK + Tendermint PoS | Akash whitepaper citation [9]; grounded |
| Marketplace mechanism | Reverse auction (price-competitive provider bidding) | Akash docs, grounded |
| GPU support | H100, A100, A6000, RTX 4090, L40S; 150+ providers, 30+ countries | hoge.gg, cross-referenced |
| AI-specific layer | AkashML (OpenAI-compatible inference API, routes across providers), launched 2025-11-22 | hoge.gg, single-source |
| Compute verification | **None found**: no cryptographic or other verification that a provider ran the workload as specified | hoge.gg analysis, single-source; flagged as a known limitation, not cross-confirmed elsewhere in this pass |
| Base-chain migration | In progress per AEP-79: Cosmos SDK → shared-security L1 (Solana reported as leading candidate), targeted Q4 2026 | akash.network/roadmap/aep-79, hoge.gg; endpoint unverified |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | NONE FOUND | "Akash rents you a GPU; it does not prove what that GPU did." | hoge.gg analysis, single-source |
| Trust rests on | OPERATORS | Whichever provider wins the lease; a substituted or quantized model would go undetected | hoge.gg analysis, single-source |
| Confidentiality | NOT COVERED |  | Not addressed in this file's sources |
| Lineage | GENERIC ONLY | Whitepaper cites Tendermint BFT consensus: a real citation, but not AI-specific | Akash whitepaper citation [9], grounded |
| Maturity | LIVE | GPU marketplace since Aug 2023, AkashML since Nov 2025; base-chain migration in progress | Akash docs + hoge.gg, cross-referenced |
| On-chain | OWN CHAIN | Cosmos SDK + Tendermint chain, migrating to a shared-security L1 (Solana reported, unconfirmed) | Akash whitepaper; AEP-79 endpoint unverified |

## Known limitations

- **No compute verification.** Per the source above: "Akash rents you a GPU;
  it does not prove what that GPU did." A provider could in principle
  substitute a cheaper/quantized model for what was requested without
  detection; there is no proof-of-execution or attestation layer analogous to
  Gensyn's RepOps/bisection game or a TEE-attestation approach. This is a
  structural gap, not a bug, and the most important thing for a future
  watch-run to monitor if Akash ever adds a verification layer.
- **Mid-migration as of this writing.** The Cosmos-SDK → shared-security-L1
  transition (AEP-79) is a live, disruptive architecture change with an
  unconfirmed landing spot. Treat any consensus-layer claim in this file as
  provisional until the migration completes and is reverified.
- Economic/tokenomics grounding (AKT emission, staking requirements) comes
  from a 2019 economics whitepaper that predates the GPU marketplace and
  AkashML entirely; not re-verified against current parameters in this pass.
