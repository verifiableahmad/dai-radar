---
name: Phala (dstack)
field: Confidential computing (TEE) for AI inference
category: private-compute
foundation: foundations/tee-remote-attestation.md
last_updated: 2026-11-24
confidence: grounded (architecture, per dstack repo); inferred (foundation lineage)
website: "https://phala.com"
---

# Phala (dstack)

## What it does

Phala describes itself as a decentralized cloud for confidential
computing: apps run in TEEs and the code and hardware can be verified by
remote attestation. Products: Phala Cloud (deploy TEE apps), dstack
(open-source runtime), and GPU TEE for confidential AI.

## Architecture summary

- **dstack** runs Docker Compose workloads in confidential VMs. Components:
  a **Guest Agent** (generates TDX attestation quotes, provisions per-app
  keys, encrypts storage), a **KMS** in its own TEE that verifies quotes
  before releasing keys under policies defined in on-chain smart contracts,
  a **Gateway** (TLS termination, RA-TLS internally), and a **VMM** on
  bare-metal TDX hosts.
- Platforms: bare-metal Intel TDX, bare-metal AMD SEV-SNP, GCP
  Confidential VMs, AWS Nitro, and NVIDIA H100 / Blackwell confidential
  GPUs. Until 2026-11-24 the repo called SEV-SNP "new and experimental"
  and Intel TDX "the production path". PR #1641, merged to the default
  `next` branch on 2026-11-24, lists bare-metal SEV-SNP as "Available"
  next to TDX (GPU workloads included) and requires dstack OS 0.6.6 or
  later for it. The change is in the "Unreleased" changelog section;
  the latest platform tag, v0.6.6, still carries the experimental
  label. SEV-SNP in public clouds is not supported.
- Governance: dstack is a Linux Foundation Confidential Computing
  Consortium project; the repo includes a zkSecurity audit.

## Built on (foundational lineage)

No paper is cited in Phala's docs or the dstack repo. Lineage to
`foundations/tee-remote-attestation.md` is **inferred** from the shared
TEE-attestation mechanism only.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Isolation | Intel TDX or bare-metal AMD SEV-SNP CVMs (+ NVIDIA CC GPUs); SEV-SNP listed as supported on `next` since PR #1641 (2026-11-24), not yet in a tagged release | dstack README; docs/amd-sev-snp.md |
| Key release | KMS in TEE, policy on-chain | dstack README |
| Audit | zkSecurity (PDF in repo) | dstack README |
| Production users | OpenRouter, NEAR AI (as claimed by the repo) | dstack README |
| Overhead | ~2-5% CPU for TDX (general claim, not benchmarked on the page) | dstack FAQ |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | ATTESTATION | Guest agent issues TDX quotes; a KMS in its own TEE verifies them before releasing keys | dstack README, grounded |
| Trust rests on | HARDWARE VENDOR | Hardware vendors' attestation; side channels on TDX/SEV are out of scope | dstack README, grounded |
| Confidentiality | HARDWARE ENCLAVE | Workloads in confidential VMs (Intel TDX, or AMD SEV-SNP on bare metal) with NVIDIA confidential GPUs | dstack README, grounded |
| Lineage | NONE FOUND | No paper cited; the link to Intel SGX Explained is inferred from the shared TEE-attestation mechanism | dstack README, inferred link |
| Maturity | LIVE | Phala Cloud product; production users OpenRouter and NEAR AI (self-reported); zkSecurity audit in repo | dstack README, grounded |
| On-chain | OPEN QUESTION | Key-release policies live in on-chain contracts; which chain, and the network-level story, weren't verified | dstack README; network side unverified, inferred |

## Known limitations

- Trust roots in hardware vendors' attestation; side-channel attacks on
  TDX/SEV are out of scope of dstack's guarantees.
- Phala's docs index gave no token, chain, or limitations details; the
  network-level (non-dstack) story was not verified in this pass.
- Production users listed are self-reported by the repo.
- On SEV-SNP, the per-boot `gpu-attestation` event and boot-time GPU
  evidence are not bound to the SNP report, since the stack has no
  quote-bound runtime register there. The docs say a verifier that
  needs GPU protection must check that the policy bound by
  `compose_hash` keeps `attest_gpu` on and requires the expected GPU
  count. The SNP verifier does not yet check ASK and VCEK certificate
  validity periods (issue #1140). Both per docs/amd-sev-snp.md and
  docs/security/security-model.md on `next`, 2026-11-24.
