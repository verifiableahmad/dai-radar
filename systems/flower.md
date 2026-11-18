---
name: Flower
field: Federated learning / confidential aggregation
category: private-compute
foundation: foundations/secure-aggregation.md
last_updated: 2026-10-27
confidence: grounded (docs name SecAgg/SecAgg+); license and paper citation not confirmed
website: "https://flower.ai"
---

# Flower

## What it does

An open federated learning framework ("a friendly federated learning
framework") for moving existing ML workloads into a federated setting.
Maintained by Flower Labs GmbH. Docs at v1.42.x.

## Architecture summary

- Components: SuperLink, SuperNodes, ServerApp, ClientApp. Deployment
  options: managed SuperLink, Docker, Helm, Slurm, SuperGrid.
- Privacy features documented: differential privacy (strategies, local-DP
  mod, privacy accountants), **SecAgg / SecAgg+** mods and a
  SecAggPlusWorkflow, TLS, SuperNode authentication, audit logging.

## Built on (foundational lineage)

The docs name SecAgg and SecAgg+ directly, so the link to
`foundations/secure-aggregation.md` (Bonawitz et al., 2017) is grounded
for the SecAgg mod; SecAgg+ is a later refinement of it by other authors,
not covered by that paper. The Flower framework paper itself was not
cited on the page reviewed.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Secure aggregation | SecAgg, SecAgg+ | Flower docs |
| Differential privacy | Strategies + local-DP mod + accountants | Flower docs |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | NOT COVERED |  | Not addressed in this file's sources |
| Trust rests on | NOT COVERED |  | Not addressed in this file's sources |
| Confidentiality | CRYPTOGRAPHIC | Raw data stays on clients; SecAgg/SecAgg+ hide individual updates (the server still sees the aggregate); DP options | Flower docs, grounded |
| Lineage | EXTERNAL PAPER | Docs name SecAgg (Bonawitz et al., 2017, Google); SecAgg+ is a later refinement by other authors | Flower docs, grounded |
| Maturity | LIVE | Open framework maintained by Flower Labs; docs at v1.42.x | Flower docs, grounded |
| On-chain | NO CHAIN |  | Inferred from absence in the cited sources |

## Known limitations

- License and limitations were not stated on the page reviewed.
- Compared to `/systems/google-confidential-federated-learning.md`, no TEE
  layer was documented: the server still sees the aggregate.
