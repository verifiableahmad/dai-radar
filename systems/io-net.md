---
name: io.net
field: Decentralized GPU / compute marketplace
category: compute-markets
foundation: foundations/distributed-compute-scheduling.md
last_updated: 2026-10-20
confidence: grounded (orchestration-engine lineage); no academic grounding found (trust/verification layer)
website: "https://io.net"
---

## What it does

io.net aggregates underutilized GPUs (data centers, crypto miners,
independent providers) across many countries into an on-demand,
clusterable compute marketplace, primarily marketed at AI
training/inference workloads.

## Architecture summary

- **IO-SDK** is io.net's own orchestration layer. Its own docs state
  directly: *"IO-SDK is our specialized fork of Ray, a core technology
  driving io.net's capabilities"* and name the base version as Ray 2.3.0.
  ([io.net/docs/guides/architecture/io-architecture](https://io.net/docs/guides/architecture/io-architecture))
- Backend stack layered on top: Kubernetes/Prefect/Airflow for
  orchestration, Ray/Ludwig/PyTorch/Keras/TensorFlow for execution,
  S3/HDFS for storage, Docker for containerization, Grafana/Datadog/
  Prometheus/NVIDIA DCGM for monitoring (per io.net's own architecture
  docs).
- **Resource-integrity mechanisms** (io.net-specific, not inherited from
  Ray): a "Proof-of-Work" and "Proof of Time-Lock" (PoTL) pair that
  check a provided GPU/CPU is real and stays dedicated to the rented
  workload for its full rental period, plus a daily "Cluster
  Verification Job" standardized benchmark that workers must pass to
  stay in the active pool.
- **Confidential compute**: io.net supports NVIDIA H200-based hardware
  confidential computing, using SPDM 1.1 for device authentication,
  X.509 PKI, OCSP, and TCG measurement/attestation specifications, via
  NVIDIA's own `nv-attestation-sdk`. Recommended mode is "Protected
  PCIe" (PPCIe), which puts an entire NVLink-connected GPU set into
  confidential mode together.
  ([io.net/docs/guides/clouds/confidential-compute-attestation-overview](https://io.net/docs/guides/clouds/confidential-compute-attestation-overview))

## Built on (foundational lineage)

**Two-hop grounding for the orchestration engine, no grounding found for
the trust/verification layer:**

1. io.net's own docs name **Ray** (specifically IO-SDK as a fork of Ray
   2.3.0) as the technology underlying its clustering/task-orchestration
   capability. Direct quote above.
2. Ray is itself an academic systems project: *"Ray: A Distributed
   Framework for Emerging AI Applications"*, P. Moritz, R. Nishihara,
   S. Wang, A. Tumanov, R. Liaw, E. Liang, M. Elibol, Z. Yang, W. Paul,
   M. I. Jordan, I. Stoica (UC Berkeley), OSDI 2018.
   ([usenix.org/system/files/osdi18-moritz.pdf](https://www.usenix.org/system/files/osdi18-moritz.pdf))

See `/foundations/distributed-compute-scheduling.md`.

**Important distinction:** this lineage grounds io.net's *compute
orchestration engine only*. The parts of io.net that are actually about
decentralization/trust (PoW/PoTL resource verification, the Cluster
Verification Job, the confidential-compute attestation chain) are not
shown by io.net's own docs to descend from any specific academic paper.
They're described in terms of industry standards (SPDM, TCG, X.509) and
NVIDIA's own tooling, not a citable research lineage. Flagged as
no-academic-grounding-found rather than inferring one.

## Property matrix

| Property | Value | Source |
|---|---|---|
| Orchestration engine | IO-SDK, a fork of Ray 2.3.0 | io.net architecture docs, grounded |
| Resource-integrity check | Proof-of-Work + Proof of Time-Lock (PoTL) | io.net support docs, grounded |
| Recurring verification | Daily Cluster Verification Job (standardized benchmark) | io.net support docs, grounded |
| Confidential compute | NVIDIA H200 hardware CC, Protected PCIe mode, SPDM 1.1 + TCG attestation | io.net docs, grounded |
| Trust model (compute delivery) | Verification-by-benchmark + continuous monitoring, not cryptographic proof of correct execution | io.net docs, grounded; note this is weaker than the ZK/proof-based verification used by systems like EZKL or Gensyn |
| Maturity | Live production marketplace, actively marketed/benchmarked | io.net site, grounded |

## Comparison axes

| Axis | Class | Detail | Source |
|---|---|---|---|
| Correctness | SPOT CHECK | Proof-of-Work + Proof of Time-Lock check the GPU is real and dedicated, plus a daily benchmark; not proof of correct execution | io.net support docs, grounded |
| Trust rests on | OPERATORS | GPU providers, policed by benchmarks and continuous monitoring rather than proofs | io.net docs, grounded |
| Confidentiality | HARDWARE ENCLAVE | Supports NVIDIA H200 confidential computing (Protected PCIe mode, SPDM attestation) | io.net confidential-compute docs, grounded |
| Lineage | EXTERNAL PAPER | IO-SDK forks Ray (OSDI 2018). Grounds orchestration only, not the trust layer | io.net architecture docs, grounded |
| Maturity | LIVE | Live production marketplace | io.net site, grounded |
| On-chain | NOT COVERED |  | Not addressed in this file's sources |

## Known limitations

- The resource-verification layer (PoW/PoTL/benchmarking) checks that a
  GPU exists and stays dedicated, not that any specific computation on
  it was executed correctly, a materially weaker guarantee than
  proof-based verification (contrast with Gensyn's RepOps+bisection or
  EZKL's ZK proofs). Worth keeping this distinction visible if io.net is
  ever compared against those systems on the same axis.
- No academic citation lineage found for the decentralization/trust
  mechanisms specifically, only for the underlying Ray-based
  orchestration layer, which is a general-purpose distributed-computing
  technology, not AI- or crypto-specific.
- This entry did not independently verify PoW/PoTL implementation
  details beyond what io.net's own support docs describe at a
  conceptual level.
