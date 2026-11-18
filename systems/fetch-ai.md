---
name: Fetch.ai (uAgents)
field: Decentralized AI agent / marketplace protocols
category: agents
last_updated: 2026-10-20
confidence: grounded (architecture description, team-authored); inferred (some matrix cells)
website: "https://github.com/fetchai/uAgents"
---

## What it does

Fetch.ai provides a framework (uAgents, Python) and blockchain infrastructure
for building autonomous software agents that can discover each other,
communicate, and transact. Agents register on-chain for discoverability and
settle value exchange via Fetch.ai's own token (FET).

**ASI Alliance context:** Fetch.ai merged with SingularityNET and Ocean
Protocol in 2024 to form the Artificial Superintelligence Alliance (ASI),
unifying their tokens (FET/AGIX/OCEAN → ASI) under one brand. CUDOS
(distributed GPU compute) joined in Oct 2024. Ocean Protocol subsequently
**withdrew from the alliance in Oct 2025**, with an associated lawsuit
settled (286M FET returned); this entry focuses on Fetch.ai's own
technical stack, not the alliance's business structure, which is actively
in flux. ([0xgreythorn Medium overview](https://0xgreythorn.medium.com/fetch-ai-the-asi-alliance-decentralized-ai-powerhouse-b39c40ec4a56))

## Architecture summary

- **uAgents**: a Python library for building event-driven agents; each
  agent has a cryptographic identity (address derived from a keypair, not
  an IP), listens for messages, and responds via decorated handlers. State
  persists via a key-value store; agent-to-agent messaging uses a typed
  "Agent Chat Protocol." ([github.com/fetchai/uAgents](https://github.com/fetchai/uAgents))
- **Almanac**: a smart contract on Fetch.ai's own chain where agents
  register for discoverability; paired with a Fetch Name Service (FNS) for
  human-readable discovery and the Fetch Ledger for record-keeping.
- **Base chain**: an independent Cosmos-SDK chain using CometBFT (formerly
  Tendermint) consensus, with IBC interoperability to other Cosmos chains,
  not an Ethereum L2/L1.
- **Agentverse / ASI:One**: a hosted platform and LLM-orchestration layer
  that translates natural-language goals into multi-agent workflows on top
  of the uAgents substrate.

## Built on (foundational lineage)

**Primary technical source:** *"Fetch.ai: An Architecture for Modern
Multi-Agent Systems"* (arXiv:2510.18699); authors Michael J. Wooldridge,
Attila Bagoly, Jonathan J. Ward, Emanuele La Malfa, Gabriel Paludo Licks.
This is a team-authored architecture paper (the submitting/corresponding
author is affiliated with Fetch.ai), not an external citation the product
docs point to, the same pattern as Gensyn's and Nillion's team-authored
protocol papers being treated as the grounding source. Notably, co-author
Michael Wooldridge is himself one of the foundational multi-agent-systems
theorists (co-author of the field's standard textbook, *An Introduction to
MultiAgent Systems*), which is a stronger-than-usual signal for this kind
of self-published grounding.

**Honest gap:** the paper's abstract states it bridges "decades of
foundational multi-agent systems (MAS) research," but does not point to
one specific external seed paper the way EZKL → Halo2 → PLONK did, and
Fetch.ai's own uAgents GitHub README cites **no academic papers or
whitepapers at all**, confirmed by direct fetch, not inferred. So unlike
most other entries in this repo, there is no crisp single foundational
citation to point to here beyond the team's own architecture description.
No new `foundations/` file created for this reason; forcing one would
mean inventing a seed paper that isn't actually referenced.

The original 2019 tokenomics/protocol whitepaper (*"FETCH: Technical
Introduction"*) describes the earlier Autonomous Economic Agents (AEA) /
Open Economic Framework design; current production architecture (above)
has superseded parts of that design, similar to how Gensyn's litepaper was
superseded by Verde.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Agent framework | uAgents (Python) | github.com/fetchai/uAgents |
| Identity scheme | Cryptographic keypair-derived address, not IP-based | uAgents README |
| Discovery mechanism | Almanac smart contract + Fetch Name Service | uAgents README |
| Base chain | Independent Cosmos-SDK chain, CometBFT consensus, IBC-interoperable | web search aggregation, not independently verified against chain docs |
| LLM orchestration layer | ASI:One: translates natural-language goals into multi-agent workflows | arXiv:2510.18699 abstract |
| Alliance structure | ASI Alliance (Fetch.ai + SingularityNET; Ocean Protocol withdrew Oct 2025) | 0xgreythorn Medium overview, secondary source, not independently verified |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | NOT COVERED | Agents authenticate by keypair-derived address; nothing described checks what an agent computes | Not addressed in this file's sources |
| Trust rests on | NOT COVERED |  | Not addressed in this file's sources |
| Confidentiality | NOT COVERED |  | Not addressed in this file's sources |
| Lineage | OWN PAPER | Team-authored architecture paper (arXiv 2510.18699); the uAgents README cites no papers | arXiv 2510.18699 + uAgents README, grounded |
| Maturity | NOT COVERED |  | Not addressed in this file's sources |
| On-chain | OWN CHAIN | Cosmos-SDK chain with CometBFT and IBC; agents register in the Almanac contract | Web search aggregation, not independently verified, inferred |

## Known limitations

- No independent academic citation grounds the current architecture beyond
  the team's own paper; treat claims of novelty against prior MAS research
  with the same caution the paper itself invites ("decades of... research"
  is a claim of continuity, not a specific, checkable citation).
- The ASI Alliance's business structure is actively unstable (Ocean
  Protocol's 2025 withdrawal plus a settled lawsuit); any claim about
  "the alliance" as a unified technical entity should be rechecked against
  current status before relying on it.
- Property matrix entries sourced from secondary blog coverage (alliance
  structure, some chain details) rather than primary Fetch.ai documentation
  were not independently cross-verified in this pass.
