---
name: OpenMined (PySyft)
field: Remote data science / private data access
category: private-compute
foundation:
last_updated: 2026-10-27
confidence: grounded (README); older framing in some sources is outdated
website: "https://github.com/OpenMined/PySyft"
---

# OpenMined (PySyft)

## What it does

An open-source Python library for "data science on data that remains in
someone else's server": the data owner keeps data in place and approves
what leaves.

## Architecture summary

- Current line is `syft` 0.10+ (README: successor to `syft-client`;
  legacy <=0.9 users pin `syft<0.10`).
- Data scientists submit Python jobs; owners approve; jobs run in a
  sandboxed virtual environment and only approved outputs are returned.
- Transport via existing cloud storage (e.g. Google Drive) or P2P like
  WebRTC. An enclave section exists in the repo; maturity unknown.
- Apache-2.0.

## Built on (foundational lineage)

None cited. Older surveys describe PySyft as focused on SMPC and HE, but
the current README doesn't mention them or differential privacy, so no
lineage is asserted; foundation intentionally empty.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Trust model | Owner-approved job execution | README |
| Differential privacy | Not mentioned in README | README |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | NOT COVERED |  | Not addressed in this file's sources |
| Trust rests on | OPERATORS | The data owner's review of submitted code and outputs, not cryptography | README, grounded |
| Confidentiality | ISOLATION | Data stays on the owner's server; jobs run in a sandbox and only approved outputs are returned | README, grounded |
| Lineage | NONE FOUND | The current README cites no SMPC, HE or DP lineage | README, grounded (as a negative finding) |
| Maturity | NOT COVERED | Current line is syft 0.10+; no scale or audit information | Not addressed in this file's sources |
| On-chain | NO CHAIN |  | Inferred from absence in the cited sources |

## Known limitations

- The current model is approval-based, so protection depends on owner
  review of code and outputs rather than cryptography.
- Setup friction (Google OAuth token per role); no performance, scale or
  audit information on the page.
