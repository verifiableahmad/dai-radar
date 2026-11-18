---
name: Ocean Protocol
field: AI data marketplace / provenance
category: ownership
last_updated: 2026-10-20
confidence: grounded (architecture); no academic lineage found for core privacy mechanism
website: "https://oceanprotocol.com"
---

## What it does

Ocean Protocol is a Web3 data marketplace: data owners publish datasets as
NFTs (ERC-721) with access rights sold as datatokens (ERC-20), and buyers
can either download data they've licensed or, via Compute-to-Data (C2D),
run an approved algorithm against a dataset without the dataset ever
leaving the owner's infrastructure, so only the computation's output is
exposed, not the raw data.

## Architecture summary

- **Data NFTs + datatokens**: the data NFT represents the underlying IP;
  datatokens are per-asset ERC-20 access-right tokens. Smart contracts for
  publishing, pricing, and access control run on Ethereum mainnet; Ocean
  is also deployed across Polygon, BNB Smart Chain, Energy Web Chain,
  Optimism, and Moonriver.
  ([docs.oceanprotocol.com/discover/networks](https://docs.oceanprotocol.com/discover/networks))
- **Compute-to-Data (C2D) privacy mechanism**: per Ocean's own
  architecture docs, privacy comes from **process isolation, not
  cryptography**. Each job runs in a dedicated, ephemeral Kubernetes pod
  that loads the dataset and algorithm, executes, publishes the result,
  then deletes the dataset, algorithm, and itself.
  ([docs.oceanprotocol.com/developers/compute-to-data/compute-to-data-architecture](https://docs.oceanprotocol.com/developers/compute-to-data/compute-to-data-architecture))
  Components: **Ocean Provider** (credentials/access control),
  **Operator Service** (HTTP API, workflow orchestration),
  **Operator Engine** (Kubernetes-backed execution).
- **Oasis Sapphire deployment**: Ocean's "Predictoor" product specifically
  is deployed on Oasis Sapphire mainnet, which *is* a confidential
  (TEE-based) EVM. This is a genuinely cryptographic/hardware-confidential
  privacy mechanism, but per Ocean's own docs, this Sapphire deployment
  does not currently support `ocean.js`, `ocean.py`, or Ocean Market, i.e.
  it isn't the mechanism behind the core data-marketplace/C2D product
  described above. Don't conflate the two.

## Built on (foundational lineage)

**No academic paper found that Ocean's own docs cite for the C2D privacy
mechanism itself.** Checked the C2D architecture page and the technical
whitepaper search results directly: the architecture is described
operationally (isolated Kubernetes pods, ephemeral execution) with no
citation to a specific academic paper, cryptographic primitive, or formal
security proof for *why* this gives the privacy property claimed. This is
the Bittensor-style situation: rather than force a citation, this is
flagged as **inferred/not grounded**: C2D is better described as an
engineering pattern (sandboxed compute-near-data) than as an
implementation of a specific cited cryptographic scheme.

No new `foundations/` file created; there's nothing to seed it with that
would survive the same source-checking bar used elsewhere in this repo.
If Ocean's team ever publishes a paper specifically justifying C2D's
security properties, revisit this.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Privacy mechanism | Process/container isolation (ephemeral Kubernetes pods), **not** cryptographic (MPC/FHE/ZK) | Ocean C2D architecture docs, grounded |
| Cryptographic confidential-compute option | Exists only for the separate Predictoor product on Oasis Sapphire (TEE-based); not used by core C2D/Ocean Market | docs.oceanprotocol.com/discover/networks + Oasis partnership announcements, grounded |
| Data representation | Data NFT (ERC-721) + per-asset datatoken (ERC-20) | Ocean docs, grounded |
| Base chain(s) | Ethereum mainnet (core contracts) + Polygon, BNB Smart Chain, Energy Web Chain, Optimism, Moonriver, Oasis Sapphire (Predictoor only) | docs.oceanprotocol.com/discover/networks, grounded |
| Maturity | Actively maintained; ocean-node v1.0.3 (Mar 2026), ocean.js v6.0.0 (Feb 2026) | repo release history via search, inferred-from-aggregation not primary-source-verified this pass |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | NONE FOUND | A buyer has no cryptographic proof the algorithm ran only against the claimed dataset | Ocean C2D architecture docs, grounded |
| Trust rests on | OPERATORS | The data owner's Kubernetes infrastructure, or a provider they trust | Ocean C2D architecture docs, grounded |
| Confidentiality | ISOLATION | Compute-to-Data runs in an ephemeral pod; raw data never leaves the owner. Not cryptographic (TEE only for the separate Predictoor product) | Ocean C2D architecture docs, grounded |
| Lineage | NONE FOUND | No paper cited for the C2D privacy mechanism | Direct check of C2D docs + whitepaper search, grounded (as a negative finding) |
| Maturity | LIVE | Actively maintained; ocean-node v1.0.3 (Mar 2026), ocean.js v6.0.0 (Feb 2026) | Release history via search, inferred-from-aggregation |
| On-chain | EXISTING CHAINS | Ethereum mainnet core contracts, plus Polygon, BNB Smart Chain, Energy Web, Optimism, Moonriver | docs.oceanprotocol.com/discover/networks, grounded |

## Known limitations

- The core privacy claim ("your data never leaves your infrastructure")
  rests on trusting the isolation of a Kubernetes pod operated by the
  data owner's own infrastructure (or a provider they trust), not on a
  cryptographic guarantee independently verifiable by the buyer. A buyer
  has no cryptographic proof that the algorithm actually ran only against
  the claimed dataset, or that the pod wasn't tampered with; this is a
  materially weaker trust model than the MPC/FHE/ZK-based systems
  elsewhere in this repo, and should not be described as "privacy-
  preserving" in the same sense as those.
- The one part of Ocean's stack with real cryptographic/hardware backing
  (Oasis Sapphire's TEE-based confidential EVM) is scoped to a single
  product (Predictoor) and explicitly not integrated with the main
  data-marketplace tooling as of this check; worth re-checking if that
  changes.
- Maturity/release claims above came from secondary aggregation during
  this research pass, not a primary-source changelog read; treat as
  approximate.
