---
name: NVIDIA Confidential Computing (H100/H200/Blackwell)
field: Confidential computing (TEE) for AI inference
category: private-compute
last_updated: 2026-10-20
confidence: grounded (standards lineage); grounded (performance, independent third-party source)
website: "https://developer.nvidia.com/blog/confidential-computing-on-h100-gpus-for-secure-and-trustworthy-ai/"
---

## What it does

Lets a cloud tenant run AI inference/training on a GPU while keeping both
the model weights and the input data encrypted in GPU memory and
protected from the cloud operator, the hypervisor, and other tenants,
extended from CPU confidential computing (already common for general
workloads) to the GPU itself, which previously sat outside the trust
boundary entirely.

## Architecture summary

- The H100 (GA April 2024) is, per NVIDIA's own developer blog, "the
  world's first native Confidential Computing GPU." Confidential mode
  ("CC-On") anchors a hardware root of trust on-die; secure boot ensures
  only verified firmware runs.
- **Attestation chain**: the GPU driver uses **SPDM** (Security Protocol
  and Data Model) for session establishment, producing a cryptographically
  signed attestation report an external verifier can check before trusting
  the GPU's state.
  ([developer.nvidia.com](https://developer.nvidia.com/blog/confidential-computing-on-h100-gpus-for-secure-and-trustworthy-ai/))
- **Does not implement a standalone GPU TEE**: it extends an existing
  *CPU* TEE's trust boundary to include the GPU. Per NVIDIA's own blog,
  it requires a CPU TEE already in place: AMD Genoa/Milan with
  **SEV-SNP** enabled, or Intel Sapphire Rapids with **TDX**. The GPU's
  DMA engine encrypts CPU↔GPU data transfer (AES-GCM-256) so the data
  stays encrypted across that boundary.
  ([developer.nvidia.com](https://developer.nvidia.com/blog/confidential-computing-on-h100-gpus-for-secure-and-trustworthy-ai/))
- Definition of "confidential computing" used throughout NVIDIA's own
  material is explicitly attributed to the **Confidential Computing
  Consortium** (an industry body, not an academic source):
  "protection of data in use by performing computation in a
  hardware-based, attested trusted execution environment (TEE), per the
  Confidential Computing Consortium."

## Built on (foundational lineage)

**No single academic seed paper: the grounding here is a set of named
industry standards, not a citation chain.** NVIDIA's own material cites:

- **SPDM** (DMTF's Security Protocol and Data Model): the attestation
  protocol used for the GPU's chain of trust.
- **Confidential Computing Consortium**'s definition of confidential
  computing: the threat model NVIDIA's design targets.
- **AMD SEV-SNP** and **Intel TDX**: the CPU TEE technologies the GPU's
  trust boundary is extended from; NVIDIA's confidential mode has no
  independent trust root of its own at the CPU level, it depends on one
  of these already being active.

This repo's `foundations/` files assume an academic-paper-shaped seed
citation (paper, authors, year, venue). Forcing that shape onto an
industry-standards lineage would misrepresent what's actually being
cited, so no `foundations/` file was created for this entry; document
the standards directly here instead, per the repo's existing rule to
flag weak/absent academic grounding rather than invent it. Worth
checking later whether the Apple Private Cloud Compute entry (also TEE
based) has the same shape; if so, both entries documenting "standards,
not papers" directly is the right pattern, not a shared paper citation.

## Property matrix

| Property | Value | Source |
|---|---|---|
| Attestation protocol | SPDM (DMTF standard) | NVIDIA developer blog, grounded |
| Trust model | Extends an existing CPU TEE (AMD SEV-SNP or Intel TDX); no independent GPU-only trust root | NVIDIA developer blog, grounded |
| CPU TEE dependency | Requires AMD Genoa/Milan+SEV-SNP or Intel Sapphire Rapids+TDX already active on the host | NVIDIA developer blog, grounded |
| Data-in-transit protection | AES-GCM-256 on the GPU's DMA engine for CPU↔GPU transfer | NVIDIA developer blog, grounded |
| Hardware availability | H100 (GA April 2024), extended in later generations (H200/Blackwell) | NVIDIA developer blog + industry reporting, grounded |
| Performance overhead | Independent study (arXiv 2505.16501): confidential mode shows 20–30% higher latency, 45–70% lower throughput, ~50% lower GPU utilization vs. non-confidential mode, attributed mainly to encrypt/decrypt overhead when loading models | [arXiv:2505.16501](https://arxiv.org/abs/2505.16501), grounded (independent third-party measurement, not NVIDIA's own figure) |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | ATTESTATION | SPDM-signed attestation report an external verifier checks before trusting the GPU's state | NVIDIA developer blog, grounded |
| Trust rests on | HARDWARE VENDOR | NVIDIA's on-die root of trust plus the host's CPU TEE (SEV-SNP or TDX); no GPU-only trust root | NVIDIA developer blog, grounded |
| Confidentiality | HARDWARE ENCLAVE | Weights and inputs encrypted in GPU memory; AES-GCM-256 on CPU-GPU transfers | NVIDIA developer blog, grounded |
| Lineage | STANDARDS | SPDM, the Confidential Computing Consortium's definition, SEV-SNP/TDX; no academic paper | NVIDIA developer blog, grounded |
| Maturity | LIVE | H100 GA April 2024, extended in H200/Blackwell; 20-30% higher latency in confidential mode per an independent study | NVIDIA developer blog + arXiv:2505.16501, grounded |
| On-chain | NO CHAIN | A hardware feature; no blockchain component | Inferred from absence in the cited sources |

## Known limitations

- Confidential mode's performance cost is substantial per the independent
  study above; this is a real adoption barrier for latency-sensitive
  inference, not a marketing footnote.
- Trust still partially rests on the CPU vendor's TEE implementation
  (SEV-SNP/TDX) and its own attestation guarantees; a GPU-level
  vulnerability in NVIDIA's implementation isn't the only thing that
  could break the model; a flaw in the underlying CPU TEE would too.
- This entry did not independently verify Blackwell-generation-specific
  changes to the attestation chain, flagged as an open follow-up.
