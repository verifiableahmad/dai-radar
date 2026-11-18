---
name: Olas (formerly Autonolas)
field: Decentralized AI agent / marketplace protocols
category: agents
last_updated: 2026-10-20
confidence: grounded (architecture); no formal academic lineage found (see below)
website: "https://olas.network"
---

# Olas (formerly Autonolas)

## What it does

Olas is an open-source stack plus on-chain protocol for building, running,
and monetizing "agent services": groups of independent off-chain programs
(agents) that coordinate to perform a task (e.g. DAO treasury management,
custom oracles, keeper bots). The blockchain layer registers and secures
these services and provides the economic incentive for developers to build
and compose reusable agent components.

## Architecture summary

Per Olas's own developer docs and whitepaper, the stack has four parts:

- **Multi-Agent Systems (MAS) architecture**: an agent service is composed
  of independently-developed code components combined via software
  composition.
- **State-Minimized Consensus Gadgets**: a lightweight consensus mechanism
  so that a group of agent instances running the same service can agree on
  shared state without requiring full blockchain-level consensus for every
  interaction.
- **Agent Communication Network (ACN)**: a distributed hash table (DHT)
  that maps an agent's wallet address to its current network location, so
  agents can find and message each other without a central directory.
  ([stack.olas.network/open-aea/acn](https://stack.olas.network/open-aea/acn/))
- **On-chain protocol**: smart contracts that register agents, agent
  "blueprints," and components as NFTs, and handle the incentive/reward
  mechanism for composing them.
  ([olas.network whitepaper](https://olas.network/documents/whitepaper/Whitepaper%20v1.0.pdf))

## Built on (foundational lineage)

**No formal academic foundational paper found**, flagged explicitly rather
than forced, following this repo's grounding rule.

- The ACN's own documentation describes it as a DHT with "consistent
  hashing" and "structured overlays" but does **not** cite a specific
  academic DHT paper (e.g. Kademlia / Maymounkov & Mazières) by name,
  confirmed by direct inspection of the ACN docs page.
- The whitepaper's own citations, per visible excerpts, are economic/
  composability essays rather than peer-reviewed CS or cryptography papers:
  a Vitalik Buterin article, Carlota Perez's work on technological
  revolutions, Tim Wu, a Placeholder VC investment thesis, and Jesse
  Walden's essay "4 eras of blockchain computing: degrees of
  composability." These ground Olas's *economic* thesis (why composable,
  incentivized software components matter), not its *technical* mechanism.
  The full PDF could not be parsed for a complete references list in this
  pass (binary/compressed text layer, no local PDF-text tool available);
  this is based on visible search-result excerpts, not a full read: treat
  as partial evidence, not a complete citation audit.

This is the same pattern seen with Bittensor: real, working architecture,
but the public-facing academic grounding is thin-to-absent for the core
technical mechanism. No `foundations/` file created for this entry, per
the repo's rule to skip one when there's nothing real to cite.

## Property matrix

| Property | Value | Source |
|---|---|---|
| Coordination mechanism | Off-chain multi-agent system + on-chain service registry | olas.network whitepaper, docs.olas.network, grounded |
| Agent discovery | ACN (DHT mapping wallet address → network location) | stack.olas.network/open-aea/acn, grounded |
| Consensus | "State-minimized consensus gadgets" for shared state among agent instances | Olas whitepaper summary (via search excerpt), grounded but not independently verified in full text |
| On-chain registration | Agents/components/blueprints registered as NFTs | olas.network whitepaper, grounded |
| Base chain(s) | Deployed on multiple smart-contract chains (not single-chain) | docs.olas.network, grounded, specific chain list not verified this pass |
| Maturity | Active, cites >9.9M agent-to-agent (A2A) transactions as of a 2026 blog post | olas.network blog ("Olas: The 2021 Origin Story..."), grounded but self-reported, not independently audited |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | CONSENSUS | Several instances of an agent service agree on shared state via consensus gadgets | Olas whitepaper summary, full text not verified; classification inferred |
| Trust rests on | NOT COVERED | The fault threshold of the consensus gadgets isn't stated | Not addressed in this file's sources |
| Confidentiality | NOT COVERED |  | Not addressed in this file's sources |
| Lineage | NONE FOUND | No formal paper: the ACN DHT cites no DHT paper; whitepaper citations are economic essays | ACN docs + whitepaper excerpts, grounded (partial evidence) |
| Maturity | LIVE | Active; more than 9.9M agent-to-agent transactions, self-reported | olas.network blog, self-reported |
| On-chain | EXISTING CHAINS | Multiple smart-contract chains; agents and components registered as NFTs | docs.olas.network, grounded; chain list not verified |

## Known limitations

- No independently verified academic grounding for the ACN's DHT design or
  the consensus-gadget mechanism; these may be sound engineering without
  being novel research, but that distinction isn't established here.
- The whitepaper's full PDF was not fully machine-readable in this research
  pass; the citation list above is based on partial search-result excerpts,
  not a complete read. Worth a follow-up pass with a proper PDF-text tool.
- Self-reported transaction-volume figures (9.9M A2A transactions) were not
  cross-checked against independent on-chain data.
