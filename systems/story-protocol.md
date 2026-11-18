---
name: Story Protocol
field: On-chain IP and training-data licensing
category: ownership
foundation:
last_updated: 2026-10-27
confidence: grounded (core repo README); inferred (what replaces the deprecated contracts)
website: "https://www.story.foundation"
---

# Story Protocol

## What it does

An on-chain registry for IP: ownership, licensing terms, derivative
permissions and economic rights in machine-readable form.

## Architecture summary

- On 2026-01-29 Story and OpenLedger announced a joint standard for
  licensed AI training: AI systems train on licensed IP, prove how it was
  used, enforce license terms at runtime, and distribute royalties
  automatically (see `/systems/openledger.md`).
- Story's role is the canonical IP registry. This is a press release, not
  independent validation.
- **Deprecation (protocol-core-v1 README):** the repo says the contracts
  were deprecated as of 2026-09-19 with no further features, bug fixes
  or security patches, and that the IP Portal and related services shut
  down on 2026-10-15; it advises against new integrations. What replaces
  them was not found. This is the strongest signal in the file and
  postdates the January joint-standard announcement.
- Mechanics (same repo): IP Assets are ERC-721 NFTs registered in an IP
  Asset Registry, each with an IP Account; modules for Licensing, Royalty,
  Dispute, Grouping and Metadata; the Programmable IP License (PIL) is an
  off-chain legal contract with terms mapped on-chain. Deployed natively
  on Story (Aeneid testnet 1315, Homer mainnet 1514). BUSL-1.1 licensed;
  audit reports are in the repo's `audits/` directory (not read).

## Built on (foundational lineage)

None found. Foundation intentionally empty.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Function | IP registry + licensing terms | Joint announcement |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | OPEN QUESTION | Proving how licensed IP was used in training is unsolved; no mechanism described | Joint announcement, inferred |
| Trust rests on | NOT COVERED | Registering IP on-chain doesn't establish the registrant owns it | Not addressed in this file's sources |
| Confidentiality | NOT COVERED |  | Not addressed in this file's sources |
| Lineage | NONE FOUND |  | File's lineage note, inferred |
| Maturity | DEPRECATED | Core contracts deprecated 2026-09-19; IP Portal shut down 2026-10-15; no new integrations advised | protocol-core-v1 README, grounded |
| On-chain | OWN CHAIN | Contracts deployed natively on the Story chain (mainnet chain id 1514) | protocol-core-v1 README, grounded |

## Known limitations

- "Cryptographically proving how IP is used" in training is the hard,
  unsolved part (see `/foundations/zk-proof-of-training.md`: proofs of
  training do not establish what data overlapped); no mechanism was
  described in the sources.
- Registering IP on-chain does not establish that the registrant owns it.
