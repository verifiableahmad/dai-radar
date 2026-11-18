---
name: Tinfoil
field: Confidential computing (TEE) for AI inference
category: private-compute
foundation: foundations/tee-remote-attestation.md
last_updated: 2026-10-27
confidence: grounded (repo contents); inferred (hardware, foundation lineage)
website: "https://tinfoil.sh"
---

# Tinfoil

## What it does

Open-source confidential AI inference: workloads run in hardware enclaves
and clients verify via remote attestation that the code running matches
what is published on GitHub. Tagline: "Replacing trust with provable
security."

## Architecture summary

- Repos in the `tinfoilsh` GitHub org: `cvmimage` (confidential VM image,
  AGPL-3.0), `tinfoil-config` (workload schema/validator), `modelwrap`
  (reproducible dm-verity images of Hugging Face models),
  `encrypted-http-body-protocol` (EHBP: HPKE encryption of HTTP bodies),
  `tinfoil-proxy`, and OpenAI-compatible clients in Go, JS, Python, Swift.
- Official SDKs verify attestation and the supply chain automatically on
  each connection. V3 attestation is in beta per the docs.
- Red Hat's Office of the CTO is collaborating with the project on
  cloud-native confidential AI (Oct 2025): weights are loaded only after
  mutual attestation, with decryption confined to the enclave.

## Built on (foundational lineage)

No paper cited. Lineage to `foundations/tee-remote-attestation.md` is
**inferred**. The specific hardware (SEV-SNP / TDX / NVIDIA CC) was not
confirmed from the pages reviewed.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Verification | Client-side attestation + open-source code | Tinfoil docs |
| Transport | EHBP (HPKE) | tinfoilsh repo |
| Model integrity | dm-verity images via modelwrap | tinfoilsh repo |
| Licenses | AGPL-3.0 (core), Apache-2.0 (some clients) | tinfoilsh repo |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | ATTESTATION | Clients verify by remote attestation that the running code matches what is published on GitHub | Tinfoil docs, grounded |
| Trust rests on | HARDWARE VENDOR | Hardware enclaves; the specific hardware wasn't confirmed | Tinfoil docs; hardware inferred |
| Confidentiality | HARDWARE ENCLAVE | Inference in enclaves; HTTP bodies encrypted end to end with HPKE (EHBP) | tinfoilsh repo, grounded |
| Lineage | NONE FOUND | No paper cited; the link to Intel SGX Explained is inferred from the shared TEE-attestation mechanism | Tinfoil docs, inferred link |
| Maturity | OPEN QUESTION | V3 attestation in beta; no audit or production-deployment evidence found | Tinfoil docs; Known limitations, inferred |
| On-chain | NO CHAIN |  | Inferred from absence in the cited sources |

## Known limitations

- "Provable" is a marketing claim; the GitHub org page doesn't describe the
  transparency log or hardware details (read the attestation-architecture
  docs before upgrading this file's confidence).
- No audit or production-deployment evidence found in this pass.
