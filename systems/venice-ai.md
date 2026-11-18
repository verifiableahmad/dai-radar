---
name: Venice.ai
field: Confidential computing (TEE) for AI inference
category: private-compute
foundation: foundations/tee-remote-attestation.md
last_updated: 2026-10-27
confidence: grounded (Venice docs as summarized); vendor/partner claims, not audited
website: "https://venice.ai"
---

# Venice.ai

## What it does

A consumer and API LLM service built around privacy, with four per-model
privacy modes that make a useful ladder of trust levels.

## Architecture summary

- **Proxy layer:** relays HTTPS requests without storing prompt/response
  content for normal inference.
- **Modes:** Anonymous (identity hidden from the model provider, who may
  store data), Private (zero retention by contract), **TEE** (hardware
  isolated enclaves, "private even from Venice"), **E2EE** (prompts
  decrypted only inside a verified TEE).
- Phala supplies the confidential-computing infrastructure for TEE models
  (see `/systems/phala-dstack.md`).

## Built on (foundational lineage)

No paper cited; **inferred** link to `foundations/tee-remote-attestation.md`
via the Phala TEE stack.

## Property matrix

| Property | Current value | Source |
|---|---|---|
| Strongest mode | E2EE inside verified TEE | Venice docs |
| Weaker modes | Contractual / policy, not cryptographic | Venice docs |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | PER APP | Only the TEE and E2EE modes are attested; the others rest on policy | Venice docs |
| Trust rests on | PER APP | Enclave hardware (via Phala) in TEE/E2EE modes; contractual promises in Anonymous/Private modes | Venice docs |
| Confidentiality | PER APP | Four per-model modes, from identity-hiding proxy to E2EE inside a verified TEE | Venice docs |
| Lineage | NONE FOUND | No paper cited; TEE link inferred via the Phala stack | Venice docs, inferred link |
| Maturity | LIVE | Consumer and API LLM service; no independent security audit per a 2026 review | Venice docs |
| On-chain | NOT COVERED |  | Not addressed in this file's sources |

## Known limitations

- Anonymous and Private modes are policy promises, not verifiable.
- A third-party post claims basic telemetry (sign-in, browser, IP) is
  logged for abuse prevention; unverified against Venice's policy.
- A 2026 review notes no independent security audit and no way to prove
  every partner follows zero-retention.
