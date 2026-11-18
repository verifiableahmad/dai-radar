---
name: OpenLedger
field: AI data attribution and payments
category: ownership
foundation:
last_updated: 2026-10-27
confidence: inferred (company announcements only)
website: "https://openledger.xyz"
---

# OpenLedger

## What it does

A blockchain for AI data attribution: users upload data to shared
"datanets"; developers train models on them; a "Proof of Attribution"
system records lineage of datasets, models and agents on-chain so outputs
trace to contributors and payouts route through smart contracts.

## Architecture summary

- OPEN mainnet launched (The Block article dated 2025-11-18); $8M seed
  led by Polychain Capital and Borderless Capital (July 2024 report).
  The article does not state the chain architecture.
- "Payable AI": datanets collect datasets; payments route through smart
  contracts per on-chain attribution trails.
- 2026 roadmap describes a nine-layer platform (company-paid announcement,
  limited independent validation).
- Joint licensed-training standard with Story Protocol (2026-01-29); see
  `/systems/story-protocol.md`.

## Built on (foundational lineage)

None found. Foundation intentionally empty; how "Proof of Attribution"
works technically was not found in the sources read.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Attribution | On-chain "Proof of Attribution" (mechanism unverified) | The Block, announcements |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | OPEN QUESTION | "Proof of Attribution" records lineage on-chain; how it works technically wasn't found | Company announcements, inferred |
| Trust rests on | NOT COVERED |  | Not addressed in this file's sources |
| Confidentiality | NOT COVERED |  | Not addressed in this file's sources |
| Lineage | NONE FOUND |  | File's lineage note, inferred |
| Maturity | LIVE | OPEN mainnet launched, per The Block | The Block, inferred |
| On-chain | OWN CHAIN | A blockchain for AI data attribution | Company announcements, inferred |

## Known limitations

- No independent audit or adoption data found.
- Attribution of a model output to specific training data is an open ML
  problem; claims need the mechanism checked before this file's
  confidence is raised.
