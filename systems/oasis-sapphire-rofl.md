---
name: Oasis (Sapphire / ROFL)
field: Confidential computing (TEE) for AI inference
category: private-compute
foundation: foundations/tee-remote-attestation.md
last_updated: 2026-10-27
confidence: grounded (Oasis docs overview); inferred (foundation lineage)
website: "https://oasis.io"
---

# Oasis (Sapphire / ROFL)

## What it does

**Sapphire** is an EVM-compatible chain with encrypted transactions and
confidential smart contracts. **ROFL** (Runtime Off-Chain Logic) runs
apps in TEEs on Oasis nodes, managed by Sapphire.

## Architecture summary

- ROFL apps ship as Docker-like containers or single executables,
  depending on the TCB and threat model wanted.
- Permissionless node pool for registration, deployment and billing; a
  built-in Key Management Service derives and stores secrets inside the TEE.
- Sapphire contracts can verify the origin of ROFL transactions.
- AI examples: a private Telegram bot running an Ollama LLM inside ROFL;
  "provable AI learning" and trustless-agent use cases listed.

## Built on (foundational lineage)

None cited. **Inferred** link to `foundations/tee-remote-attestation.md`.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| TEE | Intel SGX / TDX | Oasis ROFL docs |
| Key management | Built-in KMS in TEE | Oasis ROFL docs |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | ATTESTATION | ROFL apps run in TEEs; Sapphire contracts can verify the origin of ROFL transactions; attestation details unread | Oasis ROFL docs overview, inferred |
| Trust rests on | HARDWARE VENDOR | Intel SGX / TDX, with a key management service inside the TEE | Oasis ROFL docs, grounded |
| Confidentiality | HARDWARE ENCLAVE | Encrypted transactions and confidential contracts (Sapphire); TEE-hosted apps (ROFL) | Oasis docs overview, grounded |
| Lineage | NONE FOUND | No paper cited; the link to Intel SGX Explained is inferred from the shared TEE-attestation mechanism | Oasis docs, inferred link |
| Maturity | NOT COVERED |  | Not addressed in this file's sources |
| On-chain | OWN CHAIN | Sapphire, an EVM-compatible confidential chain | Oasis docs overview, grounded |

## Known limitations

- Overview page gives no attestation details, token/billing model, or
  limitations; read the attestation docs before raising confidence.
- GPU-based inference was not covered by the page (the examples are
  CPU-scale).
