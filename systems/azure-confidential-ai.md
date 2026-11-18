---
name: Azure Confidential AI
field: Confidential computing (TEE) for AI inference
category: private-compute
foundation: foundations/ccf-confidential-computing.md
last_updated: 2026-10-20
confidence: grounded (ledger lineage); inferred/vendor-spec (TEE hardware lineage)
website: "https://learn.microsoft.com/azure/confidential-computing/confidential-ai"
---

# Azure Confidential AI

## What it does

Microsoft's umbrella term for running AI training/inference inside
hardware-isolated Trusted Execution Environments (TEEs) on Azure, so
that neither Microsoft (the cloud operator) nor other tenants can see
model weights or input data while the workload runs, and, in the
newer "Confidential Inferencing" variant, producing an auditable record
of exactly what code ran.

**Caution:** Microsoft has published more than one architecture under
this umbrella, and they are not identical (see below); this entry
documents what was actually found for each, rather than collapsing them
into one story.

## Architecture summary

**1. Azure confidential VMs (the base layer)**: grounded directly from
Microsoft Learn (learn.microsoft.com/azure/confidential-computing/confidential-vm-overview):
- CPU TEE via AMD SEV-SNP or Intel TDX, hardware-enforced isolation from
  the hypervisor/host.
- A dedicated virtual TPM (vTPM, TPM 2.0 spec) per VM for attestation
  and key/secret storage.
- Boot-time attestation via Azure Attestation; the VM won't start if
  required isolation settings (e.g. SEV-SNP) aren't enabled.
- GPU confidential computing extension: the `NCCadsH100v5` VM series
  pairs SEV-SNP with NVIDIA H100 Tensor Core GPU confidential-computing
  mode, forming one TEE spanning CPU and GPU, with encrypted PCIe
  traffic between them.
- Secure key release: Azure Key Vault Managed HSM can be configured to
  release decryption keys only after Azure Attestation confirms the VM
  state.

**2. Azure AI Confidential Inferencing (preview, newer)**: grounded
from Microsoft's own techcommunity blog and learn.microsoft.com/azure/confidential-computing/confidential-ai:
- Inference runs inside the confidential GPU VM described above, with
  an integrity-protected disk image.
- A client obtains an HPKE public key from a KMS plus hardware
  attestation evidence and transparency evidence binding that key to
  the service's current secure-key-release policy, before sending an
  encrypted (OHTTP) request.
- An append-only **transparency ledger** records container hashes and
  deployment policies for the inference service, so what code ran is
  independently auditable. Microsoft's own docs state directly:
  **"Azure confidential ledger is built on the Confidential Consortium
  Framework (CCF)."**

**3. A third, simpler documented variant exists**:
`github.com/microsoft/confidential-ai` (inference/docs/arch.md)
describes a reference architecture using AMD SEV-SNP plus a custom
"attested TLS" extension and model-file encryption, with **no mention
of CCF, OHTTP, or a transparency ledger**. This looks like an earlier
or alternative reference design, not the same system as (2), flagged
explicitly rather than merged into one narrative, since the two
documents describe materially different mechanisms.

## Built on (foundational lineage)

Two separate lineages, with different grounding strength:

- **Transparency ledger → CCF (grounded):** Azure's own docs state the
  confidential ledger used in AI Confidential Inferencing is built on
  CCF. CCF itself is a Microsoft Research system with a direct academic
  paper; see `/foundations/ccf-confidential-computing.md`.
- **TEE hardware (vendor-spec, not an academic-paper lineage):** AMD
  SEV-SNP, Intel TDX, and NVIDIA H100 confidential computing are vendor
  hardware technologies with their own technical specifications, not
  Microsoft Research's own academic contribution. Microsoft's docs cite
  them by name (grounded: this is what Azure uses) but there is no
  single foundational *paper* here the way there is for CCF; treat any
  claim of a specific academic seed paper for the hardware TEE layer as
  unsupported unless found otherwise.

## Property matrix

| Property | Value | Source |
|---|---|---|
| CPU TEE | AMD SEV-SNP or Intel TDX | learn.microsoft.com/azure/confidential-computing/confidential-vm-overview, grounded |
| GPU TEE | NVIDIA H100 Tensor Core, confidential-computing mode (`NCCadsH100v5` series) | same source, grounded |
| Attestation | Azure Attestation (boot-time, hardware + firmware measurements); vTPM (TPM 2.0) per VM | same source, grounded |
| Key release | Azure Key Vault Managed HSM, gated on successful attestation | same source, grounded |
| Transparency/auditability | Append-only ledger (CCF-based) recording container hashes + deployment policy, **only in the AI Confidential Inferencing preview architecture**, not confirmed present in the simpler `microsoft/confidential-ai` reference design | techcommunity.microsoft.com Confidential Inferencing deep-dive, grounded for that specific architecture |
| Maturity | AI Confidential Inferencing explicitly described as **preview**, not GA, as of this research pass | techcommunity.microsoft.com, Azure AI Foundry blog, grounded |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | ATTESTATION | Azure Attestation of hardware/firmware state; Confidential Inferencing adds a ledger of container hashes | learn.microsoft.com + techcommunity deep-dive, grounded |
| Trust rests on | HARDWARE VENDOR | AMD SEV-SNP or Intel TDX CPU TEE, plus NVIDIA H100 confidential mode for GPUs | learn.microsoft.com confidential-vm-overview, grounded |
| Confidentiality | HARDWARE ENCLAVE | Neither Microsoft nor other tenants can see weights or inputs while the workload runs | learn.microsoft.com confidential-ai, grounded |
| Lineage | OWN PAPER | Transparency ledger built on CCF (Microsoft Research); the TEE hardware is vendor spec with no paper | learn.microsoft.com (Azure confidential ledger), grounded |
| Maturity | PREVIEW | AI Confidential Inferencing is preview, not GA | techcommunity.microsoft.com, Azure AI Foundry blog, grounded |
| On-chain | NO CHAIN | No public blockchain; auditability comes from the CCF-based confidential ledger | Inferred from absence in the cited sources |

## Known limitations

- Two (at least) differently-architected "confidential AI" reference
  designs are publicly documented by Microsoft with no explicit
  statement of how they relate; a future watch-run should check
  whether these converge, or whether one supersedes the other.
- Hardware TEEs as a class (SEV-SNP, TDX, any vendor) have a history of
  disclosed side-channel attacks in the broader security literature;
  this entry did not locate a specific disclosed vulnerability against
  Azure's current deployment in this research pass, so no specific
  claim is made here. Flagged as a general risk class worth a
  dedicated watch-run rather than an unsupported specific claim.
- AI Confidential Inferencing's preview status means the architecture
  (and this entry) may be out of date quickly; recheck before relying
  on specifics.
