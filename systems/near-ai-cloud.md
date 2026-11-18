---
name: NEAR AI Cloud
field: Confidential computing (TEE) for AI inference
category: private-compute
foundation: foundations/tee-remote-attestation.md
last_updated: 2026-10-27
confidence: grounded (vendor docs); inferred (foundation lineage)
website: "https://docs.near.ai/cloud/private-inference/"
---

# NEAR AI Cloud

## What it does

Private LLM inference where prompts, weights and outputs are claimed to
be hidden from the cloud provider, model provider and NEAR itself.

## Architecture summary

- **Private LLM nodes:** 8x NVIDIA H200, TDX-enabled CPUs, running a
  Private-ML-SDK for secure execution, attestation and signing.
- **LLM Gateway** (`cloud-api.near.ai`): handles model registration,
  routing, attestation-report storage, API keys; itself runs in a TEE.
- **Verification:** each TEE emits evidence; gateway TLS attestation binds
  the connection endpoint to the attestation report; responses can be
  signed by an attested signer. Optional client-side E2EE.
- dstack (see `/systems/phala-dstack.md`) lists NEAR AI as a production
  user (claimed by the dstack repo).

## Built on (foundational lineage)

None cited; **inferred** link to `foundations/tee-remote-attestation.md`.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| CPU TEE | Intel TDX | NEAR AI docs |
| GPU TEE | NVIDIA confidential computing (H200) | NEAR AI docs |
| Response verification | Optional signed responses | NEAR AI docs |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | ATTESTATION | Each TEE emits evidence; gateway TLS attestation binds the endpoint; responses can be signed by an attested signer | NEAR AI docs, grounded |
| Trust rests on | HARDWARE VENDOR | Intel TDX CPUs and NVIDIA H200 confidential GPUs; the docs call attestation "evidence, not a guarantee" | NEAR AI docs, grounded |
| Confidentiality | HARDWARE ENCLAVE | Prompts, weights and outputs claimed hidden from cloud, model provider and NEAR; optional E2EE | NEAR AI docs, grounded |
| Lineage | NONE FOUND | No paper cited; the link to Intel SGX Explained is inferred from the shared TEE-attestation mechanism | NEAR AI docs, inferred link |
| Maturity | LIVE | Documented API; listed as a production user by the dstack repo (self-reported) | NEAR AI docs + dstack README, inferred |
| On-chain | NOT COVERED |  | Not addressed in this file's sources |

## Known limitations

- Direct completions endpoints skip the gateway, are experimental, and
  have known verification limits.
- Unsigned responses are not response-verified; TLS alone protects
  transit only.
- The docs state attestation is "evidence, not a guarantee"; strong
  wording ("technically impossible") is unsupported by audits or
  performance figures on the page.
